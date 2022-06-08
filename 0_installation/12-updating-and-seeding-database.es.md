## Actualización de modelos de bases de datos

Las herramientas para actualizar automáticamente la base de datos en función de los cambios en los modelos se incluyen en
[sequelize.ts](../packages/server-core/src/sequelize.ts). La mayor parte se controla mediante la configuración
la variable de entorno `PREPARE_DATABSE=true`.

Si eso se establece, entonces la configuración de la base de datos iterará a través de los campos de cada modelo e intentará
para hacer coincidir cada uno con una columna. Si no puede encontrar una columna existente, sucederá una de dos cosas:

*   Si el modelo tiene un valor `oldColumn` establecer en la definición de campo/foreignKey, y que antiguo
    existe la columna, a continuación, se cambiará el nombre de la columna anterior al nombre actual del campo del modelo

El siguiente es un ejemplo de [usuario.model.ts](../packages/server-core/src/user/user/user.model.ts)
donde el campo `inviteCode` se le cambiará el nombre a `codeInvite`, y la asociación que se llamó
`channelInstanceId` se cambia el nombre a `instanceChannelId`. Tenga en cuenta el `oldColumn` definición en cada uno.

    import { DataTypes, Sequelize, Model } from 'sequelize'
    import { Application } from '../../../declarations'
    import { UserInterface } from '@xrengine/common/src/dbmodels/UserInterface'

    /**
     * This model contain users information
     */
    export default (app: Application) => {
      const sequelizeClient: Sequelize = app.get('sequelizeClient')
      const User = sequelizeClient.define<Model<UserInterface>>(
        'user',
        {
          id: {
            type: DataTypes.UUID,
            defaultValue: DataTypes.UUIDV1,
            allowNull: false,
            primaryKey: true
          },
          name: {
            type: DataTypes.STRING,
            defaultValue: (): string => 'Guest #' + Math.floor(Math.random() * (999 - 100 + 1) + 100),
            allowNull: false
          },
          avatarId: {
            type: DataTypes.STRING,
            defaultValue: (): string => '',
            allowNull: false
          },
          codeInvite: {
            type: DataTypes.STRING,
            oldColumn: 'inviteCode',
            unique: true
          }
        },
        {
          hooks: {
            beforeCount(options: any): void {
              options.raw = true
            }
          }
        }
      )

      ;(User as any).associate = (models: any): void => {
        ;(User as any).belongsTo(models.user_role, { foreignKey: 'userRole' })
        ;(User as any).belongsTo(models.instance, { foreignKey: { allowNull: true } }) // user can only be in one room at a time
        ;(User as any).belongsTo(models.instance, { foreignKey: { name: 'instanceChannelId', oldColumn: 'channelInstanceId', allowNull: true } })
        ;(User as any).hasOne(models.user_settings)
        ;(User as any).belongsTo(models.party, { through: 'party_user' }) // user can only be part of one party at a time
        ;(User as any).belongsToMany(models.user, {
          as: 'relatedUser',
          through: models.user_relationship
        })
        ;(User as any).hasMany(models.user_relationship, { onDelete: 'cascade' })
        ;(User as any).belongsToMany(models.group, { through: 'group_user' }) // user can join multiple orgs
        ;(User as any).hasMany(models.group_user, { unique: false, onDelete: 'cascade' })
        ;(User as any).hasMany(models.identity_provider, { onDelete: 'cascade' })
        ;(User as any).hasMany(models.static_resource)
        // (User as any).hasMany(models.subscription);
        ;(User as any).hasMany(models.channel, { foreignKey: 'userId1', onDelete: 'cascade' })
        ;(User as any).hasMany(models.channel, { foreignKey: 'userId2', onDelete: 'cascade' })
        // (User as any).hasOne(models.seat, { foreignKey: 'userId' });
        ;(User as any).belongsToMany(models.location, { through: 'location_admin' })
        ;(User as any).hasMany(models.location_admin, { unique: false })
        ;(User as any).hasMany(models.location_ban)
        ;(User as any).hasMany(models.bot, { foreignKey: 'userId' })
        ;(User as any).hasMany(models.scope, { foreignKey: 'userId' })
        ;(User as any).belongsToMany(models.instance, { through: 'instance_authorized_user' })
        ;(User as any).hasOne(models.user_api_key)
      }

      return User
    }

*   Si el modelo no tiene `oldColumn`, o lo hace y la columna antigua no existe, entonces
    agregará una nueva columna

Si la columna existe, el modelo actual para esa columna se aplica a la columna

## Siembra

La siembra ocurre si cualquiera de los dos `FORCE_DB_REFRESH=true` o `PREPARE_DATABASE=true`. Cada plantilla semilla
se repite en iteración y se comprueba la base de datos para ver si existe ese valor de plantilla. Plantillas de semillas
con los ID se comprueban a través de un singular `.get(<id>)`, las plantillas de semillas sin un ID se comprueban a través de un
`.find({ query: <template> })` (las tablas de configuración solo verifican si hay una fila presente y asumen
que su presencia es indicativa de haber sido ya sembrada). Si no se encuentra una plantilla de semilla,
luego se inserta en la base de datos.
