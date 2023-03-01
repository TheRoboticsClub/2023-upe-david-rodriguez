### [Volver al Readme principal.][]

[Volver al Readme principal.]: ../README.md

# Unibotics WebServer - Torneos automaticos

Documentos del torneo automatico

Primeros pasos para ejecutar un torneo automatico

- [Información sobre las tecnologias usadas](#información-sobre-las-tecnologias-usadas)
- [Archivos para la creación de un torneo automático](#Archivos-para-la-creación-de-un-torneo-automático)
- [Primeros pasos para ejecutar un torneo automático](#Primeros-pasos-para-ejecutar-un-torneo-automático)

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

Obs es una aplicación de código abierto preparada para la grabación y trasmisión de video por internet. El torneo automático usará este programa para trasmitir en video la evaluación del código de los participantes en los torneos.

### SQL y ElasticSearch

Servicios de búsqueda e indexación de datos en bases de datos. Ambos almacenados en contenedores Docker para el guardado de datos de los usuarios como nombres y contraseñas así como de el código, puntuación y nombre de los participantes o rankings de los torneos.

<a name="Archivos para la creación de un torneo automático"></a>
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

## Primeros pasos para ejecutar un torneo automatico

Para poder ejecutar un torneo en local se debe haber podido llevar a cabo un despliegue D1, más información en [Despliegues]: https://github.com/JdeRobot/unibotics-webserver/blob/master/docs/Despliegues.md

En primer lugar asegurarse que el programa de docker está abierto y que se tienen los contenedores de bd_container (mysql), AcademyElastic (elasticSearch) y jderobotRADI (RADI ejercicios). En caso de no tenerlos, buscar las imagenes de mysql, elasticsearch:7.11.1 y robotics_academy:latest

En caso de querer utilizar el obs para poder trasmitir en directo el torneo se deberá cambiar el nombre del programa por obs64. Esto es debido a que el comando en cmd para abrir el programa no admite nombres con espacios. Por defecto este programa estará accesible para cambiar el nombre desde:
```C:\ProgramData\Microsoft\Windows\Start Menu\Programs\OBS Studio```

Se debe asegurar de que se dispone de Firefox, ya que selenium utilizará este navegador.

#### Antes de intentar ejecutar directamente, cambiar la dirección almacenada:

Cambiar la dirección donde está almacenado el repositorio en el ordenador local: Por defecto se encuentra alojado en *D:github unibotics\unibotics-webserver*, cambiar este directorio a la dirección donde se encuentre en la maquina local.

```
RunServer.ps1 / Línea 4:

cd 'D:\github unibotics\unibotics-webserver'
```

