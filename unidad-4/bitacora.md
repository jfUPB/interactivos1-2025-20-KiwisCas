# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modificar](https://editor.p5js.org/generative-design/sketches/P_2_0_03)

Código a modificar:
### HTML

``` html
<!DOCTYPE html>
<html>
  <head>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/p5.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/addons/p5.dom.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/addons/p5.sound.min.js"></script>

    <!-- Generative Design Dependencies here -->
    <!-- GG Bundled -->
    <script src="https://cdn.jsdelivr.net/gh/generative-design/Code-Package-p5.js@master/libraries/gg-dep-bundle/gg-dep-bundle.js"></script>
    <!-- Opentype -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/opentype.js/0.7.3/opentype.min.js"></script>
    <!-- Rita -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/rita/1.3.11/rita-small.min.js"></script>
    <!-- Chroma -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/chroma-js/1.3.6/chroma.min.js"></script>
    <!-- Jquery -->
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"
      integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
      crossorigin="anonymous"></script>

    <!-- sketch additions -->

    <link rel="stylesheet" type="text/css" href="style.css">
  </head>
  <body>

    <!-- main -->
    <script src="sketch.js"></script>
  </body>
</html>


```


### JS

``` js

'use strict';

var strokeColor;

function setup() {
  createCanvas(720, 720);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(2);
  strokeColor = color(0, 10);
}

function draw() {
  if (mouseIsPressed && mouseButton == LEFT) {
    push();
    translate(width / 2, height / 2);

    var circleResolution = int(map(mouseY + 100, 0, height, 2, 10));
    var radius = mouseX - width / 2;
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
}

function keyReleased() {
  if (keyCode == DELETE || keyCode == BACKSPACE) background(0, 0, 100);
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');

  if (key == '1') strokeColor = color(0, 10);
  if (key == '2') strokeColor = color(192, 100, 64, 10);
  if (key == '3') strokeColor = color(52, 100, 71, 10);
}


```

[Enlace a la aplicación modificada](https://editor.p5js.org/KiwisCas/sketches/FRmIBe_JP)

Código modificado:

### HTML

``` html
<!DOCTYPE html>
<html>
  <head>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/p5.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/addons/p5.dom.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/addons/p5.sound.min.js"></script>

    <!-- Generative Design Dependencies here -->
    <!-- GG Bundled -->
    <script src="https://cdn.jsdelivr.net/gh/generative-design/Code-Package-p5.js@master/libraries/gg-dep-bundle/gg-dep-bundle.js"></script>
    <!-- Opentype -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/opentype.js/0.7.3/opentype.min.js"></script>
    <!-- Rita -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/rita/1.3.11/rita-small.min.js"></script>
    <!-- Chroma -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/chroma-js/1.3.6/chroma.min.js"></script>
    <!-- Jquery -->
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"
      integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
      crossorigin="anonymous"></script>
    <script src="https://unpkg.com/@gohai/p5.webserial@^1/libraries/p5.webserial.js"></script>

    <!-- sketch additions -->

    <link rel="stylesheet" type="text/css" href="style.css">
  </head>
  <body>

    <!-- main -->
    <script src="sketch.js"></script>
  </body>
</html>

```

### JS
``` js

'use strict';

let port;
let connectBtn;
let connectionInitialized = false;
let microBitConnected = false;

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

    if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length == 4) {
          microBitX = int(values[0]) + width / 2;
          microBitY = int(values[1]) + height / 2;
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";
        }
      }
    }
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

## Video

[Video demostratativo](https://youtu.be/LEghixJ9H4Y)


