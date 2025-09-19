
# Evidencias de la Unidad 5

---

## Set

### Preguntas caso de estudio

#### 1. Describe cómo se están comunicando el micro\:bit y el sketch de p5.js. ¿Qué datos envía el micro\:bit?

El **micro\:bit** y el sketch de **p5.js** establecen comunicación a través del **puerto serial**. En este proceso:

1. Desde el micro\:bit se seleccionan los datos que se quieren enviar:

   * Valores del **acelerómetro en los ejes X e Y**.
   * Estados de los **botones A y B**.
2. Se organiza la información en un **formato ASCII**, separando cada dato con comas (`,`) y usando un salto de línea (`\n`) para marcar el fin del mensaje.
3. En p5.js, tras abrir el puerto serial, se reciben los datos como texto.
4. Mediante funciones de limpieza (`trim`) y separación (`split`), los datos se fragmentan en partes y se asignan a variables individuales (`microBitX`, `microBitY`, `microBitAState`, `microBitBState`).

---

#### 2. ¿Cómo es la estructura del protocolo ASCII usado?

El mensaje enviado sigue la siguiente estructura:

```mathematica
xValue,yValue,aState,bState\n
```

* **Delimitador de campos:** la coma (`,`) separa cada dato.
* **Delimitador de paquete:** el salto de línea (`\n`) indica el fin de un conjunto de datos.
* **Codificación:** cada número o estado (`True` / `False`) se transforma en caracteres ASCII.

Ejemplo en memoria:

```mathematica
"450,-320,True,False\n"
```

Cada carácter se convierte en un **byte ASCII**:

* `4` → `0x34`
* `5` → `0x35`
* `0` → `0x30`
* `,` → `0x2C`
* `-` → `0x2D`

Esto hace que un número como `-320` ocupe **4 bytes en ASCII**, mientras que en binario ocuparía solo **2 bytes**.

---

#### 3. Muestra y explica el código de p5.js donde se leen los datos

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

**Explicación paso a paso:**

* `readUntil("\n")` → lee hasta detectar el delimitador de fin de paquete.
* `trim()` → limpia espacios innecesarios.
* `split(",")` → divide la cadena en fragmentos por las comas.
* Conversión → los dos primeros valores se transforman en enteros (`int`), y los últimos dos en booleanos (`true/false`).
* Ajuste gráfico → se suman las dimensiones de la ventana para ubicar correctamente las coordenadas en la pantalla.

---

#### 4. ¿Cómo se generan los eventos A pressed y B released?

Esto ocurre gracias a la conversión de los datos enviados por el micro\:bit en **valores booleanos** en p5.js:

```js
microBitAState = values[2].toLowerCase() === "true";
microBitBState = values[3].toLowerCase() === "true";
```

* Si `aState = True`, p5.js activa el evento **A pressed**.
* Si `bState = False`, p5.js activa el evento **B released**.

El truco está en la comparación con `"true"` y la conversión a minúscula para evitar problemas de formato.

---

#### 5. Capturas de pantalla de algunos dibujos realizados con el sketch

<img width="872" height="817" alt="20250910_170503" src="https://github.com/user-attachments/assets/af3a0988-d527-4f7e-9de1-323bd3a11855" />  

---

## Seek

### Captura de aplicación de conexión serial con datos binarios

Cuando se pasa de enviar datos en **ASCII** a **binario**, el monitor serial en modo texto muestra caracteres extraños:

<img width="985" height="172" alt="image" src="https://github.com/user-attachments/assets/7ea76a78-5bf4-49c3-b915-3c1e2acb73db" />  

Esto ocurre porque:

* Los bytes transmitidos (0–255) no siempre representan caracteres válidos en ASCII o UTF-8.
* El sistema intenta interpretarlos como letras, pero no tienen equivalencia → aparecen símbolos ilegibles.

En cambio, al visualizar en modo **hexadecimal**:

<img width="902" height="196" alt="image" src="https://github.com/user-attachments/assets/72a912f5-a03a-4e12-a03c-05ab4f98f30c" />  

Los datos se muestran **exactamente como fueron enviados**: secuencias de bytes hexadecimales que sí reflejan la realidad de la transmisión.

---

### Empaquetado con `struct.pack`

Código en MicroPython:

```py
data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
```

* `>` → **Big-endian**, el byte más significativo se envía primero.
* `2h` → dos enteros cortos con signo (16 bits cada uno).
* `2B` → dos enteros sin signo de 8 bits cada uno.

Por lo tanto, cada paquete ocupa exactamente **6 bytes**:

| Campo  | Tipo | Tamaño  |
| ------ | ---- | ------- |
| xValue | h    | 2 bytes |
| yValue | h    | 2 bytes |
| aState | B    | 1 byte  |
| bState | B    | 1 byte  |

Ejemplo de paquete:

```mathematica
ff d8 00 98 01 00
```

---

### Ejemplo modificado con gesto *shake*

```py
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

Ahora, el micro\:bit envía datos únicamente cuando detecta la acción de **agitarse**.

---

### Representación de números negativos con complemento a dos

El formato `h` usa **16 bits con signo** y representación en **complemento a dos**.

**Cómo funciona:**

1. Se representa el valor absoluto en binario.
2. Se invierten los bits (complemento a uno).
3. Se suma 1 → resultado en complemento a dos.

#### Ejemplo: `2020` y `-2020`

* `2020` decimal → `0000 0111 1110 0100` → `07 E4` en hex.
* `-2020` decimal:

  * Valor absoluto: `0000 0111 1110 0100`
  * Invertir bits: `1111 1000 0001 1011`
  * +1 → `1111 1000 0001 1100` → `F8 1C` en hex.

Por lo tanto, en `>2h2B`:

```mathematica
2020,2020,0,0 → 07 E4 07 E4 00 00
-2020,-2020,0,0 → F8 1C F8 1C 00 00
```

---

### Comparación Binario vs ASCII

| Característica        | ASCII                               | Binario                             |
| --------------------- | ----------------------------------- | ----------------------------------- |
| **Legibilidad**       | Muy alta, fácil de depurar          | Muy baja, ilegible sin herramientas |
| **Tamaño de paquete** | Variable, más grande                | Fijo, muy compacto                  |
| **Velocidad**         | Menor, necesita procesar caracteres | Mayor, transmisión directa          |
| **Errores**           | Fácil de detectar manualmente       | Necesita control de integridad      |
| **Uso típico**        | Prototipos, pruebas                 | Sistemas finales, optimización      |

Ejemplo:

* En ASCII → `"-300,450,True,False\n"` ocupa **20+ bytes**.
* En Binario → mismos datos en `>2h2B` ocupan **6 bytes**.

---

### Actividad 03

**Caso de estudio: p5.js**
Tema central: Modificación del código de micro\:bit y p5.js para soportar lectura de datos en **formato binario con framing y verificación de integridad**.

---

### Explicación inicial

En la **unidad anterior**, el micro\:bit enviaba datos serializados en formato ASCII y separados por delimitadores (comas, espacios o saltos de línea). Esto se debía a que el tamaño del paquete no estaba predefinido:

* Cada valor (x, y, aState, bState) podía ocupar un número variable de caracteres.
* Era necesario marcar el **final del paquete** con un salto de línea (`\n`) para que el receptor (p5.js) pudiera distinguir cuándo terminaba una lectura y empezaba otra.

En cambio, ahora la estructura del paquete es **binaria y fija**:

* `xValue` → 2 bytes (entero corto con signo, complemento a 2).
* `yValue` → 2 bytes (entero corto con signo, complemento a 2).
* `aState` → 1 byte (0 o 1).
* `bState` → 1 byte (0 o 1).

Total: **6 bytes exactos por paquete**.
Esto elimina la necesidad de delimitadores, porque el receptor ya sabe que cada vez que reciba 6 bytes completos, tendrá un paquete válido.

Ejemplo (big-endian):

```mathematica
xValue = 500  →  01 f4
yValue = 524  →  02 0c
aState = 1    →  01
bState = 0    →  00

Paquete = 01 f4 02 0c 01 00
```

---

### Cambios en el código

1. **Unidad anterior (ASCII + delimitadores):**

   * El micro\:bit enviaba los valores como texto (ejemplo: `"500,524,True,False\n"`).
   * p5.js recibía una cadena, la partía con `.split()`, y luego convertía cada valor a número o booleano.

2. **Unidad actual (binario fijo):**

   * El micro\:bit usa `struct.pack('>2h2B', ...)` para empaquetar los valores en binario (big-endian, enteros con signo).
   * p5.js lee directamente **6 bytes** con `port.readBytes(6)` y usa `DataView` para interpretar:

     * `getInt16(0)` → xValue (complemento a 2, permite negativos).
     * `getInt16(2)` → yValue.
     * `getUint8(4)` y `getUint8(5)` → estados de los botones.

3. **Problema detectado:**
   Al ejecutar varias veces, aparecen valores inconsistentes como:

   ```js
   
   microBitX: 500 microBitY: 524 ...
   microBitX: 524 microBitY: 256 ...
   microBitX: 3073 microBitY: 1 ...
   
   ```

   <img width="1919" height="849" alt="image" src="https://github.com/user-attachments/assets/12f7c302-b709-4183-b822-5c5a8886a84d" />

   Esto ocurre porque los 6 bytes no siempre llegan alineados. El puerto serie entrega datos en **fragmentos arbitrarios**, y p5.js puede leer bytes que pertenecen a **dos paquetes distintos** → error de sincronización.

4. **Solución aplicada: Framing + Checksum**

   * Se añade un **byte de inicio (0xAA)** para identificar el comienzo del paquete.
   * Se añaden los 6 bytes de datos.
   * Se agrega un **checksum** (suma de los bytes de datos módulo 256) para validar la integridad.

   Ahora el paquete tiene **8 bytes**:

   ```methematica
   [Header][xValue][yValue][aState][bState][Checksum]
   0xAA    01 f4   02 0c   01      00      ??
   ```

   En p5.js:

   * Se acumulan los bytes en un `serialBuffer`.
   * Se busca el `0xAA` como header.
   * Se verifica el checksum.
   * Solo si es válido, se extraen los valores.

---

###  Observaciones en consola

1. **Antes del framing:**

   * La consola mostraba datos correctos al inicio, pero luego aparecían lecturas erróneas como `microBitY: 513` o valores muy grandes (`3073`).
   * Esto confirma que los paquetes llegaban desalineados.

     <img width="1919" height="849" alt="image" src="https://github.com/user-attachments/assets/12f7c302-b709-4183-b822-5c5a8886a84d" />

2. **Con framing y checksum:**

   * La consola muestra datos estables y coherentes, incluso si se producen fragmentaciones en la transmisión.
   * Si llega un paquete corrupto, aparece un mensaje:

     ```js
     Checksum error in packet
     ```
     <img width="1919" height="785" alt="image" src="https://github.com/user-attachments/assets/bd33cd25-0d4e-4871-b151-e331119db2d9" />
     lo cual permite detectar el problema sin dañar la lectura global.

3. **Cambios finales:**

   * El micro\:bit ahora envía datos binarios con header y checksum.
   * p5.js procesa un buffer de bytes, sincroniza con el header y valida integridad.
   * En consola ya no aparecen valores erráticos, lo que demuestra que el **framing resuelve los problemas de sincronización**.

---

### Actividad 04

**Aplicación práctica del protocolo binario**
---

### Proceso de construcción

El trabajo inició a partir de la aplicación de la unidad anterior, que recibía datos en formato **ASCII** y dependía de delimitadores como comas y saltos de línea. Esa estrategia funcionaba, pero generaba sobrecarga y no era adecuada para una comunicación binaria.

Se plantearon varios pasos:

1. **Revisión del código existente:**
   Se observó que el flujo usaba `readUntil("\n")` y luego `split(",")`. Esto debía reemplazarse por un sistema de lectura de **bytes binarios**.

2. **Creación de un buffer serial:**
   Se implementó un arreglo `serialBuffer` que acumula los bytes a medida que llegan del puerto. Esto era esencial, ya que el puerto serial no garantiza la llegada de un paquete completo en un solo ciclo.

3. **Framing con header (`0xAA`):**
   Se introdujo un identificador de inicio. Si el primer byte no coincidía con `0xAA`, se descartaba hasta lograr la alineación correcta.

4. **Validación de checksum:**
   Cada paquete incluye un byte de checksum calculado como la suma de los datos módulo 256. En el receptor se recalculaba y, si no coincidía, el paquete se descartaba.

5. **Lectura de datos binarios con DataView:**
   Una vez validado el paquete, se usó un `DataView` para interpretar los valores correctamente:

   * `getInt16(0)` para `xValue`.
   * `getInt16(2)` para `yValue`.
   * `getUint8(4)` y `getUint8(5)` para los botones A y B.

---

### Experimentos y dificultades

Durante la implementación surgieron varios problemas que permitieron reforzar la comprensión:

1. **Valores incoherentes en pantalla:**
   En las primeras pruebas, los valores del acelerómetro se mostraban como `3073`, `-123` o números fuera de rango. Esto ocurrió porque los 6 bytes del paquete llegaban desalineados, lo que confirmaba la necesidad de usar el header para recuperar sincronización.

2. **Errores de checksum recurrentes:**
   Durante la validación, algunos paquetes eran rechazados con mensajes de `"Checksum error"`.

     <img width="2559" height="1029" alt="image" src="https://github.com/user-attachments/assets/70b8cbde-cb76-480e-8f05-a134e32e2b4b" />

   Al revisar, se detectó que ciertos bytes se perdían en la transmisión o quedaban mezclados entre paquetes. La estrategia de descartar paquetes inválidos y esperar al próximo header resolvió el problema, por lo que al final, realizando cambios a la linea de código que presentaba el problema, se corrigió quedando de la siguiente forma

   ```js
   let computedChecksum = dataBytes.reduce((a, b) => a + b, 0) % 256;
   ```

4. **Interpretación de números negativos:**
   Al inclinar el micro\:bit o hacer pruebas con el puerto de conexión serial algunos valores aparecían como números muy grandes en lugar de negativos.

    <img width="2559" height="1031" alt="image" src="https://github.com/user-attachments/assets/772baee7-09d3-41eb-8762-d6284622ab74" />

   Esto llevó a verificar la forma en que `getInt16()` interpreta los enteros en **complemento a 2**. Una vez corregido el uso de big-endian, los valores negativos (ej. `-2020` representado como `f8 1c`) se mostraron de una forma más aproximada, ya que luego se puede ver en la siguiente imagen que los datos que están siendo enviados y los que están siendo recibidos no son correctos tampoco :(

   <img width="2559" height="1029" alt="image" src="https://github.com/user-attachments/assets/c1a22082-955b-4080-9f7c-8cbbf5bda206" />

   Por lo que, habiendo descubierto esto, hay que entender que es correcto que en el log se vean datos erroneos, esto debido a que en el programa se están haciendo unos cálculos de más para que el programa dibuje adecuadamente en el canvas, si realizamos un pequeño cambio al código podemos observar que, efectimvamente los datos que están siendo enviados al programa por el programa de conexión serial y el micro:bit son correctos

   <img width="2516" height="955" alt="image" src="https://github.com/user-attachments/assets/6db7d7a7-cfae-48c8-aa6c-0a13645e8036" />

6. **Pérdida de sincronización temporal:**
   En ocasiones, al desconectar y reconectar, el sistema tardaba en recuperar la alineación de los paquetes. La solución fue limpiar el buffer al establecer conexión y dejar que el algoritmo buscara nuevamente el header.

---

### Observaciones finales

* Con el sistema ASCII inicial, los datos eran fáciles de leer pero poco eficientes.
* La transición a binario fijo de 6 bytes redujo el tamaño del paquete, pero expuso problemas de sincronización.
* La inclusión del **header** resolvió el problema de alineación, y el **checksum** permitió descartar paquetes corruptos sin afectar la aplicación.
* Al final, el dibujo generado en p5.js respondió correctamente a los movimientos y botones del micro\:bit, mostrando estabilidad en los valores recibidos.

---

### Conclusión

El desarrollo de esta actividad permitió comprender que, en comunicación serial:

* No basta con transmitir datos, es necesario diseñar un protocolo que asegure **longitud fija, sincronización e integridad**.
* El **complemento a 2** es fundamental para representar valores negativos en formato binario y debe interpretarse correctamente en el receptor.
* Los problemas iniciales de incoherencia y pérdida de paquetes demostraron por qué es necesario aplicar técnicas como **framing** y **checksum**.

En suma, la aplicación resultante es más **eficiente y robusta**, capaz de manejar errores y recuperar la sincronización automáticamente, simulando mecanismos que se usan en protocolos industriales y de telecomunicaciones.

---

### Código final (p5.js modificado)

```js
'use strict';

let port;
let connectBtn;
let connectionInitialized = false;
let microBitConnected = false;

let serialBuffer = []; // buffer de bytes

let microBitX = 0;
let microBitY = 0;
let microBitAState = false;
let microBitBState = false;
let prevMicroBitBState = false;

var strokeColor;

const STATES = {
  WAIT_MICROBIT_CONNECTION: "WAIT_MICROBIT_CONNECTION",
  RUNNING: "RUNNING",
};
let appState = STATES.WAIT_MICROBIT_CONNECTION;

function setup() {
  createCanvas(720, 720);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(2);
  strokeColor = color(0, 10);

  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(0, 0);
  connectBtn.mousePressed(() => connectBtnClick('emu'));
}

function draw() {
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
    appState = STATES.WAIT_MICROBIT_CONNECTION;
  } else {
    microBitConnected = true;
    connectBtn.html("Disconnect");
    appState = STATES.RUNNING;

    if (port.opened() && !connectionInitialized) {
      port.clear();
      connectionInitialized = true;
    }

    // === lectura de datos binarios ===
    readSerialData();
  }

  switch (appState) {
    case STATES.WAIT_MICROBIT_CONNECTION:
      break;

    case STATES.RUNNING:
      if (microBitAState) {
        push();
        translate(width / 2, height / 2);

        var circleResolution = int(map(microBitY + 100, 0, height, 2, 10));
        var radius = microBitX - width / 2;
        var angle = TAU / circleResolution;

        stroke(strokeColor);

        beginShape();
        for (var i = 0; i <= circleResolution; i++) {
          var x = cos(angle * i) * radius;
          var y = sin(angle * i) * radius;
          vertex(x, y);
        }
        endShape();

        pop();
      }

      if (prevMicroBitBState === true && microBitBState === false) {
        background(0, 0, 100);
        print("B released → borrado");
      }
      prevMicroBitBState = microBitBState;
      break;
  }
}

function readSerialData() {
  let available = port.availableBytes();
  if (available > 0) {
    let newData = port.readBytes(available);
    serialBuffer = serialBuffer.concat(newData);
  }

  while (serialBuffer.length >= 8) {
    // buscar header
    if (serialBuffer[0] !== 0xaa) {
      serialBuffer.shift();
      continue;
    }

    if (serialBuffer.length < 8) break;

    let packet = serialBuffer.slice(0, 8);
    serialBuffer.splice(0, 8);

    let dataBytes = packet.slice(1, 7);
    let receivedChecksum = packet[7];
    let computedChecksum = dataBytes.reduce((a, b) => a + b, 0) % 256;

    if (computedChecksum !== receivedChecksum) {
      console.log("Checksum error");
      continue;
    }

    let buffer = new Uint8Array(dataBytes).buffer;
    let view = new DataView(buffer);
    microBitX = view.getInt16(0) + width / 2;
    microBitY = view.getInt16(2) + height / 2;
    microBitAState = view.getUint8(4) === 1;
    microBitBState = view.getUint8(5) === 1;
  }
}

function keyReleased() {
  if (keyCode == DELETE || keyCode == BACKSPACE) background(0, 0, 100);
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');

  if (key == '1') strokeColor = color(0, 10);
  if (key == '2') strokeColor = color(192, 100, 64, 10);
  if (key == '3') strokeColor = color(52, 100, 71, 10);
}

function connectBtnClick(mode = 'emu') {
  if (!port.opened()) {
    if (mode === 'micro') {
      port.open('MicroPython', 115200);
    } else {
      port.open(115200);
    }
    connectionInitialized = false;
  } else {
    port.close();
  }
}
```

## Adicional: Funcionamiento de aplicación de puertos seriales para simular el micro:bit 

### Versión inicial (código proporcionado por el profesor para anterior actividad)

En la primera versión, el programa proporcionado por el profesor funcionaba de la siguiente manera:

* Se usaba la librería `serialport` para abrir el puerto.
* Con Express se montaba una API que permitía modificar manualmente los valores (`xf`, `yf`, `aState`, `bState`).
* Cada cierto tiempo (dependiendo de la frecuencia indicada con `--hz`), se escribía en el puerto serial una línea de texto con el siguiente formato:

```js
xf,yf,aState,bState
```

Ejemplo:

```js
120,-45,True,False
```

**Ventaja:** era fácil de leer y de depurar en consola.
**Desventaja:** no era compatible con el código del micro\:bit, que envía datos en binario, no en texto. Esto hacía que otro programa que esperara los paquetes originales no pudiera entender lo que estábamos mandando.

---

### Problemas detectados

* **Incompatibilidad con micro\:bit real:** el micro\:bit utiliza un protocolo binario con enteros de 16 bits (acelerómetro) y 8 bits (botones).
* **Ambigüedad en los datos:** al enviar texto, era más lento y se necesitaban separadores (comas, saltos de línea).
* **Sin control de integridad:** no había *checksum*, por lo que no se podía detectar si algún byte se corrompía.

---

### Versión final (código 2)

En la segunda versión, se corrigieron los puntos anteriores y el programa evolucionó de la siguiente manera:

1. **Creación de buffer binario**
   En vez de enviar texto plano, se construyó un `Buffer` de 6 bytes:

   * `xf`: entero con signo de 16 bits (2 bytes).
   * `yf`: entero con signo de 16 bits (2 bytes).
   * `aState`: boolean convertido a 1 o 0, en un byte.
   * `bState`: boolean convertido a 1 o 0, en un byte.

   Ejemplo de código:

   ```js
   buf.writeInt16BE(sensorValues.xf, 0);
   buf.writeInt16BE(sensorValues.yf, 2);
   buf.writeUInt8(sensorValues.aState ? 1 : 0, 4);
   buf.writeUInt8(sensorValues.bState ? 1 : 0, 5);
   ```

2. **Implementación de *checksum***
   Se sumaron todos los bytes del `Buffer` y se calculó el módulo 256 para obtener un único byte de verificación.

   ```js
   let checksum = 0;
   for (let i = 0; i < buf.length; i++) checksum = (checksum + buf[i]) & 0xFF;
   ```

3. **Estructura del paquete**
   Finalmente, cada envío se componía de:

   * **Header** fijo `0xAA` → indica el inicio de paquete.
   * **Payload** (`xf`, `yf`, `aState`, `bState`) → 6 bytes en binario.
   * **Checksum** (1 byte) → para validar la integridad.

   ```js
   const packet = Buffer.concat([Buffer.from([0xAA]), buf, Buffer.from([checksum])]);
   port.write(packet);
   ```

---

### Conclusiones

* **De texto a binario:** pasamos de un sistema simple (texto plano) a un sistema robusto (binario estructurado).
* **Compatibilidad:** ahora el programa manda exactamente los mismos paquetes que un micro\:bit real con Python (`struct.pack('>2h2B', ...)`).
* **Fiabilidad:** el checksum nos permite verificar que los datos lleguen completos y sin errores.
* **Próximo paso:** hacer pruebas con receptores reales o simuladores para verificar que los datos son interpretados correctamente.

---

### Código para lectura del programa en sistema binario en base a la actividad

**Vamos a tomar de referencia [esta aplicación](https://github.com/juanferfranco/serialEmulator)**

En el archivo de `emulator.js` vamos a cambiar lo que está dentro del archivo por el siguiente:

```js
const { SerialPort } = require('serialport');
const express = require('express');
const path = require('path');

// ---- CLI: --port=COM10 --baud=115200 --hz=10
const args = Object.fromEntries(process.argv.slice(2).map(s => {
  const [k, v] = s.replace(/^--/, '').split('=');
  return [k, v ?? true];
}));

const PORT = args.port || 'COM10';
const BAUD = parseInt(args.baud || '115200', 10);
const HZ   = parseFloat(args.hz   || '10'); // Hz → frecuencia de envío

// Estado global
let sensorValues = {
  xf: 0,
  yf: 0,
  aState: true,
  bState: false
};

// Servidor web
const app = express();
app.use(express.json());
app.use(express.static(path.join(__dirname, 'public')));

app.get('/api/values', (req, res) => res.json(sensorValues));

app.post('/api/values', (req, res) => {
  const { xf, yf, aState, bState } = req.body;
  if (xf !== undefined) sensorValues.xf = Math.max(-1200, Math.min(1200, parseInt(xf)));
  if (yf !== undefined) sensorValues.yf = Math.max(-1200, Math.min(1200, parseInt(yf)));
  if (aState !== undefined) sensorValues.aState = Boolean(aState);
  if (bState !== undefined) sensorValues.bState = Boolean(bState);
  res.json(sensorValues);
});

app.listen(3000, () => {
  console.log('[WEB] Interfaz en http://localhost:3000');
});

// Puerto serie
const port = new SerialPort({ path: PORT, baudRate: BAUD });

port.on('open', () => {
  console.log(`[OK] Abierto ${PORT} @ ${BAUD} baud`);
  const period = 1000 / HZ;

  const timer = setInterval(() => {
    // Crear buffer con el formato >2h2B
    const buf = Buffer.alloc(6);
    buf.writeInt16BE(sensorValues.xf, 0);      // xf en 2 bytes
    buf.writeInt16BE(sensorValues.yf, 2);      // yf en 2 bytes
    buf.writeUInt8(sensorValues.aState ? 1 : 0, 4); // aState en 1 byte
    buf.writeUInt8(sensorValues.bState ? 1 : 0, 5); // bState en 1 byte

    // Calcular checksum
    let checksum = 0;
    for (let i = 0; i < buf.length; i++) checksum = (checksum + buf[i]) & 0xFF;

    // Paquete completo: header + payload + checksum
    const packet = Buffer.concat([Buffer.from([0xAA]), buf, Buffer.from([checksum])]);

    port.write(packet, err => {
      if (err) console.error('[ERROR write]:', err.message);
    });
  }, period);

  port.on('close', () => {
    clearInterval(timer);
    console.log('[INFO] Puerto cerrado.');
  });
});

port.on('error', err => {
  console.error('[ERROR serial]:', err.message);
  process.exit(1);
});

```

Esto va a hacer que el programa ahora funcione enviando datos en el siguiente formato:

```mathematica
[0xAA] [xfH] [xfL] [yfH] [yfL] [aState] [bState] [checksum]
```
---

## NOTA

**Para hacer que el programa funcione (tanto el proporcionado por el profesor como el nuevo), es necesario realizar un cambio en la configuración de la bios, habilitando el `Secure Boot` e instalando dependencias que el programa requiere, hazlo bajo tu responsabilidad**

---




















