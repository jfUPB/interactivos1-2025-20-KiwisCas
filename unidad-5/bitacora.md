
# Evidencias de la unidad 5

## Set-Seek

### Preguntas caso de estudio

- Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?

El micro:bit y el sketch poseen una comunicación mediante protocolos ASCII, inicialmente y desde el microbit, se escogen que datos son los que se quieren enviar para luego mediante un formato preestablecido que separa los tipos de datos en comas (,) y separa una cantidad de datos utilizando `\n` para que luego desde el sketch, se reciban estos datos despues de realizar una conexión con el puerto serial que permite detectar el tipo de datos que están siendo enviados por el Micro:Bit, posterior a esto, en el sistema se llama a una función que se encarga de borrar los espaciados y mediante una función que divide los datos en base a una referencia que en este caso son las comas (,) se logran dividir los datos en partes para luego poder ser utilizados en el sketch de forma separada, recibiando datos de los acelerometros en X y Y, al igual que de los botonas A y B del Micro:Bit.

- ¿Cómo es la estructura del protocolo ASCII usado?

- Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.
 ```js
    let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length == 4) {
          microBitX = int(values[0]) + windowWidth / 2;
          microBitY = int(values[1]) + windowHeight / 2;
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";
          updateButtonStates(microBitAState, microBitBState);
        } else {
          print("No se están recibiendo 4 datos del micro:bit");
        }
      }
 ```

Esta es la parte del código en donde en el sketch se obtienen, se leen y se transforman los datos a coordenadas, como podemos ver que inicialmente el sistema va a leer los datos y los va a partir en el momento en el que este lee una `\n`, desde ese momento empieza a realizar una función que se encarga de quitar los espacios y separar los datos en base a comas para luego, asignarlos a 4 variables las cuales en este código son `microBitX`, `microBitY`, `microBitAState` y `microBitBState`, funciones que luego son utilizadas en el sistema para poder dibujar en pantalla en base a los datos recibidos por el micro:Bit
  
- ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?

Esto ocurre gracias a las siguientes líneas de código
  ```js
  microBitAState = values[2].toLowerCase() === "true";
  microBitBState = values[3].toLowerCase() === "true";
  ```
Lo que hacen estas funciones es que convierten lo recibido en las dos ultimas secciones de datos, un string de datos que convierte en minúscula, luego de esto, se llama a una función que realiza funciones especiales en baso a los estados anteriores del micro:bit

Este proceso se realiza 
- Capturas de pantalla de los algunos dibujos que hayas hecho con el sketch.
<img width="872" height="817" alt="20250910_170503" src="https://github.com/user-attachments/assets/af3a0988-d527-4f7e-9de1-323bd3a11855" />
