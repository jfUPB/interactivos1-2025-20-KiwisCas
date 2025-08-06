# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1

#### Código documentado para entender :D

```py
# - utime: para manejar el tiempo con precisión en milisegundos
from microbit import *
import utime

# Definición de la clase Pixel: representa un solo LED de la pantalla que parpadea
class Pixel:
    # Constructor que se ejecuta al crear un nuevo Pixel
    def __init__(self, pixelX, pixelY, initState, interval):
        self.state = "Init"           # Estado inicial del objeto (una máquina de estados)
        self.startTime = 0            # Marca de tiempo en la que empieza a contar el intervalo
        self.interval = interval      # Intervalo de tiempo (en ms) para los cambios de estado del pixel
        self.pixelX = pixelX          # Posición horizontal (columna) del píxel en el microbit (es un cuadrado 5x5)
        self.pixelY = pixelY          # Posición vertical (fila) del píxel
        self.pixelState = initState   # Estado de brillo inicial del píxel (0 = apagado, 9 = encendido)

    # Método que se debe llamar repetidamente para actualizar el comportamiento del pixel
    def update(self):

        # Si el estado actual es "Init", esto solo se ejecuta una vez, al comienzo
        if self.state == "Init":
            self.startTime = utime.ticks_ms()  # Guarda el tiempo actual como referencia de inicio
            self.state = "WaitTimeout"         # Cambia al siguiente estado (espera a que pase el intervalo)
            display.set_pixel(self.pixelX, self.pixelY, self.pixelState)  # Muestra el estado inicial del píxel en pantalla

        # Si el estado es "WaitTimeout", se revisa si ya pasó el intervalo de tiempo
        elif self.state == "WaitTimeout":
            # Calcula la diferencia entre el tiempo actual y el de inicio
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > self.interval:
                self.startTime = utime.ticks_ms()  # Reinicia el tiempo de referencia para el próximo cambio

                # Alterna el estado del píxel: si estaba encendido (9), lo apaga (0); si estaba apagado, lo enciende
                if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9

                # Aplica el nuevo estado del pixel en los LEDs
                display.set_pixel(self.pixelX, self.pixelY, self.pixelState)


# Crea una instancia del pixel 1:
# - posición (0, 0)
# - estado inicial apagado (0)
# - intervalo de 1000 ms = 1 segundo
pixel1 = Pixel(0, 0, 0, 1000)

# Crea una instancia del pixel 2:
# - posición (4, 4)
# - estado inicial apagado (0)
# - intervalo de 500 ms = 0.5 segundos
pixel2 = Pixel(4, 4, 0, 500)

# Bucle principal que se ejecuta infinitamente
while True:
    # Actualiza ambos pixeles en cada paso del bucle
    pixel1.update()
    pixel2.update()
```


#### Funcionamiento del programa
##### 1. Clase Pixel
La clase `Pixel` modela un pixel que alterna entre encendido (valor 9) y apagado (valor 0), con un tiempo de intervalo (`interval`) entre cada cambio.

##### 2. Atributos importantes: 

- **`pixelX` y `pixelY`:** coordenadas del píxel en la matriz de LEDs (de 0 a 4).
- **`pixelState`:** estado actual del píxel (9 = encendido, 0 = apagado).
- **`interval`:** tiempo (en milisegundos) que debe pasar para alternar el estado del píxel.
- **`startTime`:** momento en que se inició el conteo para el cambio.
- **`state`:** estado de la máquina de estados del píxel (`"Init"` o `"WaitTimeout"`).

##### 3. Método "`update()`"

- Es llamado continuamente en el bucle `while True`.

- La primera vez que se llama, está en el estado `"Init"`:

    - Guarda el tiempo de inicio con `utime.ticks_ms()`.
    - Cambia al estado `"WaitTimeout"`.
    - Muestra el estado inicial del píxel.

- Después, en el estado `"WaitTimeout"`:

    - Compara el tiempo actual con `startTime` usando `utime.ticks_diff`.
    - Si ya pasó el intervalo definido, alterna el valor de `pixelState` (de 0 a 9 o al revés).
    - Actualiza el tiempo de inicio.
    - Cambia el valor del píxel en pantalla.

##### 4. Objetos creados

- `pixel1`: en la posición (0,0), parpadea cada 1000 ms (1 segundo).
- `pixel2`: en la posición (4,4), parpadea cada 500 ms (0.5 segundos).

#### Estados del programa
En el programa se pueden visualizar dos estados que se implementan en la clase `Pixel`
1. `"Init"`
     - Este es el estado inicial, estado en donde se configura el tiempo de referencia y se muestra el estado inicial del pixel.
2. `"WaitTimeout"`
     - Este es el estado en donde se espera a que pase el tiempo definido `(interval)` para alternar los estados del pixel.

#### Eventos del Programa
Hay que entender que el programa no tiene entradas de forma externa, pero responde a un evento de tiempo de forma interna, y es cuando el tiempo transcurrido desde `startTime` sobrepasa a `interval`

#### Acciones que realiza el programa
Las acciones que se ejecutan acorde a los estados y eventos son:
  1. Mostrar el estado inicial del pixel en `"Init"`
  2. Esperar el tiempo determinado en `"WaitTimeout"`
  3. Alternar el estado de los pixeles entre encendido (9) y apagado (0)
  4. Actualizar el LED establecido utilizando `display.set_pixel`


### Actividad 2

#### Código Comentado (parcial)

```py
from microbit import *
import utime # Contador en milisegundos

# Estados del semaforo
RED = "Red"
GREEN = "Green"
YELLOW = "Yellow"

# Posiciones de los LEDs del semáforo
red_pos = (2,0)
yellow_pos = (2,1)
green_pos = (2,2)

# Estado inicial del semáforo, va a empezar en rojo
current_state = RED
# Tiempo de referencia
start_time = utime.ticks_ms()

# Una función que va a apagar todos los LEDs
def clear_all():
    display.set_pixel(red_pos[0],red_pos[1], 0)
    display.set_pixel(yellow_pos[0], yellow_pos[1], 0)
    display.set_pixel(green_pos[0],green_pos[1], 0)

# Función que permite actualizar el led encendido cada cierto tiempo

def update_traffic_light():
    global current_state, start_time

    elapsed = utime.ticks_diff(utime.ticks_ms(), start_time)

    if current_state == RED:
        clear_all()
        display.set_pixel(red_pos[0],red_pos[1], 9)
        if elapsed >= 5000: # Si pasan más de 5 segundos, cambia de estado a "GREEN"
            current_state = GREEN
            start_time = utime.ticks_ms()
    elif current_state == GREEN:
        clear_all()
        display.set_pixel(green_pos[0],green_pos[1], 9)
        if elapsed >= 3000:# Si pasan más de 3 segundos, cambia de estado a "YELLOW"
            current_state = YELLOW
            start_time = utime.ticks_ms()
    elif current_state == YELLOW:
        clear_all()
        display.set_pixel(yellow_pos[0], yellow_pos[1], 9)
        if elapsed >= 1000: # Si pasa más de 1 segundo, cambia de estado a "RED"
            current_state = RED
            start_time = utime.ticks_ms()

while True:
    update_traffic_light()
    sleep(100)
            
```
#### Estados

- `RED`: Es el estado en donde el led que corresponde a la posición roja se enciende.
- `YELLOW`: Es el estado en donde el led que corresponde a la posición amarilla se enciende.
- `GREEN`: Es el estado en donde el led que corresponde a la posición verde se enciende.


#### Eventos

- Ocurren según paso del tiempo dado por (`elapsed`) dependiendo de los estados:
  - Rojo: 5 segundos.
  - Amarillo: 1 segundo.
  - Verde: 3 segundos.
 
#### Acciones
- Enciende el led correspondiente.
- Apaga los demás LEDs.
- Cambia al siguiente estado del semáforo.


### Actividad 3


```py
from microbit import *
import utime

STATE_INIT = 0
STATE_HAPPY = 1
STATE_SMILE = 2
STATE_SAD = 3

currentState = STATE_INIT
HAPPY_INTERVAL = 1500
SMILE_INTERVAL = 1000
SAD_INTERVAL = 2000


startTime = 0
interval = 0


def tarea1():
    global currentState
    global startTime
    global interval

    if currentState == STATE_INIT:
        display.show(Image.HAPPY)
        startTime = utime.ticks_ms()
        interval = HAPPY_INTERVAL
        currentState = STATE_HAPPY

    elif currentState == STATE_HAPPY:
        if button_a.was_pressed():
            display.show(Image.SAD)
            startTime = utime.ticks_ms()
            interval = SAD_INTERVAL
            currentState = STATE_SAD

        if utime.ticks_diff(utime.ticks_ms(), startTime) > interval:
            display.show(Image.SMILE)
            startTime = utime.ticks_ms()
            interval = SMILE_INTERVAL
            currentState = STATE_SMILE
       
    elif currentState == STATE_SMILE:
        if button_a.was_pressed():
            display.show(Image.HAPPY)
            startTime = utime.ticks_ms()
            interval = HAPPY_INTERVAL
            currentState = STATE_HAPPY

        if utime.ticks_diff(utime.ticks_ms(), startTime) > interval:
            display.show(Image.SAD)
            startTime = utime.ticks_ms()
            interval = SAD_INTERVAL
            currentState = STATE_SAD

    elif currentState == STATE_SAD:
        if button_a.was_pressed():
            display.show(Image.SMILE)
            startTime = utime.ticks_ms()
            interval = SMILE_INTERVAL
            currentState = STATE_SMILE

        if utime.ticks_diff(utime.ticks_ms(), startTime) > interval:
            display.show(Image.HAPPY)
            startTime = utime.ticks_ms()
            interval = HAPPY_INTERVAL
            currentState = STATE_HAPPY

    else:
        display.show(Image.PACMAN)

while True:
    tarea1()
```
#### Preguntas actividad 3

1. Explica por qué decimos que este programa permite realizar de manera concurrente varias tareas.
    - El programa realizado permite la concurrencia puesto que atiende simultáneamente a dos tipos de eventos:
      - Eventos temporales (transiciones entre imágenes según un cronómetro).
   
      - Eventos por parte del usuario (cuando presiona el botón A).
      
    Aunque de por si el micro:bit solamente ejecuta el código de una manera secuencial (no permite la ejecución multitarea), el diseño que se realizó en la actividad es a modo de una máquina de estados y la consulta constante que hace el programa hace a los temporizadores y al botón permiten simular un comportamiento concurrente: el sistema sigue el ciclo de imágenes, pero si el usuario presiona el botón `a`, el sistema interrumpe ese ciclo y reacciona de forma inmediata, sin esperar a que termine el intervalo de tiempo.
El enfoque del código simula concurrencia cooperativa, donde en el código, las tareas comparten recursos para que las tareas se realizen de una forma eficaz sin detener el funcionamiento del código en ningún momento, por lo que está siempre listo para reaccionar a un evento externo sin bloquearse mientras espera el tiempo.

2. Identifica los estados, eventos y acciones en el programa.

- **Estados:** `STATE_INIT, STATE_HAPPY, STATE_SMILE, STATE_SAD`
- **Eventos:** 
    - `utime` excede el intervalo del estado actual
    - `button_a.was_pressed()`
- **Acciones:**
    - Mostrar imágenes (`Image.HAPPY`, `Image.SMILE`, `Image.SAD`)
    - Actualizar el estado actual y el start_time
    - Asignar el nuevo intervalo
      
3. Describe y aplica al menos 3 vectores de prueba para el programa. Para definir un vector de prueba debes llevar al sistema a un estado, generar los eventos y observar el estado siguiente y las acciones que ocurrirán. Por tanto, un vector de prueba tiene unas condiciones iniciales del sistema, unos resultados esperados y los resultados realmente obtenidos. Si el resultado obtenido es igual al esperado entonces el sistema pasó el vector de prueba, de lo contrario el sistema puede tener un error.

- **Vector de Prueba 1**
  - Condiciones iniciales: Estado = STATE_HAPPY, el botón A es presionado antes de que pasen 1500 ms.
  - Evento: button_a.was_pressed() == True
  - Resultado esperado:
      - Imagen: Image.SAD
      - Estado nuevo: STATE_SAD
      - Intervalo: 2000 ms
  - _Resultado obtenido:_ coincide con el comportamiento esperado
     
   
- **Vector de Prueba 2**
  - Condiciones iniciales: Estado = STATE_SMILE, tiempo transcurrido = 1000 ms, no se presiona el botón A.
  - Evento: utime.ticks_diff(utime.ticks_ms(), startTime) > interval
  - Resultado esperado:
      - Imagen: Image.SAD
      - Estado nuevo: STATE_SAD
      - Intervalo: 2000 ms
  - _Resultado obtenido:_ el sistema responde de manera satisfactoria y coincide con el comportamiento esperado
     
   
- **Vector de Prueba 3**
  - Condiciones iniciales: Estado = STATE_SAD, el botón A es presionado antes de los 2000 ms.
  - Evento: button_a.was_pressed() == True
  - Resultado esperado:
      - Imagen: Image.SMILE
      - Estado nuevo: STATE_SMILE
      - Intervalo: 1000 ms
  - _Resultado obtenido:_ el sistema responde como se espera




