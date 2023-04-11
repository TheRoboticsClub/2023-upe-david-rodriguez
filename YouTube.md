### [Volver al Readme principal.][]

[Volver al Readme principal.]: ../README.md

# Unibotics WebServer - Grabación, guardado y subida de videos a YouTube

Documentos del torneo automático

Pasos para ejecutar la grabación, guardado y subida de videos

- [Información sobre las tecnologias usadas](#información-sobre-las-tecnologias-usadas)
- [Archivos para la grabación](#archivos-para-la-grabación)
- [Primeros pasos para grabar y subir videos](#primeros-pasos-para-grabar-y-subir-videos)

<a name="información sobre las tecnologias usadas"></a>
## Información sobre las tecnologias usadas

Se han hecho uso de las siguientes herramientas:

### Google Cloud

Google Cloud es una plataforma para la creación y desarrollo de servicios que usan la tecnología de Google para funcionar, en nuestro caso será necesaria para acceder a la API de Youtube para poder automatizar la subida de videos a esta plataforma.

### Obs

Obs es un programa de grabación y retrasmisión de contenido audiovisual. Se usará este programa para la grabación de los videos automaticamente. Los controles para la grabación se harán a traves de la página web http://obs-web.niek.tv.

### YouTube

Será el lugar donde subiremos los videos grabados.

<a name="archivos-para-la-grabación"></a>
## Archivos para la grabación

### YouTube.md

Documentación donde se detallan todos los aspectos de la configuración y despliegue de esta tecnología.

### Run.py y TorneoAutomatico.py

Scripts donde se almacenan las instrucciones de control ejecutables.

### Carpeta videosGrabados

Carpeta donde el programa de OBS almacenará los videos recién grabados.

### Carpeta videosGuardados

Carpeta que almacenará los videos una vez estos hayan sido cambiados de nombre, además de los archivos "upload_video.py", "client_secrets.json" y "upload_video.py-oauth2.json".

### upload_video.py

Script que se encargará de subir el video a YouTube.

### client_secrets.json y upload_video.py-oauth2.json

Archivos json que incluirán información **SENSIBLE e IMPORTANTE** de la cuenta de Google asociada.

<a name="Primeros pasos para grabar y subir videos"></a>
## Primeros pasos para grabar y subir videos

En primer lugar, antes de configurar Obs y la cuenta de Google para poder subir los videos se deberá tener un Desplieuge D1 operativo, más información en [Despliegues.]: ./docs/Despliegues.md

### Configuración de OBS

Deberemos ajustar la salida de la grabación de videos de OBS de esta forma:

En primer lugar, grabar en mp4 y en la carpeta de videosGrabados
- Entrar en Ajustes y en el apartado de Salida, seleccionar Grabación
- De este lugar solo cambiaremos la Ruta de la Grabación poniendo donde se encuentre la carpeta de videosGrabados de nuestro proyecto. Además de eso cambiaremos el Formato de Grabación a mp4.

Activar los websockets
- En la pestaña de Herramientas entrar en Ajustes de obs-websockets
- Aceptar la opción de Habilitar servidor Websocket
- Copiar los datos de la información de la conexión (ip:puerto y contraseña) en las líneas:

```
TorneoAutomatico.py / Línea 646

self.startRecord("ws://192.168.1.36:4444", "fWN24QpQuPWdLeO6")
```

```
Estas no serán necesarias pero son recomendables al ser la misma información
Run.py / Líneas 13 y 58

bot.startStreaming("ws://192.168.1.36:4444","fWN24QpQuPWdLeO6")
                        "ws://[ip]:[port]","[password]" (if password is null use "")

bot.stopStreaming("ws://192.168.1.36:4444", "fWN24QpQuPWdLeO6")

-----------------------------------------------------------------------

TorneoAutomatico.py / Línea 58

self.host = env.str("ws://192.168.1.36:4444", "fWN24QpQuPWdLeO6")
```

### Configuración de la API

Estos pasos estarán mejor explicados en el tutorial de: https://www.youtube.com/watch?v=aFwZgth790Q

En primer lugar necesitaremos configurar una cuenta de Google y un proyecto para poder utilizar la API de Youtube.

En primer lugar entrar en https://console.cloud.google.com y crear un nuevo proyecto con el nombre que queramos y sin organización.

Nos iremos al apartado de APIs y servicios y habilitaremos "YouTube Data API v3", esta será la API que nos permitirá subir videos a youTube.

Ir al apartado de credenciales y crear una nueva credencial de tipo "OAuth Client ID", configurar la pantalla de consentimiento con un usuario de tipo externo. Rellenar la infromacion necesaria.

En la pestaña de Scopes darle acceso al manejo (upload) de videos. En la pestaña de Test Users introducir el usuario de la cuenta propia de google que utilicemos.

Despues de configurar las credenciales compatibles deberemos crear unas nuevas credenciales de "ID de cliente de OAuth" con una "App de escritorio" como Tipo de aplicación.

### Configuración en el proyecto

Crear un nuevo archivo en el proyecto llamado "client_secrets.json" en el directorio de videosGuardados y rellenarlo de estos datos:

```
{
  "web": {
    "client_id": "[[INSERT CLIENT ID HERE]]",
    "client_secret": "[[INSERT CLIENT SECRET HERE]]",
    "redirect_uris": [],
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://accounts.google.com/o/oauth2/token"
  }
}
```
Insertando en el client_id y en client_secret las credenciales que acabamos de crear en la API

### Primera ejecución

Ya debería estar todo listo para la primera ejecución del código. Se aconseja que antes de poner en marcha todo el mecanismo de simulación y grabado se pruebe a solo subir a YouTube un video concreto almacenado en la carpeta de videosGuardados. 

En la primera ejecución del código, en caso de ser una cuenta personal seguramente te pida que introduccas de nuevo tu contraseña y en caso de esa cuenta estar conectada con una segunda autentificación te pedirá aceptar la aplicación no solo desde el ordenador sino desde un móvil conectado personal.

Después de esta primera ejecución se creará un archivo llamado "upload_video.py-oauth2.json" con información sensible y que, al igual que "client_secrets.json" no deberá ser publicado con nadie.

Posteriormente se podrá utilizar el script sin deber darle acceso de forma manual.

### Limitaciones

Los títulos personalizados de los videos que se suban utilizando este método no podrán tener espacios. Lo mismo ocurrirá con las descripciones de tales videos.

Solo podrán subirse un máximo de 6 videos diarios antes de que la quota por defecto  de YouTube deniegue la operación. https://developers.google.com/youtube/v3/getting-started?hl=es-419#quota