# Configuración avanzada

Si desea configurar instancias de Docker de XREngine, cliente, servidor y/o
game-server manualmente, siga estas instrucciones. Se recomienda la configuración avanzada
para todos los usuarios, con el fin de entender más sobre todo lo que está pasando.

### 1. Instala tus dependencias

    cd path/to/xrengine
    npm install
    npm run dev-docker
    npm run dev-reinit

No debería necesitar usar sudo en ningún caso.

¿Error con mediasoup? https://mediasoup.org/documentation/v3/mediasoup/installation/

### 2. Asegúrese de tener una base de datos mysql instalada y en ejecución: nuestra recomendación es Mariadb.

Hemos proporcionado un contenedor docker para una fácil configuración:

    cd scripts && sudo bash start-db.sh

Esto crea un contenedor Docker de mariadb denominado xrengine_db. Debes tener
docker instalado en su máquina para que este script funcione.
Si no tiene Docker instalado y no desea instalarlo, tendrá
para crear manualmente un servidor MariaDB.

El nombre de usuario predeterminado es 'servidor', la contraseña predeterminada es 'contraseña', el
el nombre predeterminado de la base de datos es 'xrengine', el nombre de host predeterminado es '127.0.0.1', y
El puerto predeterminado es `3306`.

¿Ve errores al conectarse a la base de datos local? **Intente apagar el firewall local.**

### 3. Abra una nueva pestaña e inicie el sidecar Agones en modo local

    cd scripts
    sudo bash start-agones.sh

También puede ir a proveedor / agones / y ejecutar

`./sdk-server.linux.amd64 --local`

Si está utilizando una máquina Windows, ejecute

`sdk-server.windows.amd64.exe --local`

y para mac, ejecute

`./sdk-server.darwin.amd64 --local`

### 4. Inicie el servidor en modo semilla de base de datos

Es necesario sembrar varias tablas de la base de datos con valores predeterminados.
Correr `npm run dev-reinit` o si está en Windows `npm run dev-reinit-windows`
Después de varios segundos, no debería haber más registro.
Algunas de las líneas finales deben decir así:

    Server Ready
    Executing (default): SET FOREIGN_KEY_CHECKS = 1
    Server EXIT

En este punto, la base de datos ha sido sembrada.

### 5. Configuración del servidor de archivos local

Si el archivo .env.local que tiene tiene la línea
`STORAGE_PROVIDER=local`
luego, el editor de escenas guardará componentes, modelos, escenas, etc. localmente
(en lugar de almacenarlos en S3). Deberá iniciar un servidor local
Para servir estos archivos y asegúrese de que .env.local tiene la línea
`LOCAL_STORAGE_PROVIDER="localhost:8642"`.
En una pestaña nueva, vaya a `packages/server` y ejecutar `npm run serve-local-files`.
Esto se pondrá en marcha `http-server` Para servir archivos desde `packages/server/upload`
en `localhost:8642`.
Es posible que tenga que aceptar el certificado autofirmado no válido en el navegador;
consulte 'Permitir la conexión del servidor http de archivos locales con un certificado no válido' a continuación.

### 6. Abra dos o tres pestañas separadas e inicie el servidor API, el servidor de juegos y el cliente

En /packages/server, ejecute `npm run dev` que iniciará el servidor api, el servidor de juegos y el servidor de archivos.
Si no está utilizando servidores de juegos, puede ejecutar `npm run dev-api-server` en el servidor API.
En la pestaña final, vaya a /packages/client y ejecute `npm run dev`.
Si está en Windows, debe usar `npm run dev-windows` En lugar de `npm run dev`.

### 7. En un navegador, navegue hasta https://127.0.0.1:3000/location/default

El proceso de siembra de la base de datos crea una ubicación vacía predeterminada denominada 'predeterminada'.
Se puede navegar yendo a 'https://127.0.0.1:3000/location/default'.
Al momento de escribir este artículo, el certificado proporcionado en el paquete XREngine para uso local es
no está debidamente firmado. Puede crear certificados firmados y reemplazar el
predeterminados, pero la mayoría de los desarrolladores simplemente ignoran las advertencias. Los navegadores lanzarán
hasta advertencias sobre ir a páginas inseguras. Debería poder decirle al navegador
para ignorarlo, generalmente haciendo clic en algún tipo de botón de 'opciones avanzadas' o
enlace y luego algo así como 've allí de todos modos'.
