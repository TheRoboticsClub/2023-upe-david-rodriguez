### [Volver al Readme principal.][]

[Volver al Readme principal.]: ../README.md

# Unibotics WebServer - Torneos automaticos

Documentos del torneo automatico

Primeros pasos para ejecutar un torneo automatico

- [Información sobre las tecnologias usadas](#información-sobre-las-tecnologias-usadas)
- [Archivos para la creación de un torneo automático](#archivos-para-la-creación-de-un-torneo-automático)
- [Primeros pasos para ejecutar un torneo automático](#primeros-pasos-para-ejecutar-un-torneo-automático)

<a name="información sobre las tecnologias usadas"></a>
## Información sobre las tecnologias usadas

Se ha hecho uso de las siguientes herramientas externas:

### Selenium version 4.1.2

Selenium es una herramienta de código abierto disponible en varios lenguajes de programación en forma de paquetes que permite al programador emular interacciones con navegadores web. En concreto esta herramienta nos servirá para acceder desde el código python a un navegador y simular un usuario de rol "Maestro de ceremomias" que se encargue de obtener en código escrito por los participantes de un evento, evaluarlo y almacenar su puntuación en forma de ranking.

### Docker

Docker es un proyecto de código abierto que automatiza el despliegue de aplicaciones dentro de contenedores de software. Este programa nos servirá para almacenar la información y bases de datos necesarias para la ejecución del servidor y los ejercicios.

### Django

Django es un framework de desarrollo web de codigo abierto escrito en Python que permite la creación y gestión de páginas web. Nosotros usaremos el servidor creado con Django para poder lanzar los ejercicios de forma local.

### Obs

Obs es una aplicación de código abierto preparada para la grabación y trasmisión de video por internet. El torneo automático usará este programa para trasmitir en video la evaluación del código de los participantes en los torneos. También se usará la pagina web de http://obs-web.niek.tv para controlar el inicio del stream.

### SQL y ElasticSearch

Servicios de búsqueda e indexación de datos en bases de datos. Ambos almacenados en contenedores Docker para el guardado de datos de los usuarios como nombres y contraseñas así como de el código, puntuación y nombre de los participantes o rankings de los torneos.

<a name="archivos-para-la-creación-de-un-torneo-automático"></a>
## Archivos para la creación de un torneo automático

Todos ellos contenidos en la carpeta [./jderobot_academy/academy/raul]: ../unibotics-webserver/jderobot_academy/academy/raul

### TournamentDoc.md

Documentación donde se detallan todos los aspectos de la creación y utilización de esta herramienta

### RunServer.ps1

Un programa de Powershell preparado para abrir los contenedores docker necesarios además de iniciar y actuar como servidor. Este programa está pensado para sustituir un despliegue d1.

### Run.py

Contiene las instrucciones de control que podrán ejecutarse. Es el script que se ejecutará para el uso de los torneos automáticos

### TorneoAutomatico.py

Librería local que almacena todas las instrucciones que podrán ejecutarse desde el Run.py

<a name="primeros-pasos-para-ejecutar-un-torneo-automático"></a>
## Primeros pasos para ejecutar un torneo automatico

Para poder ejecutar un torneo en local se debe haber podido llevar a cabo un despliegue D1, más información en [Despliegues.]: ./docs/Despliegues.md

En primer lugar asegurarse que el programa de docker está abierto y que se tienen los contenedores de bd_container (mysql), AcademyElastic (elasticSearch) y jderobotRADI (RADI ejercicios). En caso de no tenerlos, buscar las imagenes de mysql, elasticsearch:7.11.1 y robotics_academy:latest

Se debe asegurar de que se dispone de Firefox, ya que selenium utilizará este navegador.

#### Antes de intentar ejecutar directamente, cambiar la dirección almacenada:

Cambiar la dirección donde está almacenado el repositorio en el ordenador local: Por defecto se encuentra alojado en *D:github unibotics\unibotics-webserver*, cambiar este directorio a la dirección donde se encuentre en la maquina local.

```
RunServer.ps1 / Línea 4:

cd 'D:\github unibotics\unibotics-webserver'
```

### Si se quiere hacer stream, cambiar la direccion del directo y configurar obs

Cambiar el usuario que va a estremear el torneo en TorneoAutomatico.html. Por defecto, el usuario será miniesda:

```
TorneoAutomatico.html / Línea 147:

channel: "miniesda"
```
En caso de querer utilizar el obs para poder trasmitir en directo el torneo se deberá cambiar el nombre del programa por { **obs64** }. Esto es debido a que el comando en cmd para abrir el programa no admite nombres con espacios. Por defecto este programa estará accesible para cambiar el nombre desde:
```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\OBS Studio
```
Dentro del obs, en el panel de herramientas buscar la opción de ajustes de obs-websocket y activar el servidorWebsocket y configurar el servidor al puerto 4444:
![image](https://user-images.githubusercontent.com/63803821/222417486-17262aa1-d849-43e4-bec4-0e8fa901901b.png)

Es recomendable generar una contraseña para el servidor y aplicar. Antes de cerrar utilizar el botón de mostrar información de conexión para copiar la IP del servidor y contraseña en el archivo Run.py: 
```
Run.py / Línea 13

bot.startStreaming("ws://192.168.1.36:4444","fWN24QpQuPWdLeO6")
                         "ws://[ip]:[port]","[password]" (if password is null use "")
```
También comentar los métodos de openObs() en la línea 10 y StartStreaming() en la línea 13.

### Asegurarse que existe un usuario con el rango de maestro

Para poder acceder a la pantalla de torneos, el usuario debe ser un Maestro de Ceremonias. En caso contrario solo se accederá a la pantalla de ejercicios. En caso de ser admin si que se podrán ver los torneos desde la pantalla de academy/index, pulsando sobre el botón de Ver Torneos, además de poder unirse a ellos si todavía se puede.

Una vez que se tiene un usuario maestro, se deberá introducir sus datos en el script de TorneoAutomatico.py:
```
TorneoAutomatico.py / Línea 53:

self.nombreUsuario = env.str("CEREMONY_MASTER", "masterchief")
self.contrasena = env.str("CEREMONY_PASSWORD", "david2001ppg")
```

### Asegurarse del ejercicio del torneo

Por defecto el ejercicio que se va a evaluar será el de follow_line, en caso de querer evaluar otro, cambiarlo en Run.py / Línea 44

### Asegurar la id del torneo que se quiere evaluar y descomentar la evaluación

Para cambiar el id del torneo que se querrá evaluar, se deberá cambiar el número de las funciones de getUsersNames(id) y playUsersCodes(id).

**Además de esto descomentar ambas funciones para evaluar el código de los usuarios que participen**
