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








