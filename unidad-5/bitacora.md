
# Evidencias de la unidad 5

## Set

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


## Seek

### Captura de Aplicación de conexción serial cuando micro:bit envía datos de tipo binario

<img width="985" height="172" alt="image" src="https://github.com/user-attachments/assets/7ea76a78-5bf4-49c3-b915-3c1e2acb73db" />

Analicemos este caso, como se puede observar en la imagen, cuandoi se pone el la forma en la que se ven los datos como modo texto se puede observar una secuencia de caracteres sin sentido esto ocurre debido a que los datos están siendo enviados directamente como una secuencia de `bytes` que respresentan valores entre 0 y 255, hay que entender que cuando se leen archivos a modo de texto este tipo de codificación posee una codificación estandar que suele ser en formatos como `UTF-8`, `ASCII`, `ISO-8859-1` y asi sucesivamente, lo que pasa cuando se intentan leer envíos de datos binarios y se intentan traducir a formato de texto es que el sistema intentará codificar los bytes como si estos representaran caracteres válidos según el tipo de codificación pero en este caso muchos bytes no representan caracteres válidos por lo que simplemente se observarán caracteres extraños como se ve en el ejemplo.

<img width="902" height="196" alt="image" src="https://github.com/user-attachments/assets/72a912f5-a03a-4e12-a03c-05ab4f98f30c" />

En este caso, se puede ver que hay una secuencia de caracteres en formato Hexadecimal, lo que ocurre acá es que el formato en los que los datos están siendo recibidos es el correcto en términos de rececpción de señal, es decir que al estar los datos siendo enviados en un formato binario, la aplicación de conexión serial está mostrando los datos tal cual y como son enviados, byte por byte, lo que permite comprender correctamente el tipo de dato que está siendo enviado y recibido en números, lo que ocurre es que claramente este tipo de datos no son para compresión humana en un inicio.

Lo que ocurre con la siguiente linea de código
```py
data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
```
Es que esa linea de código se encarga de empaquetar datos en formato binario usando una librería de python llamada `struct` este empaquetado indica un tipo de compresión de datos donde el formato de compresión lo da este pedacito `>2h2B` este formato indica cosas diferentes dependiendo del pedazo que se analice, por ejemplo:
 - `>` Big-endian (byte más significativo primero).
 - `2h` Dos enteros cortos con signo (short de 2 bytes cada uno = 4 bytes en total).
 -  `2B` Dos enteros sin signo de un byte cada uno.
lo que hace que los datos se envíen en un formato como el siguiente:

```mathematica
ff d8 00 98 01 00
``` 


<img width="916" height="300" alt="image" src="https://github.com/user-attachments/assets/936cb2e2-d2dc-44b7-b322-b63e2482a6a5" />


 <img width="1919" height="949" alt="image" src="https://github.com/user-attachments/assets/c0a79e7d-d89b-44d8-b23e-6df997281f1d" />




