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


