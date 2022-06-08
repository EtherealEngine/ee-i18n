# Instalación en Mac OS X

1.  Ve a la raíz y ejecuta

<!---->

    npm install
    npm run dev-docker
    npm run dev-reinit

O si estás en un Mac basado en M1

(Recomendado)

1.  Duplicar la aplicación Terminal y configurarla para que se ejecute en Rosetta
2.  Ejecute lo anterior en Rosetta Terminal

(No recomendado)

    yarn install

Esto se debe a que en los chips de Apple el node-darwin a veces no se instala.
correctamente y mediante el uso de hilo, soluciona el problema.

2.  Hacer que docker se inicie en segundo plano y luego en el tipo de terminal

<!---->

    npm run dev

Esto abrirá los scripts mariaDB y SQL en el docker e iniciará los servidores

3.  Para asegurarse de que su entorno está configurado y funcionando correctamente, simplemente vaya a
    https://localhost:3000/location/default y deberías poder caminar alrededor de una escena 3D vacía

<!---->

    Note : Make sure you are on Node >= 16 and have docker running. 

## Solución de problemas de Mac

*   Si encuentra problemas en su terminal que dicen que el acceso denegado para el usuario
    `server@localhost` A continuación, puede utilizar este comando

<!---->

    brew services stop mysql

*   Si encuentra un problema en su terminal que dice
    `An unexpected error occurred: "expected workspace package`
    Mientras usa Yarn, puede utilizar este comando en su terminal

<!---->

    yarn policies set-version 1.18.0

Como hilo > 1.18 a veces no funciona correctamente con lerna.
