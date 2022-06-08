# Instalación

Ponerse en marcha requiere solo unos pocos pasos, pero esto puede ser complicado,
dependiendo de su plataforma y entorno actual. Por favor, siga las instrucciones
para su entorno.

## Lista de comprobación previa a la instalación

*   \[ ] Clonar el repositorio
*   \[ ] Instalar Node.js v16 o posterior
*   \[ ] Instalar Python >=3.6 + [PEPITA](https://pypi.org/project/pip/), C++, y
    otras herramientas de compilación. Ver el [Instrucciones de instalación de Mediasoup](https://mediasoup.org/documentation/v3/mediasoup/installation/)
    para más detalles.
*   \[ ] Instalar Docker (opcional pero muy recomendable)
*   \[ ] (Opcional) Si NO está utilizando docker, instale MariaDB y Redis.

Ahora debería estar listo para seguir el [Inicio rápido](#quick-start) instrucciones.

### Clonar el repositorio

Mucho ha cambiado durante el desarrollo, y nuestro monorepo se ha vuelto bastante grande.
Para evitar clonar todo, use este comando:

    git clone https://github.com/XRFoundation/XREngine --depth 1

### Asegúrese de que está en el Nodo 16 o superior

Tú **mosto** tener instalado el nodo 16 o superior.

Un administrador de versiones puede ser útil para esto:

*   Solo NodeJS: [NVM](https://github.com/nvm-sh/nvm)
*   Políglota: [ASDF](https://github.com/asdf-vm/asdf)

Antes de hacer funcionar el motor, compruebe `node --version`
Si está utilizando una versión de nodo inferior a 16, actualice o nada funcionará.
Sabrá que tiene problemas si intenta instalar en la raíz y
obtener errores de dependencia.

### Docker es tu amigo

No es necesario que lo uses [Estibador](\(https://docs.docker.com/\)), pero hará
tu vida mucho más fácil.
Si no desea usar Docker, deberá configurar mariadb y redis en
su máquina. Puede encontrar credenciales en `xrengine/scripts/docker-compose.yml`

## Inicio rápido

Si tienes suerte, esto simplemente funcionará. Sin embargo, puede encontrar algunos
cuestiones. Asegúrese de que está ejecutando node 16 y compruebe sus dependencias.

    cd path/to/xrengine
    cp .env.local.default .env.local
    npm install
    npm run dev-docker
    npm run dev-reinit
    npm run dev

### Configurar Elastic Search & Grafana

Elastic Search y Grafana se iniciarán automáticamente con `npm run dev`.

Elasticsearch y Grafana se ejecutarán en los puertos localhost 9200 y 5601 respectivamente.

Esto configurará y ejecutará automáticamente Redis/MariaDB docker
contenedores e instancias de cliente/servidor/servidor/servidor de juegos de XRengine.

En un navegador, vaya a https://127.0.0.1:3000/location/default

El proceso de siembra de la base de datos crea una ubicación vacía de prueba llamada 'prueba'.
Se puede navegar yendo a 'https://127.0.0.1:3000/location/default'

Al momento de escribir este artículo, el certificado proporcionado en el paquete xrengine para uso local es
no está debidamente firmado. Los navegadores lanzarán advertencias sobre ir a inseguro
Páginas. Debería poder decirle al navegador que lo ignore, generalmente haciendo clic en
en algún tipo de botón o enlace de 'opciones avanzadas' y luego algo a lo largo del
líneas de 've allí de todos modos'.

Chrome a veces no muestra una opción en la que se pueda hacer clic en la advertencia. Si es así, solo
tipo `badidea` o `thisisunsafe` cuando en esa página. No entras
que en la barra de direcciones o en un cuadro de texto, Chrome solo está escuchando pasivamente
para esos comandos.

### Sistema de administración y configuración de usuario

Puede administrar muchas funciones desde el panel de administración en https://localhost:3000/admin

Cómo convertir a un usuario en administrador:

Crear un usuario en `https://localhost:3000/`

Para localizar su SEUDÓNIMO:
En la consola de Chrome Dev Tools, escribe `copy(userId)`. Esto copiará su ID de usuario
(Como se muestra en la captura de pantalla adjunta). Péguelo y ejecute el siguiente comando en
un shell 'nix (por ejemplo, Bash, ZSH):

`npm run make-user-admin -- --id={COPIED_USER_ID}`

Ejemplo:
`npm run make-user-admin -- --id=c06b0210-453e-11ec-afc3-c57a57eeb1ac`

![image](https://user-images.githubusercontent.com/43248658/142813912-35f450e1-f012-4bdf-adfa-f0fa2816160f.png)

2.  TODO: Mejore con el soporte de ID de correo electrónico / teléfono

Método alternativo:

1.  Busque en la tabla Usuario y cambie userRole a 'admin'
2.  Las credenciales de la base de datos de desarrollo se pueden encontrar aquí: packages/ops/docker-compose-local.yml#L42
3.  Sugerido: beekeeperstudio.io

Pruebe los privilegios de administrador del usuario yendo a `/admin`

## Instalación avanzada y solución de problemas

Si tiene algún problema con las instrucciones de inicio rápido:

*   Por favor, asegúrese de haber seguido el
    [Instrucciones de instalación de Mediasoup](https://mediasoup.org/documentation/v3/mediasoup/installation/)
*   Consulta las instrucciones específicas de tu sistema operativo:
    *   [Instalación en Windows (10+)](windows.md)
    *   [Instalación en Mac OS X](mac_os_x.md)
*   [Solución de problemas de instalación](install-troubleshooting.md)
*   [Configuración avanzada](advanced_setup.md)
