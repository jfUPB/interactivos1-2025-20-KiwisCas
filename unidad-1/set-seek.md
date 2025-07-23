# Unidad 1

## 🔎 Fase: Set + Seek

## Actividad 1
### ¿Que es un sistema físico interactivo?

Es un sistema que se podría decir que es una especie de sistema hibíbrido, puesto que este hace uso de componentes físicos de entrada (por ejemplo sensores, dispositivos electrónicos, entre muchos otros) y los combina con el software de una computadora y un sistema, permitiendo que el usuario o un entorno interactue mediante los estímulos para poder así, generar una experiencia en donde el usuario se vea implicado de mayor forma en lo que ocurre dentro del sistema.

### ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

Desde el entretenimiento digital se pueden aplicar el uso de sistemas interactivos en una infinidad de formas, por ejemplo en el caso de videojuegos, se pueden hacer productos que puedan funcionar mediante senssore, actualment existen una gran cantidad de juegos que hacen uso de sistemas externos por ejemplo las cámaras para hacer que los videojuegos tengan funciones que hagan las experiencias mucho más inversivas, en otro caso, desde el area de experiencias interactivas, cuya idea se basa en crear experiencias que generen un gran impacto en las personas, desde el uso de sistemas físicos, haciendo uso de hardware se pueden crear sistemas que permitan a los usuarios tener interacciones con un sistema virtual que no se limita a algo físico solamente.

## Actividad 2

### ¿Qué es el diseño/arte generativo?
El arte generativo se refiere a cualquier práctica artística en la que el artista utiliza un sistema, como un conjunto de reglas de lenguaje natural, un programa informático, una máquina u otra invención procedimental, que se pone en marcha con cierto grado de autonomía, contribuyendo así a una obra de arte completa o dando como resultado una obra de arte completa. Podría decirse que es el tipo de arte que se genera cuando el humano y la máquina trabajan juntas, siendo el humano una especie de _"Compositor"_ y la máquina es quien se encarga de reproducir o pintar la obra con ayuda del compositor, lo que da paso a una obra en donde el humano acompañado por la máquina que es dirigida por este mismo por medio de código, pueden crear ilustraciones visuales haciendo uso de música o distintas prácticas para generar arte.

### ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

Teniendo en cuenta lo anterior, me parece que el arte generativo es una excelente herramienta para poder crear experiencias para usuarios que les permita interactuar con aspectos de su entorno y por medio de estos, realizar cambios en el entorno visual, proporcionando una experiencia visual acompañada de campos de audio que generen una inmersión en la actividad que se esté haciendo, me parece tambipen que es posible generar una posible guía para entornos, crear arte que pueda servir de inspiración para la creación de escenarios en videojuegos o animaciones, la verdad dudo un poco que se puedan tomar de forma cruda las imagenes generadas y que estas sean escenarios pero me parece que podría servir a forma de inicio para poder crear ideas para escenarios, al igual que generar partículas que puedan tener ya sea visuales o comportamientos generados por el programa y el sistema.

## Actividad 3


### En este sistemas físico interactivo identifica los inputs, outputs y el proceso.

Por parte de los inputs se pueden identificar los botones del micro:bit (Botón A y B), el sensor de movimiento que se activa al sacudir el micro:bit y el botón que dice _Send Love_ en el p5.js, su proceso es sencillo, el micro:bit detecta los botones que son presionados por el usuario, en este caso los físicos o el sensor, este envía los datos al computador para posteriormente ser recibidos por el computador y dentro de este el programa, el cual procesa la información recibida desde los dispositvos de entrada y proyecta la función determinada para cada entrada en la pantalla (colores y letras diferentes dependiendo de lo que se presione) por otra parte si se acciona en el computador el botón con la palabra "Send Love" el computador envía una orden al micro:bit, el cual muestra por medio de sus leds, lo especificado en el código, en este caso muestra una carita feliz y un corazón.

En terminos de los outputs podríamos poner al propio computador, puesto que en su pantalla se ven reflejados los cambios que se generan por los datos enviados desde el micro:bit, en el propio micro:bit también hay dispotivos de salida, en este caso serán los leds que sirven a modo de aviso de que el sistema está recibiendo ordenes desde el ordenador, desde el momento en que se conecta a este mostrando en sus leds una especie de mariposa y posteriormente cambiando cuando este recibe las ordenes en el momento en que se presiona el botón de "Send Love".


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
<img width="1817" height="887" alt="Captura de pantalla 2025-07-22 223910" src="https://github.com/user-attachments/assets/b17d8ed7-8e0b-4787-b96a-741ebff853ce" />

[Enlace al Editor por si quieres revisar todo el código :D ](https://editor.p5js.org/KiwisCas/sketches/EFciMr1Fk)
