# Unidad 1

## 🛠 Fase: Apply

### Actividad 5

#### 1. _INPUTS:_ Clic en el botón "Connect to micro:bit" en la página, Pusar el boton 'A', Envío de caracteres 'A' o 'N' a través del puerto serial de acuerdo con la acción realizada.
#### 2. _OUTPUTS:_ En la interfaz p5.js debe aparecer un rectángulo en el centro que cambia de color según el código, rojo o verde, adicionalmente debe aparecer el "Connect to micro:bit" o "Disconnect" según el estado del dispositivo.
#### 3. _PROCESO:_ Al iniciar el programa, se dibuja en la pantalla del computador un botón que dice "Connect to micro:bit", posterior a esto, el usuario presiona el botón para iniciar la conexión serial con el micro:bit.
#### - Si la conexión se realiza de forma correcta se limpia el buffer de datos y la interfaz se mantiene revisando constantemente en busqueda de nuevos datos.
#### - Si el micro:bit envía el carácter 'A' *(En este caso, el código que fue enviado al micro:bit dice que cada vez que se presione el botón 'A' el microbit va a enviar el caracter 'A')*, el sistema detectará el caracter recibido y pinta un cuadro rojo en pantalla mientras se esté presionando el botón y siga enviando 'A'.
#### - Si no se recibe el caracter 'A' del micro:bit, el sistema muestra el cuadro verde como estado por defecto.
#### - El botón cambia su texto dinámicamente entre "Connect to micro:bit" y "Disconnect", según el estado de la conexión


### Actividad 6

#### Enlace al Editor 💻
[LINK AL P5.JS](https://editor.p5js.org/KiwisCas/sketches/H-XH3ZV-d)


#### Código del programa:

```js 
let port;
let connectBtn;
let connectionInitialized = false;
let dirX = 200;

function setup() {
  createCanvas(400, 400);
  background(200);
  port = createSerial();
  connectBtn = createButton('Connect to micro:bit');
  connectBtn.position(80,300);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);
  
  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }
  
  if (port.availableBytes() > 0 ){
    let dataRx = port.read(1);
    if (dataRx == 'A' && dirX > 50){
      dirX -= 10;
      fill("red")
    }
    else if (dataRx == 'B' && dirX < 350){
      dirX += 10;
      fill("blue")
    } 
  }
    
    ellipseMode(RADIUS);
    ellipse(dirX, height / 2, 50, 50);
    if (!port.opened()){
      connectBtn.html("Connect to Micro:bit");
    } else{
      connectBtn.html("Disconnect");
    }
  
  text(dirX, 50, 50)
}

function connectBtnClick(){
  if(!port.opened()){
    port.open('MicroPython', 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}
```
#### Código del Micro:bit

```py
# Imports go at the top
from microbit import *
uart.init(baudrate=115200)

display.show(Image.HAPPY)


# Code in a 'while True:' loop repeats forever
while True:
    if (button_a.is_pressed()):
        uart.write('A')
        
    if (button_b.is_pressed()):
        uart.write('B')
        
    else:
        uart.write('N')

    sleep(100)
    

```

