
# Evidencias de la unidad 7

¿Qué URL de Dev Tunnels obtuviste? ¿Por qué crees que necesitamos usar esta URL en lugar de http://localhost:3000 o la IP local de tu computador para que el celular se conecte?

obtuve la siguiente URL: https://f51lclcj-3000.use2.devtunnels.ms/ 
Cuando se trabaja con servidores locales, al usar `https://localhost:3000` esto significa que el servidor solamente va a funcionar dentro del mismo computador en donde se está corriendo el servidor debido a que cuando se usa local host este **apunta a la dirección de si mismo**, el loopback de la máquina, por lo que cuando el celular o cualquier otro dispositivo intenta acceder a esta misma dirección url, este va a buscar una dirección en su propio sistema, no en un sistema externo.


Describe brevemente qué hace npm install y npm start.

`npm install` se encarga de instalar las dependencias (librerías y módulos) que el proyecto necesita de acuerdo a lo que se indique en el archivo de `package.json`, básicamente este se encarga de preparar el entorno para que la aplicación se pueda ejecutar de forma adecuada.

Por otra parte, el `npm start` ejecuta elcomando definido en la sección scripts del `package.json` bajo la sección `start` Normalmente esta suele ser la que inicia el sevidor de desarrollo o lanza la aplicación.

¿Qué mensajes observaste en la terminal del servidor al conectar el cliente de escritorio y el cliente móvil? ¿Eran diferentes los mensajes o identificadores?

Cuando se conecta el cliente de escritorio y el cliente movil aparecen los mismos mensajes `New Client Connected`, no especifica cual.

Describe el comportamiento observado: ¿Funcionó la interacción? ¿Hubo algún retraso (latencia)?

Cuando se abren los dos clientes, por la parte de mobile aparece lo siguiente:

<img width="468" height="908" alt="image" src="https://github.com/user-attachments/assets/285d3b81-35f0-4d98-b905-e97f5af80855" />


Por otra parte, para la aplicación de escritorio aparece lo siguiente:

<img width="435" height="906" alt="image" src="https://github.com/user-attachments/assets/29ab4c52-b601-4e37-8331-24e708e0f42f" />

---

El sistema funciona de la siguiente manera, cuando se pone el dedo encima en la aplicación de escritorio, este envía señales de tipo touch con las coordenadas en donde se está detectando el dedo, luego, estas mismas coordenadas son recibidas por la pestaña de escritorio la cual se encarga de darle movimiento a la bolita siguiendo las mismas coordanadas de movimiento que sigue el dedo, si bien hay un poco de delay, este es demasiado poco que puede llegar a percibirse solo durante unos instantes.

## Actividad 2

### Explica con tus propias palabras: ¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente?
Según lo que tengo entendido, Dev Tunnels es una especie de "tunel seguro" para poder acceder a la aplicación por fuera del entorno de ejecución local, la aplciación funciona algo así:
    - La aplicación se ejecuta inicialmente en un entorno local desde un puerto, como se ha estado trabajando, podría ponerse de ejemplo el puerto 3000 `http://localhost:3000`
    - Dev Tunnels se encarga de crear puerto de conexión cifrada (https) entre el pc y los servidores de Microsoft.
    - Luego estos servidores dan una URL pública que apunta directamente al tunel local, posterior a eso
      - La solicitud pasa por el servidor de Dev Tunnels.
      - Este la redirige a la aplicación local.
      - app responde, y la respuesta viaja de vuelta por el túnel al cliente remoto.

### Describe la función de touchMoved()
- La función `touchMoved()`es una función que se ejcuta por p5.js, esta se ejecuta cuando el usuario arrastra el dedo sobre el canvas en el cliente móvil.
- Dentro de esta función se comprueba de que exista la conexión socket y que esté conectada: `if (socket && socket.connected)`.
- Luego esta calcula el desplaamiento desde la última posición de toque registrada por medio de las siguientes lineas de código:
  
```js
let dx = abs(mouseX - lastTouchX);
let dy = abs(mouseY - lastTouchY);
```
En celulares, los valores mouseX y mouseY representan la posición de toque activa.

- Es solo cuando el desplazamiento supera cierto umbral (o si antes no habían posiciones registradas) que se crea un objeto touchData con `{ type: 'touch', x: mouseX, y: mouseY }` y este es enviado al servidor via Socket.IO
- Despues de la emisión este actualiza `lastTouchX` y `lastTouchY` con la nueva posición
- Por último retorna `false` para evitar el comportamiento que tiene por defecto del navegador que es el de scrollear cuando se toca el canvas.

  
### Por qué se usa la variable threshold en el cliente móvil.

La variable treshold (`const threshold = 5;`) se encarga de definir la distancia mínima en pixeles que debe cambiar la posición entre envíos sucesivos para que se emita un nuevo mensaje. Esta es una función que se encarga de reducir el jitter puesto que los sensores táctiles peuden llegar a dar pequeñas variaciones cuando el dedo puede llegar a estar incluso quieto, este umbral se encarga de evitar esos cambios insignificantes, aparte de que se encarga de ahorrar ancho de banda y CPU puesto que evta enviar demasiados pensajes por Socket.IO cuando no hay movimientos reales. aparte de que evita que la aplicación reciba muchos eventos y se vea que tiembla por micromovimientos.

### Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?

|Puntos|Uso de IPs|Uso de Dev Tunnels|
|----|----|----|
|Accesibilidad|Solo funciona utilizando la misma red Wi-Fi, en caso de que no, esta no va a funcionar|Cuando de utiliza Dev Tunnels, esta es accesible de sde cualquier red y cualquier dispositivo movil, no hay que estar conectado a una red en específico para poder funcionar adecuadamente|


- Coloca en tu bitácora capturas de pantalla del sistema completo funcionando. Esto lo puedes hacer abriendo tanto el mobile como el desktop en tu computador y tomando una captura de pantalla de todos los involucrados (celular, computador y terminal).


