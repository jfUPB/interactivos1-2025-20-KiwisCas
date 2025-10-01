
# Evidencias de la unidad 6


## Actividad 1

¿Qué ocurrió en la terminal cuando ejecutaste npm install? ¿Cuál crees que es su propósito?

Lo que ocurre es que se muestra la siguiente linea de código

```bash
up to date, audited 121 packages in 753ms

17 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

esto quiere decir que, en ese momento, se estaban instalando para el programa, las dependencias del sistema que este necesita para funcionar de forma correcta y sin algún fallo por parte de estas por lo que, de no haberse ejecutado, el programa no podría funcionar.
Luego de que se ejecuten las instalaciones y que las dependencias se instalaran correctamente, se procede a ejecutar el `npm start`, cuando se ejecuta este comando, pasado unos segundos aparecerán las siguientes lineas en la consola de ejecución:

```bash
$ npm start

> nodejs-test-1@1.0.0 start
> node server.js

Server is listening on http://localhost:3000
A user connected - ID: PSArXx1RRBboa3ehAAAC
A user connected - ID: f-x55X7NjL9LTHwvAAAD
```
Lo que sucede es que node se encarga de enviar información al buscador de forma local por medio de un puerto de comunicación serial, luego de esto, el buscador se encarga de descargar las dependencias que se encuentran en page1.js, page2.js y para luego ejecutar los html que se encargan de dar forma (imagen) a la página para que el ususario pueda ver lo que sucede.

luego de que ocurra todo este proceso, en la página, en primera instancia se puede bservar lo siguiente:
<img width="1919" height="1024" alt="image" src="https://github.com/user-attachments/assets/e25b6bfd-133e-47aa-be77-f7ea369b26db" />
acá lo que se puede ver es que, como no hay otra ventana abierta ejecutando la page 2, el programa está esperando a recibir la señal y hasta que no ocurra esto, ser qudará en esta ventana esperando, por lo que vamos a proceder a abrir la otra ventana y como podemos ver, lo que ocurre es que en una de las dos pestañas se observa lo siguiente:
<img width="1919" height="1026" alt="image" src="https://github.com/user-attachments/assets/6a6552e1-5995-4f29-8df8-9c74cab07e1d" />


## Actividad 2
### Sobre el internet

De acuerdo con la analogía, se supone que el internet es algún tipo de carretera, lugares en donde millones de dispositivos se ingresan a esta ruta en conjunto y que tanto el Wi-Fi como los cables ethernet funcionan a modo de rampas para acceder a esta carretera, yo me imagino algo así como que en lugar de rampas el internet es un motor que permite ingresar a esa ruta, lo que ocurre cuando el motor falla lo que en palabras menores podría decirse que el internet se cortó o dejó de enviar señales, en gran parte de los casos podría decirse que este automovil se estropea y debe parar momentaneamente, lugar en donde, si se ha hecho conexión antes, este va a quedar (en muchos de los casos) recibiendo la información que este estaba observando hasta el ultimo momento antes de fallar, en casos mayores, la conexión se pierde por completo y toca volver a reingresar nuevamente a la vía.

Profundizando un poco más y siguiendo el concepto de carretera, las carreteras en la vida real cumplen la funcion de ser de guía para un destino al cual tu aspiras llegar, en este aco, con el internet pasa lo mismo, la vía en este caso con las miles de carreteras que tiene te puede llevar a millones de sitios, simplemente debes ser tu el que se encargue de escoger que deseas hacer, en muchos casos debes ser tu el que da las indicaciones para que el sistema sepa a donde debe dirigirse, podría decirse que los vehículos que transportan al usuario por toda la red se encargan de que en cada momento en donde la vía debe partirse en varios caminos, sea el usuario el que decida por donde debe ir el auto.

### sobre URLs

Como buen fanático de los videojuegos, tengo un peculiar gusto por steam por lo que suelo usar mucho esta aplicacioón pero esta aplicación tiene algo un tanto peculiar, resulta y sucede que steam en gran parte ,ás que ser una aplicación, es un browser modificado para funcionar con páginas en específico, la página inicial de steam es la siguiente: [https://store.steampowered.com/?l=spanish](https://store.steampowered.com/?l=spanish) en donde HTTPS es el protocolo, `store.steampowered.com` es el dominio y `/?l=spanish` es la ruta.

### Sobre protocolos seriales y el protocolo http

Entendamos algo, las dos formas de protocolos cumplen de cierta forma la misma función la cual es recibir datos enviados de un "servidor" a un "cliente" para que este se encargue de daro forma al funcionamiento de estos datos, que ocurre, estas dos formas de comunicación si bien comparten ciertas similitudes como por ejemplo ser protocolos de comunicación los cuales miten las reglas para que el emisor y receptor entiendan los mensajes, necesitan delimitaciones para saber en que momento acaba el envío de un mensaje 




