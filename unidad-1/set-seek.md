# Unidad 1

## 🔎 Fase: Set + Seek

## Actividad 1
### ¿Que es un sistema físico interactivo?

Es un sistema que se podría decir que es una especie de sistema hibíbrido, puesto que este hace uso de componentes físicos de entrada (por ejemplo sensores, dispositivos electrónicos, entre muchos otros) y los combina con el software de una computadora y un sistema, permitiendo que el usuario o un entorno interactue mediante los estímulos para poder así, generar una experiencia en donde el usuario se vea implicado de mayor forma en lo que ocurre dentro del sistema.

### ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

Desde el entretenimiento digital se pueden aplicar el uso de sistemas interactivos en una infinidad de formas, por ejemplo en el caso de videojuegos, se pueden hacer productos que puedan funcionar mediante senssore, actualment existen una gran cantidad de juegos que hacen uso de sistemas externos por ejemplo las cámaras para hacer que los videojuegos tengan funciones que hagan las experiencias mucho más inversivas, en otro caso, desde el area de experiencias interactivas, cuya idea se basa en crear experiencias que generen un gran impacto en las personas, desde el uso de sistemas físicos, haciendo uso de hardware se pueden crear sistemas que permitan a los usuarios tener interacciones con un sistema virtual que no se limita a algo físico solamente.

## Actividad 2

### ¿Qué es el diseño/arte generativo?
El arte generativo se refiere a cualquier práctica artística en la que el artista utiliza un sistema, como un conjunto de reglas de lenguaje natural, un programa informático, una máquina u otra invención procedimental, que se pone en marcha con cierto grado de autonomía, contribuyendo así a una obra de arte completa o dando como resultado una obra de arte completa.

### ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?



## Actividad 3


### En este sistemas físico interactivo identifica los inputs, outputs y el proceso.

Por parte de los inputs se pueden identificar los botones del micro:bit, el sensor de movimiento, 

Outputs: LEDs


## Actividad 4

### Código

```js
let t = 0;
let a1, f1, s1;
let a2, f2, s2;
let col1, col2;

function setup() {
  createCanvas(400, 400);
  background(20);
  strokeWeight(2);

  generarFunciones();
}

function draw() {
  translate(width / 2, height / 2);

  let x1 = sin(t * f1) * a1;
  let y1 = cos(t * f1) * s1;

  let x2 = cos(t * f2) * a2;
  let y2 = sin(t * f2) * s2;

  stroke(col1);
  point(x1, y1);

  stroke(col2);
  point(x2, y2);

  t += 0.01;

  if (frameCount % 300 === 0) {
    generarFunciones();
  }
}

function generarFunciones() {
  a1 = random(50, 180);
  f1 = random(0.5, 2);
  s1 = random(50, 180);

  a2 = random(50, 180);
  f2 = random(0.5, 2);
  s2 = random(50, 180);
  
  col1 = color(random(100, 255), random(100, 255), random(100, 255), 150);
  col2 = color(random(100, 255), random(100, 255), random(100, 255), 150);
}
```

<img width="399" height="399" alt="image" src="https://github.com/user-attachments/assets/892e9165-fae1-4d37-b077-7049b89c6570" />

[Enlace al Editor](https://editor.p5js.org/KiwisCas/sketches/EFciMr1Fk)
