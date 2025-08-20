# Unidad 3

## 🔎 Fase: Set + Seek

### Código rehecho para el semáforo

```py
# Imports go at the top
from microbit import *
import utime


class Semaforo:
    def __init__(self,columna,tR,tY,tG):
        self.col = columna
        self.tr= tR
        self.ty = tY
        self.tg = tG
        display.set_pixel(self.col,0,9)
        self.interval = self.tr
        self.start_time = utime.ticks_ms()
        self.state = "RED"
        

        
    def update(self):
        if self.state == "RED":
            if utime.ticks_diff(utime.ticks_ms(),self.start_time) >= self.interval:
                display.set_pixel(self.col,0,0)
                display.set_pixel(self.col,2,9)
                self.interval = self.ty
                self.start_time = utime.ticks_ms()
                self.state = "YELLOW"        
        if self.state == "YELLOW":
            if utime.ticks_diff(utime.ticks_ms(),self.start_time) >= self.interval:
                display.set_pixel(self.col,2,0)
                display.set_pixel(self.col,4,9)
                self.interval = self.tg
                self.start_time = utime.ticks_ms()
                self.state = "GREEN"    
        if self.state == "GREEN":
            if utime.ticks_diff(utime.ticks_ms(),self.start_time) >= self.interval:
                display.set_pixel(self.col,4,0)
                display.set_pixel(self.col,0,9)
                self.interval = self.tr
                self.start_time = utime.ticks_ms()
                self.state = "RED"  

semaforo1 = Semaforo(0,5000,2000,3000)
semaforo2 = Semaforo(2,3000,1000,2000)
semaforo3 = Semaforo(4,4000,3000,2000)
# Code in a 'while True:' loop repeats forever
while True:
    semaforo1.update()
    semaforo2.update()
    semaforo3.update()

```

### Código modificado para que reciba intrucciones por radio:

```py
# Imports go at the top
from microbit import *
import utime
import radio

display.clear()


class Event:
    def __init__(self):
        self.value = 0

    def set(self,_val):
        self.value = _val

    def clear(self):
        self.value = 0

    def read(self):
        return self.value

class SerialTask:
    def __init__(self):
        uart.init(baudrate=115200)

    def update(self):
        if uart.any():
            data = uart.read(1)
            if data:
                if data[0] == ord('A'):
                    event.set('A')                 
                elif data[0] == ord('B'):
                    event.set('B')                
                elif data[0] == ord('S'):
                    event.set('S')                  
                elif data[0] == ord('T'):
                    event.set('T')
                    


class ButtonTask:
    def __init__(self):
        pass

    def update(self):
        if button_a.was_pressed():
            event.set('A')
        elif button_b.was_pressed():
            event.set('B')
        elif accelerometer.was_gesture('shake'):
            event.set('S')
        elif pin_logo.is_touched():
            event.set('T')

class RadioTask:
    def __init__(self):
        radio.config(group=16)
        radio.on()
        
    def update(self):
        message = radio.receive()
        if message == 'A':
            event.set('A')
        elif message == 'B':
            event.set('B')
        elif message == 'S':
            event.set('S')
        elif message == 'T':
            event.set('T')

class BombTask:
    def __init__(self):
        self.PASSWORD = ['A','B','A']
        self.key = ['']*len(self.PASSWORD)
        self.keyindex = 0
        self.count = 20
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'
        display.clear()
        display.show(self.count,wait=False)

    def update(self):
        if self.state == 'CONFIG':
            if event.read() == 'A':
                event.clear()
                self.count = min(self.count+1,60)
                display.show(self.count,wait=False)

            if event.read() == 'B':
                event.clear()
                self.count = max(10,self.count-1)
                display.show(self.count, wait=False)

            if event.read() == 'S':
                event.clear()
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count = self.count - 1
                display.show(self.count,wait=False)
                if self.count == 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            if event.read() == 'A':
                event.clear()
                self.key[self.keyindex] = 'A'
                self.keyindex = self.keyindex + 1

            if event.read() == 'B':
                event.clear()
                self.key[self.keyindex] = 'B'
                self.keyindex = self.keyindex + 1

            if self.keyindex == len(self.key):

                passIsOK = True
                for i in range(len(self.key)):
                    if self.key[i] != self.PASSWORD[i]:
                        passIsOK = False
                        break;
                if passIsOK == True:
                    self.count = 20
                    display.show(self.count,wait=False)
                    self.keyindex = 0
                    self.state = 'CONFIG'
                else:
                    self.keyindex = 0

        elif self.state == 'EXPLODED':
            if event.read() == 'T':
                event.clear()
                self.count = 20
                display.show(self.count,wait=False)
                self.startTime = utime.ticks_ms()
                self.state = 'CONFIG'

bombTask = BombTask()
serialTask = SerialTask()
buttonTask = ButtonTask()
event = Event()
radioRecieve = RadioTask()

while True:
    radioRecieve.update()
    serialTask.update()
    buttonTask.update()
    bombTask.update()
```
# Actividad 5
![Actividad5](https://github.com/user-attachments/assets/45a3c12e-3f43-40d0-9ff7-367a5a95a78c)

## Tabla de vectores de prueba

| Estado inicial                  | Evento disparador     | Acciones                                                    | Estado final                          |
|---------------------------------|-----------------------|-------------------------------------------------------------|---------------------------------------|
| CONFIG (count=20)               | Evento A              | Incrementa count (+1, máx. 60), muestra en display          | CONFIG (count=21)                      |
| CONFIG (count=20)               | Evento B              | Decrementa count (-1, mín. 10), muestra en display          | CONFIG (count=19)                      |
| CONFIG                          | Evento S              | startTime=ahora, cambia a ARMED                             | ARMED (count inicia en valor configurado) |
| ARMED (count=5)                 | Tick 1s (tiempo)      | Decrementa count y muestra en el display el numero 4        | ARMED (count=4)                        |
| ARMED (count=1)                 | Tick 1s (tiempo)      | Decrementa count a 0, muestra SKULL                         | EXPLODED                               |
| ARMED (esperando clave)         | Evento A              | Guarda 'A' en key, avanza keyindex                          | ARMED (keyindex+1)                     |
| ARMED (esperando clave)         | Evento B              | Guarda 'B' en key, avanza keyindex                          | ARMED (keyindex+1)                     |
| ARMED (keyindex == len(PASSWORD)) | Condición interna     | Compara clave con PASSWORD y es correcta                    | CONFIG (count=20, keyindex=0)          |
| ARMED (keyindex == len(PASSWORD)) | Condición interna     | Compara clave con PASSWORD y es incorrecta                  | ARMED (keyindex=0)                     |
| EXPLODED                        | Evento T              | Reinicia count=20, startTime=utime.ticks_ms(), muestra count  | CONFIG                                 |









