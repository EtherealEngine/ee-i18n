Siga estos pasos para ejecutar el motor en una IP estática en lugar de localhost. En
En la mayoría de los casos, debería poder acceder simplemente al motor utilizando la IP pública
asignado a su dispositivo, pero si tiene algún problema o si se está ejecutando
la pila en WSL2 luego puede consultar las siguientes instrucciones.

### Ejecución en IP estática bajo WSL2

1.  Reemplace todos los valores de localhost con la IP estática en la que desea ejecutar la pila
    en el archivo .env.local.
2.  Abra un terminal de PowerShell como administrador. Y ejecute wsl2-port-forwarding.ps1
    script presente en el directorio /scripts.
    Nota: Asegúrese de que todos los puertos necesarios estén presentes en la matriz de puertos de la
    Script wsl2-port-forwarding.ps1.
3.  Y ahora simplemente haga funcionar el motor como lo haría normalmente y todo debería ser
    accesible a través de la IP estática.
4.  Si recibe algún error relacionado con localhost:8642, asegúrese de que ninguno de
    los activos de la escena se han guardado en la ruta de acceso localhost. Si los hay entonces
    reemplace localhost con la IP estática en la ruta del activo respectivo también.
