
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




