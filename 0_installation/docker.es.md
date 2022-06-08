# Instrucciones antiguas de Docker

Puede iniciar rápidamente localmente mediante docker, si no tiene un nodo instalado o
solo quiero probar lo último.

## Obtener la dirección IP local

Utilice una herramienta como ifconfig para obtener su dirección IP local.

## Iniciar bases de datos locales

```bash
cd scripts
docker-compose up
```

Cuando se detiene el registro, eso indica que las bases de datos se han creado y
están corriendo.

Ctrl + c fuera de eso, luego desde scripts se ejecutan `./start-all-docker.sh`
(Esto debe ejecutarse cada vez que inicie su máquina de nuevo)

## Crear la imagen

Crear una carpeta vacía en la raíz llamada `project-package-jsons` y la carrera
el siguiente comando para compilar:

```bash
DOCKER_BUILDKIT=1 docker build -t xrengine --build-arg MYSQL_USER=server \
  --build-arg MYSQL_PASSWORD=password --build-arg MYSQL_HOST=127.0.0.1 \
  --build-arg MYSQL_DATABASE=xrengine --build-arg MYSQL_PORT=3304 \
  --build-arg VITE_SERVER_HOST=localhost --build-arg VITE_SERVER_PORT=3030 \
  --build-arg VITE_GAMESERVER_HOST=localhost --build-arg VITE_GAMESERVER_PORT=3031 \
  --build-arg VITE_LOCAL_BUILD=true --build-arg CACHE_DATE="$(date)" --network="host" .
```

## Ejecute el servidor para sembrar la base de datos, espere un par de minutos y, a continuación, elimínela

```bash
docker run -d --name server --env-file .env.local.default -e "SERVER_MODE=api" -e "FORCE_DB_REFRESH=true" --network host xrengine
docker logs server -f
-Wait for the line "Server Ready", then Ctrl+c out of the logs-
docker container stop server
docker container rm server
```

## Ejecutar las imágenes

```bash
docker run -d --name serve-local --env-file .env.local.default -e "SERVER_MODE=serve-local" --network host xrengine
docker run -d --name server --env-file .env.local.default -e "SERVER_MODE=api" -e "GAMESERVER_HOST=<local IP address" --network host xrengine
docker run -d --name client --env-file .env.local.default -e "SERVER_MODE=client" --network host xrengine
docker run -d --name world --env-file .env.local.default -e "SERVER_MODE=realtime" -e "GAMESERVER_HOST=<local IP address>" --network host xrengine
docker run -d --name channel --env-file .env.local.default -e "SERVER_MODE=realtime" -e "GAMESERVER_HOST=<local IP address>" -e "GAMESERVER_PORT=3032" --network host xrengine
```

## Elimine los contenedores, si desea ejecutar una nueva compilación, o simplemente deshacerse de ellos

```bash
docker container stop serve-local
docker container rm serve-local
docker container stop server
docker container rm server
docker container stop client
docker container rm client
docker container stop world
docker container rm world
docker container stop channel
docker container rm channel
```
