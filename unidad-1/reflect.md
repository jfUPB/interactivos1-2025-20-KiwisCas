# Unidad 1

## 🤔 Fase: Reflect

### Actividad 7

### Parte 1

#### 1. Basándote en los ejemplos que vimos de sistemas físicos interactivos al iniciar el curso, describe las tres características que definen a un sistema físico interactivo.

Son sistemas en los cuales tanto el sistema como el humano actúan por igual, hay una constante cantidad de interacciones por parte del humano y la máquina, son sistemas que se encargan de transmitir obras de distintas formas (arte visual, sonido, etc.) mediante instrucciones realizadas por el humano.

#### 2. Explica el modelo input-process-output de Patrick Hübner usando como ejemplo el sistema que construiste con micro:bit y p5.js en la Actividad 06. ¿Cuál fue el input, cuál fue el proceso y cuál fue el output?

El sistema realizado en la actividad 6 era un sistema que permitía que el usuario al conectar el micro:bit al programa, cada vez que este recibía una información de los botones del micro:bit, un circulo cambiaba su posición en el eje X, el programa empieza desde los dispositivos de entrada en los cuales podemos incluir el botón de connect to micro:bit que está en el código, los botones A y B ya que estos cuando son presionados se envía información al computador.

Por parte de los outputs podemos encontrar el programa, ya que este genera un circulo en el centro del canvas y que cuando este programa se conecta al micro:bit para luego empezar a enviar información al computador a través del puerto serial, el programa hace que el circulo dependiendo de la información recibida se mueva tanto a la izquierda como a la derecha y adicionalmente cambie de color.

#### 3. ¿Cuál es la función de la línea uart.write('A') en el código del micro:bit y qué función en p5.js se encarga de “escuchar” ese mensaje?

_uart.write('A')_ se encarga de enviar al sistema la letra 'A' para que luego, después de que el sistema verifique entrada de datos por medio de la linea ``` port.availableBytes() ``` para luego leer los puertos a través de ``` port.read() ```, eso si, el puerto de entrada de datos debe de estar abierto.

#### 4. ¿Cuál es la diferencia fundamental entre el arte/diseño tradicional y el arte/diseño generativo?

Que el arte tradicional requiere completamente de la influencia y mano humana, el arte hecho en lienzo o digital requiere completamente de hacer todo por tu cuenta desde el inicio, en cambio, el diseño o arte generativo complementa las acciones humanas incluso realizadas en una disciplina artística y las amplifica, los sonidos pueden convertirse y ser arte visual generado por ayuda de la computadora, también puede aplicarse en viceversa, podría decirse que el arte generativo puede ser una extensión de lo generado por un humano que por medio del código y su su propia imaginación.

#### 5. Imagina que quieres que un círculo en p5.js cambie a un color aleatorio cada vez que sacudes el micro:bit. Describe qué tendrías que programar en el micro:bit y qué tendrías que programar en p5.js para lograrlo.

inicialmente, en el micro:bit tendría que hacer que el sistema cada vez que se sacuda el microbit envíe una señal, podría ser una letra, luego de esto en p5.js haría inicialmente una variable global que se encargue de generar códigos de color al azar por medio de la función ``` color() ```, luego, en la parte de setup, un botón que conecte el sistema con el micro:bit y abra el puerto de entrada de datos, luego haría el canvas, posterior a eso en draw(), validaría la entrada de datos del micro:bit y para que luego el sistema lo lea y haga que cada vez que el sistema recibe la entrada de datos a modo de la letra establecida, y que cada vez que esta letra sea recibida se llame a la variable y cree un nuevo código, finalmente haría que el código dibuje el circulo en la parte de para que este se actualice.

### Parte 2

#### 1. ¿Qué fue más desafiante para ti en esta unidad: la parte conceptual (entender qué es un sistema físico interactivo) o la parte técnica (hacer que el micro:bit y p5.js se comunicaran)? ¿Por qué?

Sinceramente, me pareció un poco compleja la parte teórica, la verdad es que yo soy alguien que entiende más por la parte de la práctica, me entretiene un poco más, yo soy un poco malo por la parte de aprender conceptos a modo teórico y un poco más si no es tan directo, pero me parecen formas muy interesantes de aprender.

#### 2. Describe el momento “¡Aha!” que tuviste cuando lograste que una acción en el micro:bit (presionar un botón, sacudirlo) tuviera un efecto visible en el canvas de p5.js por primera vez. ¿Qué fue lo que entendiste en ese instante?

La verdad, fue en la clase en la que hicimos el circulo que se movía, si bien en la primera clase que trabajamos con el micro:bit me pareció algo extremadamente expectacular, el hecho de haber realizado la conexión y el código por mi propia cuenta me pareció magnifico y me emocionó mucho, se me hizo muy interesante la parte del movimiento del circulo, entendí en ese instante como es que la información se envía del micro:bit al computador y posterior a eso como ocurre la interpretación de los datos.

#### 3. Al inicio de la unidad te pregunté: “¿Este curso para qué me sirve?”. Después de experimentar y construir tu primer prototipo, ¿Cómo ha cambiado o se ha vuelto más concreta tu respuesta a esa pregunta?

Sigo firme a mi pensamiento al inicio de semestre, sin embargo, esto me ayuda a entender que los sistemas físicos poseen una profundidad mucho más compleja y que estos pueden tener impactos mucho más interesantes de lo que pensaba en un inicio.

#### 4. El tutorial de la Actividad 05 te llevó paso a paso. ¿Cómo te sentiste con ese método de aprendizaje? ¿Te dio seguridad o preferirías haberlo intentado por tu cuenta desde el principio?

Me parece bien la forma en la que se hizo la actividad puesto que hay una gran cantidad de personas del curso que no tienen muchos conocimientos de programación en JavaScript 









