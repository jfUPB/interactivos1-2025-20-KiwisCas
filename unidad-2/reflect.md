# Unidad 2


## 🤔 Fase: Reflect

### Actividad 6

#### Parte 1: recuperación de conocimiento (Retrieval Practice)

- Describe con tus palabras qué es una máquina de estados. ¿Cuáles son sus cuatro componentes fundamentales que has utilizado en esta unidad?
  Es un modelo computacional el cual representa un sistema que puede alternar entre diferentes estados y cambiar entre estos en respuesta a eventos.

  La máquina se compone de:
    - Estados: Son los que representan las condiciones o situaciones en la que puede mostrarse un sistema.
    - Transiciones: Estos indican cómo el sistema cambia de un estado a otro. Las transiciones están asociadas a eventos que desencadenan en el cambio de estado.
    - Eventos: Son las señales o estímulos que causan que el sistema cambie de estado. Pueden ser externos, es decir, provocados por el entorno o internos, es decir que son causados por el propio sistema.
    - Acciones: Son operaciones que se realizan cuando el sistema cambia de estado al entrar o salir de uno de estos.

  
- Explica por qué la técnica de máquina de estados es tan útil para gestionar la “concurrencia” (atender un temporizador y botones “al mismo tiempo”) en un dispositivo con un solo hilo de ejecución como el micro:bit. ¿Qué problema soluciona en comparación con usar funciones como sleep()?

  
- Imagina que tienes que añadir una nueva funcionalidad a la bomba temporizada: si se agita (shake) el micro:bit mientras la cuenta regresiva está activa, el tiempo se reduce a la mitad. ¿Cómo modificarías tu diagrama de máquina de estados para incluir este nuevo evento y acción?

  
- Explica qué es un “vector de prueba” y por qué es una herramienta crucial para verificar que una máquina de estados funciona como se espera.
