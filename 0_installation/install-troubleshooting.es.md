### Depuración del navegador

`p key` vista de depurar colisionadores

### Errores de certificado no válidos en el entorno local

Al momento de escribir este artículo, el certificado proporcionado en el paquete xrengine para uso local
no está debidamente firmado. Los navegadores lanzarán advertencias sobre ir a páginas inseguras.
Debería poder decirle al navegador que lo ignore, generalmente haciendo clic en algún tipo
de 'opciones avanzadas' botón o enlace y luego algo así como 'ir allí de todos modos'.

Chrome a veces no muestra una opción en la que se pueda hacer clic en la advertencia. Si es así, solo
tipo `badidea` o `thisisunsafe` cuando en esa página. No entras eso en el
barra de direcciones o en un cuadro de texto, Chrome solo escucha pasivamente esos comandos.

### Permitir la conexión de la dirección del servidor de juegos mediante la instalación de la autoridad de certificación local

Para obtener instrucciones más detalladas, consulte: https://github.com/FiloSottile/mkcert

Versión corta (común para el proceso de desarrollo en Ubuntu):

1.  `sudo apt install libnss3-tools`
2.  `brew install mkcert` (si no tienes cerveza, revisa su página: https://brew.sh/)
3.  `mkcert --install`
4.  desplácese a `./certs` carpeta
5.  mkcert -key-file key.pem -cert-file cert.pem localhost 127.0.0.1 ::1

### Permitir la conexión del servidor http de archivos locales con un certificado no válido

Abra las herramientas de desarrollo en su navegador presionando `Ctrl+Shift+i` en el
mismo tiempo. Vaya a la pestaña 'Consola' y mire el historial de mensajes. Si los hay
errores rojos que dicen algo como
`GET https://127.0.0.1:3030/socket.io/?EIO=3&transport=polling&t=NXlZLTa net::ERR_CERT_AUTHORITY_INVALID`,
luego haga clic con el botón derecho en esa URL, luego seleccione 'Abrir en una nueva pestaña' y acepte el certificado no válido.

### Permitir la conexión de la dirección del servidor de juegos con un certificado no válido

La funcionalidad del servidor de juegos está alojada en una dirección distinta de 127.0.0.1 en el local
medio ambiente. Aceptar un certificado no válido para 127.0.0.1 no se aplicará a esta dirección.
Abre la consola de desarrollo para Chrome/Firefox pulsando `Ctrl+Shift+i` simultáneamente, y
vaya a las pestañas Consola o Red.

Si ve errores acerca de no poder conectarse a
algo así como `https://192.168.0.81:3031/socket.io/?location=<foobar>`, haga clic derecho en
esa URL y ábrela en una nueva pestaña. Debería volver a recibir una página de advertencia sobre un no válido
certificado, y de nuevo debe permitirlo.

### AccessDenied conexión a mariadb

Asegúrese de que no tiene otra instancia de mariadb ejecutándose en el puerto 3306

    lsof -i :3306

En Linux, también puede verificar si algún proceso se está ejecutando en el puerto 3306 con
`sudo netstat -utlp | grep 3306`
La última columna debe tener el siguiente aspecto `<ID>/<something`
Puede matar cualquier proceso en ejecución con `sudo kill <ID>`

### Error: escuchar EADDRINUSE :::3030

Compruebe qué proceso está utilizando el puerto 3030 y kill

    killall -9 node 

O

    lsof -i :3030
    kill -3 <proccessIDfromPreviousCommand>

### 'Error CORS' en el terminal

Intente acceder a la página usando `https://localhost:3000`
En lugar de `https://127.0.0.1:3000`

### Pantalla en blanco predeterminada

Intenta escribir `“thisisunsafe”` o `"iknowwhatiamdoing"` a continuación, vuelva a cargar la página

### ¿Servidor de juegos o error de carga de recursos?

Abra la consola de desarrollo, haga clic en el enlace GET en la nueva pestaña y acepte el certificado por
Mecanografía `thisisunsafe”` o `"iknowwhatiamdoing"` a continuación, vuelva a cargar la página original

### ¿Colgar en la página giratoria?

Intente escribir lo siguiente en terminal, en el directorio /packages/server

    npm run dev-reinit-db

### Para iniciar sesión como administrador

En la herramienta de desarrollo de Chrome, escribir `userId` Esto mostrará su ID de usuario. Copia esto
ID de usuario como cadena, ejecútelo como el siguiente comando en el shell:

    npm run make-user-admin -- --id={COPIED_USER_ID}

Ejemplo

    npm run make-user-admin -- --id=c06b0210-453e-21ws-afc3-c97a57eeb1ac

### Para instalar un nuevo paquete para componentes de reacción del editor en monorepo

Escriba en el terminal

     npm i <packagename> -w @xrengine/editor

### Para rellenar la base de datos

Mate el servidor primero y luego escriba el terminal

    cd packages/server && npm run dev-reinit-db

### La base de datos no siembra rutas (por ejemplo, error: no hay proyecto instalado, póngase en contacto con el administrador del sitio)

Probar

    npm run dev-reinit 

o

    docker container stop xrengine_db
    docker container rm xrengine_db
    docker container prune
    npm run dev-docker
    npm run dev-reinit

### 'TypeError: No se puede leer la propiedad 'position' de undefined' al acceder a /location/home

Al momento de escribir este artículo, hay un error con la ubicación de prueba sembrada predeterminada.
Vaya a /editor/projects y abra el proyecto 'Test'. Guarde el proyecto, y
el error debería desaparecer.

### ¿Problemas extraños con su base de datos?

Probar

    npm run dev-reinit

o si está en Windows

    npm run dev-reinit-windows

[Siguiente =>>](running-on-static-IP.md)
