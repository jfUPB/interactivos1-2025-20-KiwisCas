# Unidad 1

## 🛠 Fase: Apply

### Actividad 5

### Actividad 6
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

[LINK AL P5.JS](https://editor.p5js.org/KiwisCas/sketches/H-XH3ZV-d)
