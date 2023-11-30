# Notificaciones para Seguimiento de Preocupacionales.

Este repositorio contiene el código del bot de notificaciones para el seguimiento de preocupacionales. Este proyecto lo hicimos para automatizar tareas repetitivas del área de Recursos Humanos al momento de darle seguimiento a los preocupacionales. 

En este bot usamos tecnologías como Nodejs, Expressjs, TypeScript, Power Automate, Sharepoint, Microsoft Teams, Docker y Whatsapp.

## Instalación del bot

Para instalarlo en tu máquina local tienes que instalar primero las librerias y dependencias:

```
npm install
```

Y después estos dos comandos:

```
npm run build
```

```
npm run dev
```

Despues por consola se comenzara a ejecutar el proyecto y te mostrara el código QR que tendras que escanear en Whatsapp para que el bot tenga acceso.

Finalmente cuando lo escanees podras enviar peticiones con el método POST al endpoint:

```
http://localhost:7672/lead
```

Tendras que enviar un JSON con el mensaje y el número telefónico al que deseas enviar el mensaje. Por ejemplo:

```
{   
   "message": "hola, mensaje de prueba",   
   "phone": "5491169854355"
}
```

Tendran que especificar el código de acceso internacional de Argentina (54), prefijo para indicar que es un número de celular (9), el código de area en este caso es de Ciudad Autónoma de Buenos Aires (CABA) y después número de teléfono específico (69854355). Esto lo aclaro porque si se hace mal, no se va a recibir ningun mensaje.

## Automatización

Este proyecto esta divido en dos, el servidor donde estaría el contenedor Docker con el bot y la automatización con Power Automate y Sharepoint.

En la parte de automatización creé un flujo nuevo. Si la columna en la lista de Sharepoint llamada "Notificar" tenga el valor "true" le notifique al que tiene el turno preocupacional datos sobre el estudio, como la fecha y lugar. 

El flujo primero verifica si se modifico un elemento, en tal caso de que se haya modificado un elemento, si la columna Notificar tiene el valor "true" se va a enviar una petición al endpoint con el JSON y se va a publicar un mensaje en un canal de Teams diciendo que ya se envío el mensaje.

![Automatización](img/automatizacion.png)

## Docker

Para ejecutarlo en un contenedor de Docker, primero se tiene que crear la imagen con este comando:

```
docker build -t bot-notificaciones .
```

Después hay que levantar el contenedor:

```
docker run --privileged -e PORT=7672 -p 7672:7672 apiwhatsappts:latest
```

