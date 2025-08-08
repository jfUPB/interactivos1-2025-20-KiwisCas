# Unidad 2


## 🛠 Fase: Apply

### ACTIVIDAD 4 
#### Diseño de la lógica de una bomba temporizada

##### Diagrama de máquina de estados
<img width="679" height="751" alt="Diagrama drawio (1)" src="https://github.com/user-attachments/assets/d5d73a06-34b2-4018-bc05-0caadf0eab7c" />


### ACTIVIDAD 5
#### Implementando la Bomba Temporizada

##### Código de la bomba temporizada

```py
# Imports go at the top
from microbit import *
import utime
import music

STATE_INIT = 0
STATE_CONFIG = 1
STATE_ARMED = 2

current_state = STATE_INIT
BOMB_INTERVAL = 20000
armed_start_time = 0
remaining_time = BOMB_INTERVAL

def show_time(ms):
    seconds = ms // 1000
    for digit in str(seconds):
        display.show(digit)
        sleep(400)


# Code in a 'while True:' loop repeats forever
while True:

    if current_state == STATE_INIT:
        display.show(Image.SAD)
        sleep(1000)
        current_state= STATE_CONFIG
        display.clear()

    elif current_state == STATE_CONFIG:  
        show_time(BOMB_INTERVAL)
        
        if button_a.was_pressed() and BOMB_INTERVAL < 60000:
            BOMB_INTERVAL += 1000
            display.show(Image.ARROW_N)
            sleep(300)
            
        elif button_b.was_pressed() and BOMB_INTERVAL > 10000:
            BOMB_INTERVAL -= 1000
            display.show(Image.ARROW_S)
            sleep(300)
            
        elif accelerometer.was_gesture('shake'):
            current_state = STATE_ARMED
            armed_start_time = utime.ticks_ms()
            remaining_time = BOMB_INTERVAL
            display.clear()
            
        sleep(100)
        
    elif current_state == STATE_ARMED:
        now = utime.ticks_ms()
        elapsed = utime.ticks_diff(now,armed_start_time)
        remaining_time = BOMB_INTERVAL - elapsed

        if remaining_time > 0:
            show_time(remaining_time)
        else:
            display.clear()
            display.show(Image.SKULL)
            music.play(music.WAWAWAWAA)
            sleep(2000)
            display.clear()
            current_state = STATE_CONFIG
            BOMB_INTERVAL = 20000
            continue

        if pin_logo.is_touched():
            current_state = STATE_CONFIG
            BOMB_INTERVAL = 20000
            display.clear()

        sleep(100)

```

##### Vectores de Prueba

###### Vector de Prueba 1

**Condiciones iniciales:** Estado = `STATE_CONFIG`, `BOMB_INTERVAL = 20000 ms`. El botón A es presionado 5 veces antes de agitar el micro:bit.
**Evento:** `button_a.was_pressed() == True` (5 veces) seguido de `accelerometer.was_gesture('shake') == True`.

**Resultado esperado:**

  - Imagen: `Image.ARROW_N` en cada incremento.
  - Estado nuevo: `STATE_ARMED`.
  - Intervalo (BOMB_INTERVAL) = 25000 ms.
  - Comienza cuenta regresiva desde 25 s y, al llegar a 0, muestra Image.SKULL, reproduce `music.WAWAWAWAA`, vuelve a `STATE_CONFIG` con `BOMB_INTERVAL = 20000 ms`.

**Resultado obtenido:** El sistema incrementa el tiempo, muestra flechas hacia arriba, cuenta desde 25 s, detona y reinicia correctamente.

**Veredicto:** Coincide con el comportamiento esperado

###### Vector de Prueba 2

**Condiciones iniciales:** Estado = `STATE_CONFIG`, `BOMB_INTERVAL = 20000 ms`. El botón B es presionado 5 veces antes de agitar el micro:bit.
**Evento:** `button_b.was_pressed() == True` (5 veces) seguido de `accelerometer.was_gesture('shake') == True`, luego `pin_logo.is_touched() == True` antes de llegar a 0.

**Resultado esperado:**

  - Imagen: `Image.ARROW_S` en cada decremento.
  - Estado nuevo después de agitar: `STATE_ARMED`.
  - Intervalo `(BOMB_INTERVAL) = 15000 ms`.
  - Se desarma al tocar pin_logo, vuelve a `STATE_CONFIG` con `BOMB_INTERVAL = 20000 ms`, sin sonido ni imagen de explosión.

**Resultado obtenido:** El sistema reduce el tiempo a 15 s, pasa a `STATE_ARMED`, se desarma al tocar `pin_logo` y vuelve a configuración con el tiempo reiniciado.

**Veredicto:** Coincide con el comportamiento esperado

###### Vector de Prueba 3

**Condiciones iniciales:** Estado = `STATE_CONFIG`, `BOMB_INTERVAL` = 20000 ms.
**Evento:** Se presiona `button_a.was_pressed() == True` repetidamente hasta intentar superar 60000 ms y luego `button_b.was_pressed() == True` repetidamente hasta intentar bajar de 10000 ms, después `accelerometer.was_gesture('shake') == True`.

**Resultado esperado:**
  - Imagen: `Image.ARROW_N` al incrementar y `Image.ARROW_S` al decrementar.
  - Límite superior: 60000 ms.
  - Límite inferior: 10000 ms.
  - Estado nuevo después de agitar: `STATE_ARMED` con el último tiempo válido.
  - Al llegar a 0: `Image.SKULL`, sonido `music.WAWAWAWAA`, vuelve a `STATE_CONFIG` con `BOMB_INTERVAL = 20000 ms`.

**Resultado obtenido:** El sistema respeta los límites, arma la bomba y detona con el tiempo configurado, reiniciando correctamente.

**Veredicto:** Coincide con el comportamiento esperado 
