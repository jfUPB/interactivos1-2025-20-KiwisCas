
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
Lo que hacen estas funciones es que convierten lo recibido en las dos ultimas secciones de datos, un string de datos que convierte en minúscula, luego de esto, se llama a una función que realiza funciones especiales en base a los estados anteriores del micro:bit

- Capturas de pantalla de los algunos dibujos que hayas hecho con el sketch.
<img width="872" height="817" alt="20250910_170503" src="https://github.com/user-attachments/assets/af3a0988-d527-4f7e-9de1-323bd3a11855" />





## Seek

### Captura de Aplicación de conexión serial cuando micro:bit envía datos de tipo binario

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
### Vamos a modificar el código de la siguiente manera:

```py
# Imports go at the top
from microbit import *
import struct
uart.init(115200)
display.set_pixel(0,0,9)

while True:
    if accelerometer.was_gesture('shake'):
        xValue = accelerometer.get_x()
        yValue = accelerometer.get_y()
        aState = button_a.is_pressed()
        bState = button_b.is_pressed()
        data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
        uart.write(data)
```

Acá el código se está modificando para que cada vez que el micro:bit sea sacudido este envíe datos cuando este se sacude, a continuación se observa cuando el micro:bit es ajitado y se analizan los datos en la aplicación de conexión serial

<img width="998" height="459" alt="image" src="https://github.com/user-attachments/assets/fc7952d6-11a3-4fef-840f-30bd565de13f" />


Acá lo que podemos observar es que se están enviando datos de a 6 bytes cada uno ya que cada par de numeros representa un byte, como se dijo en el pasado, el formato `>2h2B` es un modo en el que los datos son empaquetados, en este caso  se empaquetan primero con los bytes más significativos primero, luego van dos enteros de 16 bits (2h) donde cada `h` equivale a dos bytes lo que nos da a entender que se están enviando 4 bytes de este formato y luego dos enteros de 8 bits sin signo (2B) donde cada `B` equivale a 1 byte así que acá se reciben 2 bytes de este tipo, por lo que haciendo una asociación podemos decir que los 2 primeros bytes corresponden a `xValue`, los otros dos que le siguen corresponden a `yValue` y los ultimos dos corresponden a `aState` y `bState` (convertidos en enteros) de forma respectiva.

Acá nos paramos a pensar un poco sobre la forma en la que los enteros positivos y negativos se envían en el formato con el que estamos trabajando, para que esto funcione, las computadoras actuales realizan un sistema binario de interpretación numérica conocido como el complemento a dos, para calcular este mismo, se tienen que invertir todos los bits de un número (0 por 1 y 1 por 0) y luego se le suma 1 a este resultado.

**Pongamos un Ejemplo**
Digamos que en el formato que se están enviando los archivos, para las dos primeras formas de bytes que se están enviando se quieren enviar los numeros 2020 y luego el -2020, primero vamos con el 2020, utilizando un conversor de Decimales a Hex nos percatamos de que el número 2020 en complemento a dos se da de la siguiente manera en Hex: `0x07 E4` por lo que, si se enviase el mensaje en el formato `>2h2B` y suponiendo que los otros dos valores son falsos este quedaría de la siguiente manera:

```mathematica
07 E4 07 E4 00 00
```

Ahora vamos con el -2020, este en formato hex se ve de la siguiente manera: `0xF81C`, y enviando un mensaje con el formato con el que se ha estado trabajando este quedaría de la siguiente manera: 

```mathematica
F8 1C F8 1C 00 00
```
### Datos en formato binario Vs. Datos en Formato ASCII

**Observemos las siguientes dos imágenes**


<img width="924" height="457" alt="image" src="https://github.com/user-attachments/assets/6b558916-1e6c-4b55-8c2b-15e03322026d" />


<img width="921" height="461" alt="image" src="https://github.com/user-attachments/assets/62f45e26-3088-479d-8288-6e3f7c3e2bed" />

Si te fijas en las imágenes verás que hay unos numeros y datos resaltados, estos son los datos que están siendo enviados en binario, como puedes observar, es un formato muy optimizado puesto que, al tratarse de un formato que está diseñado para poseer una comunicación directa con la computadora, la forma en la que estos datos son enviados y recibidos es mucho más óptima en términos de optimización para el sistema, sin embargo, si observas la parte en la que los datos se leen como texto puedes ver que los datos pasados a textos son símbolos sin sentido y es ahí en donde entra la ventaja del formato ASCII puesto que este formato es mucho más comprensible para nosotros ya que nos permite entender con gran facilidad los datos que son recibidos puesto que estos datos son directamente texto, sin embargo, para la computadora, la interpretación de estos datos requiere de un esfuerzo un poco mayor lo que obviamente puede llegar a afectar el rendimiento de este mismo, pero como se dijo anteriormente, para las personas se nos es más facil escribir de esta manera puesto que realizar las conversiones de instrucciones a un formato binario es un proceso muy extenso y complejo.



 <img width="1919" height="949" alt="image" src="https://github.com/user-attachments/assets/c0a79e7d-d89b-44d8-b23e-6df997281f1d" />


<img width="1919" height="894" alt="image" src="https://github.com/user-attachments/assets/0b1413ee-8c4f-4e8e-9256-fed58891ebb8" />

<img width="1919" height="849" alt="image" src="https://github.com/user-attachments/assets/12f7c302-b709-4183-b822-5c5a8886a84d" />

<img width="1919" height="785" alt="image" src="https://github.com/user-attachments/assets/bd33cd25-0d4e-4871-b151-e331119db2d9" />


<img width="1919" height="893" alt="image" src="https://github.com/user-attachments/assets/f7a602bf-0fc8-49dd-88f2-c7dead237e38" />






