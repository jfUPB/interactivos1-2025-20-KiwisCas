
# 📘 Evidencias de la Unidad 5

---

## 🔹 Set

### 📌 Preguntas caso de estudio

#### 1. Describe cómo se están comunicando el micro\:bit y el sketch de p5.js. ¿Qué datos envía el micro\:bit?

El **micro\:bit** y el sketch de **p5.js** establecen comunicación a través del **puerto serial**. En este proceso:

1. Desde el micro\:bit se seleccionan los datos que se quieren enviar:

   * Valores del **acelerómetro en los ejes X e Y**.
   * Estados de los **botones A y B**.
2. Se organiza la información en un **formato ASCII**, separando cada dato con comas (`,`) y usando un salto de línea (`\n`) para marcar el fin del mensaje.
3. En p5.js, tras abrir el puerto serial, se reciben los datos como texto.
4. Mediante funciones de limpieza (`trim`) y separación (`split`), los datos se fragmentan en partes y se asignan a variables individuales (`microBitX`, `microBitY`, `microBitAState`, `microBitBState`).

En conclusión, el micro\:bit envía en **formato ASCII** valores del acelerómetro y estados de los botones, que p5.js procesa para utilizarlos en la lógica gráfica.

---

#### 2. ¿Cómo es la estructura del protocolo ASCII usado?

El mensaje enviado sigue la siguiente estructura:

```
xValue,yValue,aState,bState\n
```

* **Delimitador de campos:** la coma (`,`) separa cada dato.
* **Delimitador de paquete:** el salto de línea (`\n`) indica el fin de un conjunto de datos.
* **Codificación:** cada número o estado (`True` / `False`) se transforma en caracteres ASCII.

Ejemplo en memoria:

```
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

## 🔹 Seek

### 📌 Captura de aplicación de conexión serial con datos binarios

Cuando se pasa de enviar datos en **ASCII** a **binario**, el monitor serial en modo texto muestra caracteres extraños:

<img width="985" height="172" alt="image" src="https://github.com/user-attachments/assets/7ea76a78-5bf4-49c3-b915-3c1e2acb73db" />  

Esto ocurre porque:

* Los bytes transmitidos (0–255) no siempre representan caracteres válidos en ASCII o UTF-8.
* El sistema intenta interpretarlos como letras, pero no tienen equivalencia → aparecen símbolos ilegibles.

En cambio, al visualizar en modo **hexadecimal**:

<img width="902" height="196" alt="image" src="https://github.com/user-attachments/assets/72a912f5-a03a-4e12-a03c-05ab4f98f30c" />  

Los datos se muestran **exactamente como fueron enviados**: secuencias de bytes hexadecimales que sí reflejan la realidad de la transmisión.

---

### 📌 Empaquetado con `struct.pack`

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

```
ff d8 00 98 01 00
```

---

### 📌 Ejemplo modificado con gesto *shake*

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

### 📌 Representación de números negativos con complemento a dos

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

```
2020,2020,0,0 → 07 E4 07 E4 00 00
-2020,-2020,0,0 → F8 1C F8 1C 00 00
```

---

### 📌 Comparación Binario vs ASCII

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

### 📌 Evidencias gráficas

 <img width="1919" height="949" alt="image" src="https://github.com/user-attachments/assets/c0a79e7d-d89b-44d8-b23e-6df997281f1d" />  
 <img width="1919" height="894" alt="image" src="https://github.com/user-attachments/assets/0b1413ee-8c4f-4e8e-9256-fed58891ebb8" />  
 <img width="1919" height="849" alt="image" src="https://github.com/user-attachments/assets/12f7c302-b709-4183-b822-5c5a8886a84d" />  
   
 <img width="1919" height="893" alt="image" src="https://github.com/user-attachments/assets/f7a602bf-0fc8-49dd-88f2-c7dead237e38" />  

---

### 📝 Actividad 03

**Caso de estudio: p5.js**
Tema central: Modificación del código de micro\:bit y p5.js para soportar lectura de datos en **formato binario con framing y verificación de integridad**.

---

### 🔎 Explicación inicial

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

```
xValue = 500  →  01 f4
yValue = 524  →  02 0c
aState = 1    →  01
bState = 0    →  00

Paquete = 01 f4 02 0c 01 00
```

---

### 🔧 Cambios en el código

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

   ```
   microBitX: 500 microBitY: 524 ...
   microBitX: 500 microBitY: 513 ...
   microBitX: 3073 microBitY: 1 ...
   ```

   <img width="1919" height="849" alt="image" src="https://github.com/user-attachments/assets/12f7c302-b709-4183-b822-5c5a8886a84d" />

   Esto ocurre porque los 6 bytes no siempre llegan alineados. El puerto serie entrega datos en **fragmentos arbitrarios**, y p5.js puede leer bytes que pertenecen a **dos paquetes distintos** → error de sincronización.

4. **Solución aplicada: Framing + Checksum**

   * Se añade un **byte de inicio (0xAA)** para identificar el comienzo del paquete.
   * Se añaden los 6 bytes de datos.
   * Se agrega un **checksum** (suma de los bytes de datos módulo 256) para validar la integridad.

   Ahora el paquete tiene **8 bytes**:

   ```
   [Header][xValue][yValue][aState][bState][Checksum]
   0xAA    01 f4   02 0c   01      00      ??
   ```

   En p5.js:

   * Se acumulan los bytes en un `serialBuffer`.
   * Se busca el `0xAA` como header.
   * Se verifica el checksum.
   * Solo si es válido, se extraen los valores.

---

### 🔬 Observaciones en consola

1. **Antes del framing:**

   * La consola mostraba datos correctos al inicio, pero luego aparecían lecturas erróneas como `microBitY: 513` o valores muy grandes (`3073`).
   * Esto confirma que los paquetes llegaban desalineados.

     <img width="1919" height="849" alt="image" src="https://github.com/user-attachments/assets/12f7c302-b709-4183-b822-5c5a8886a84d" />

2. **Con framing y checksum:**

   * La consola muestra datos estables y coherentes, incluso si se producen fragmentaciones en la transmisión.
   * Si llega un paquete corrupto, aparece un mensaje:

     ```
     Checksum error in packet
     ```
     <img width="1919" height="785" alt="image" src="https://github.com/user-attachments/assets/bd33cd25-0d4e-4871-b151-e331119db2d9" />
     lo cual permite detectar el problema sin dañar la lectura global.

3. **Cambios finales:**

   * El micro\:bit ahora envía datos binarios con header y checksum.
   * p5.js procesa un buffer de bytes, sincroniza con el header y valida integridad.
   * En consola ya no aparecen valores erráticos, lo que demuestra que el **framing resuelve los problemas de sincronización**.

---

### 📝 Actividad 04

**Aplicación práctica del protocolo binario con framing y checksum**
Tema central: Modificación de la aplicación de p5.js para que soporte el protocolo de datos binarios enviado por el micro\:bit, integrando header y checksum para asegurar sincronización e integridad.

---

### 🔎 Proceso de construcción

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

### 🧪 Experimentos y dificultades

Durante la implementación surgieron varios problemas que permitieron reforzar la comprensión:

1. **Valores incoherentes en pantalla:**
   En las primeras pruebas, los valores del acelerómetro se mostraban como `3073`, `-123` o números fuera de rango. Esto ocurrió porque los 6 bytes del paquete llegaban desalineados, lo que confirmaba la necesidad de usar el header para recuperar sincronización.

2. **Errores de checksum recurrentes:**
   Durante la validación, algunos paquetes eran rechazados con mensajes de `"Checksum error"`. Al revisar, se detectó que ciertos bytes se perdían en la transmisión o quedaban mezclados entre paquetes. La estrategia de descartar paquetes inválidos y esperar al próximo header resolvió el problema.

3. **Interpretación de números negativos:**
   Al inclinar el micro\:bit, algunos valores aparecían como números muy grandes en lugar de negativos. Esto llevó a verificar la forma en que `getInt16()` interpreta los enteros en **complemento a 2**. Una vez corregido el uso de big-endian, los valores negativos (ej. `-2020` representado como `f8 1c`) se mostraron correctamente.

4. **Pérdida de sincronización temporal:**
   En ocasiones, al desconectar y reconectar, el sistema tardaba en recuperar la alineación de los paquetes. La solución fue limpiar el buffer al establecer conexión y dejar que el algoritmo buscara nuevamente el header.

---

### 🔬 Observaciones finales

* Con el sistema ASCII inicial, los datos eran fáciles de leer pero poco eficientes.
* La transición a binario fijo de 6 bytes redujo el tamaño del paquete, pero expuso problemas de sincronización.
* La inclusión del **header** resolvió el problema de alineación, y el **checksum** permitió descartar paquetes corruptos sin afectar la aplicación.
* Al final, el dibujo generado en p5.js respondió correctamente a los movimientos y botones del micro\:bit, mostrando estabilidad en los valores recibidos.

---

### 📌 Conclusión

El desarrollo de esta actividad permitió comprender que, en comunicación serial:

* No basta con transmitir datos, es necesario diseñar un protocolo que asegure **longitud fija, sincronización e integridad**.
* El **complemento a 2** es fundamental para representar valores negativos en formato binario y debe interpretarse correctamente en el receptor.
* Los problemas iniciales de incoherencia y pérdida de paquetes demostraron por qué es necesario aplicar técnicas como **framing** y **checksum**.

En suma, la aplicación resultante es más **eficiente y robusta**, capaz de manejar errores y recuperar la sincronización automáticamente, simulando mecanismos que se usan en protocolos industriales y de telecomunicaciones.

---

### ✅ Código final (p5.js modificado)

```javascript
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













