# Unidad 3


## 🛠 Fase: Apply

### Actividad 6 y 7

En caso de acceder al código en p5.js accede al siguiente [Link](https://editor.p5js.org/KiwisCas/sketches/rQRvK4rBB)

#### Código Bomba 3.0 en p5.js con conexión a micro:bit 

```js
let bomb;
let port;
let connectBtn;


function setup(){
  createCanvas(400,400);
  textAlign(CENTER,CENTER);
  textSize(32);
  port = createSerial();
  connectBtn = createButton('Connect To Microbit')
  connectBtn.position(130,300)
  connectBtn.mousePressed(connectBtnClick);
  
  bomb = new Bomb()
}

function draw(){
  background(0);
  bomb.update();
  bomb.display();
  microBomb();
}

function keyPressed(){
  if(key === 'A') bomb.handleEvent('A');
  else if(key === 'B') bomb.handleEvent('B');
  else if(key === 'S') bomb.handleEvent('S');
  else if(key === 'T') bomb.handleEvent('T');
}

function connectBtnClick() {
  
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        connectBtn.html('Disconnect');
      
    }
}

function microBomb(){
  if(port.availableBytes() > 0){
    let dataRx = port.read(1);
    if(dataRx === 'A') bomb.handleEvent('A');
    else if (dataRx === 'B') bomb.handleEvent('B');
    else if (dataRx === 'S') bomb.handleEvent('S');
    else if (dataRx === 'T') bomb.handleEvent('T');
  }
  if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    }
    else {
        connectBtn.html('Disconnect');
    }
}


class Bomb {
  constructor(){
    this.PASSWORD = ['A','B','A'];
    this.key =[];
    this.count = 20;
    this.startTime = millis();
    this.state = 'CONFIG';
  }
  
  handleEvent(e){
    if(this.state === 'CONFIG'){
      if (e === 'A') this.count = min(this.count + 1, 60);
      else if(e === 'B') this.count = max(this.count - 1, 10);
      else if(e === 'S'){
        this.startTime = millis();
        this.state = 'ARMED';
      }    
    }
    else if(this.state === 'ARMED'){
      if(e === 'A' || e === 'B'){
        this.key.push(e);
        if(this.key.length === this.PASSWORD.length){
          if(this.key.join('') === this.PASSWORD.join('')){
            this.state = 'CONFIG';
            this.count = 20;
          }
          this.key = [];
        }
      }
    }
    else if (this.state === 'EXPLODED'){
      if (e === 'T'){
        this.state = 'CONFIG';
        this.count = 20;
        this.startTime = millis();
      }
    }
  }
  
  update(){
    if(this.state === 'ARMED'){
      if(millis() - this.startTime > 1000){
        this.count--;
        this.startTime = millis();
        if (this.count <= 0){
          this.state = 'EXPLODED';
        }
      }
    }
  }
  
  
  display(){
    fill(255);
    if (this.state === 'CONFIG') {
      text(`CONFIG\n${this.count}`, width/2, height/2);
    } else if (this.state === 'ARMED') {
      text(`ARMED\n${this.count}`, width/2, height/2);
    } else if (this.state === 'EXPLODED') {
      fill(255, 0, 0);
      text("EXPLODED ", width/2, height/2);
    }
  } 
  
}
```


#### Código Micro:Python

```py
# Imports go at the top
from microbit import *

uart.init(baudrate=115200)
display.show(Image.HAPPY)

while True:
    display.show(Image.HAPPY)
    if button_a.was_pressed():
        uart.write('A')
        display.show(Image.ARROW_N)
        sleep(200)
    if button_b.was_pressed():
        uart.write('B')
        display.show(Image.ARROW_S)
        sleep(200)
    if accelerometer.was_gesture('shake'):
        uart.write('S')
        display.show(Image.SAD)
        sleep(200)
    if pin_logo.is_touched():
        uart.write('T')
        display.show(Image.HAPPY)
        sleep(200)
```

