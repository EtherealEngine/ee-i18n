# Instalación en Windows 10+

1.  instale python 3 y agregue la ruta del directorio de instalación de python a la variable env 'path'.
2.  Instalar nodo js
3.  instalar Visual Studio Community Edition con herramientas de compilación. siga los siguientes pasos.
    Si mediasoup no se instala correctamente, modifique el programa de instalación de Visual Studio para
    agregar compatibilidad con c++ y Node.js.
4.  Agregar ruta de acceso a MSbuild.exe (que está presente en la carpeta de instalación de vs)
    en la variable 'path' (por ejemplo:`  C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin `)
5.  Instalar todas las dependencias mediante `npm`.
6.  Si el error persiste, compruebe si hay errores tipográficos en las variables de entorno.
7.  Si está en Windows, puede usar docker-compose para iniciar el `scripts/docker-compose.yml`
    o instale mariadb y copie el nombre de inicio de sesión/pase y de base de datos
    docker-compose o `.env.local` -- tendrá que crear la base de datos con
    el nombre coincidente, pero no es necesario rellenarlo

`./start-db.sh` solo necesita ejecutarse una vez. Si la imagen de Docker se ha detenido,
inícielo de nuevo con:

    docker container start xrengine_db

8.  Compruebe si hay configuraciones de red incorrectas en la configuración de WSL.
    https://docs.microsoft.com/en-us/windows/wsl/wsl-config#network

### Instalación en Windows con WSL2

Nota: **Debe tener WSL2 instalado para que estas instrucciones funcionen**

Primero, abra un mensaje wsl. A continuación, escriba estos comandos:

    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install build-essential
    npm install
    npm install mediasoup@3 --save
    sudo service docker start
    npm run dev-docker
    npm run dev-reinit
