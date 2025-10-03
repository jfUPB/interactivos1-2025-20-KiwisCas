
# Evidencias de la unidad 6


## Actividad 1

¿Qué ocurrió en la terminal cuando ejecutaste npm install? ¿Cuál crees que es su propósito?

Lo que ocurre es que se muestra la siguiente linea de código

```bash
up to date, audited 121 packages in 753ms

17 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```
Esto significa que en ese momento se estaben revisando e instalando las dependencias del sistema que el proyecto necesita para funcionar. Si no se ejecutase este paso, el programa no podría funcionar ya que este carecería de las librarías necesarias para su correcto funcionamiento.

Luego de esto, se procede a ejecutar el comando de `npm start`. Pasados unos segundos, aparecen las siguientes lineas en la consola de gitBash

```bash
$ npm start

> nodejs-test-1@1.0.0 start
> node server.js

Server is listening on http://localhost:3000
A user connected - ID: PSArXx1RRBboa3ehAAAC
A user connected - ID: f-x55X7NjL9LTHwvAAAD
```
Aquí Node.js actúa como intermediario: levanta un servidor que escucha en el puerto 3000 y, cuando se abre una página en el navegador, este se conecta y recibe los archivos `page1.js`, `page2.js` y sus respectivos `.html`. Estos definen la lógica y la estructura visual de la aplicación.  

En **page1**, al inicio se observa un lienzo esperando la conexión. Si solo está abierta esta página, el sistema queda “en espera” hasta que aparezca otra ventana (page2).

[Figura 1. Estado inicial de la página page1 esperando conexión.]<img width="1919" height="1024" alt="image" src="https://github.com/user-attachments/assets/e25b6bfd-133e-47aa-be77-f7ea369b26db" />
Una vez que se abre la segunda ventana, ambas empiezan a interactuar: los movimientos y cambios en una se reflejan en la otra, evidenciando la comunicación en tiempo real.

[Figura 2. Interacción entre page1 y page2 en tiempo real.]<img width="1919" height="1026" alt="image" src="https://github.com/user-attachments/assets/6a6552e1-5995-4f29-8df8-9c74cab07e1d" />

---

## Actividad 2

### Sobre el internet

Siguiendo la analogía, Internet puede entenderse como una enorme red de carreteras. Para entrar a esa red necesitamos una “rampa de acceso”: Wi-Fi, datos móviles o cable de red.  

Cuando la “rampa” se corta (por ejemplo, el router falla), el auto se queda fuera de circulación. Si ya se tenía acceso a una página, a veces la información almacenada en caché se mantiene por un rato, pero sin conexión nueva no se puede avanzar más.  

Profundizando en la metáfora: las carreteras permiten llegar a diferentes destinos, pero siempre es el usuario quien debe escoger la ruta (qué sitio visitar, qué acción realizar). El navegador es el vehículo, pero el conductor es la persona que está accediendo al interno.  

---

### Cliente y Servidor en la vida diaria  

Algunos ejemplos claros:  

- **Restaurante**: cliente = comensal, servidor = mesero/cocina. El cliente pide un plato, el servidor lo prepara y entrega.  
- **Biblioteca**: cliente = lector, servidor = bibliotecario. El cliente solicita un libro, el servidor lo localiza y entrega.  
- **Aplicaciones digitales**: en WhatsApp, cliente = aplicación en tu teléfono, servidor = infraestructura de Meta que transmite los mensajes.  

---

### Sobre URLs  

Como fan de los videojuegos, uso con frecuencia **Steam**. Una URL típica es:  

```
https://store.steampowered.com/?l=spanish
```

- Protocolo: `https://` (versión segura de HTTP)  
- Dominio: `store.steampowered.com`  
- Ruta: `/?l=spanish` (parámetro que ajusta el idioma al español)  

Si solo escribo `steampowered.com`, el servidor me redirige a la página principal (la tienda). Esa sería la “página por defecto”.  

---

### Protocolos seriales vs HTTP  

Similitudes:  
- Ambos son **protocolos de comunicación**.  
- Definen reglas para que emisor y receptor entiendan los mensajes.  
- Usan delimitadores para identificar inicios y finales de mensajes.  

Diferencias:  
- El protocolo serial es más simple: envía bytes directamente, suele usarse entre dispositivos físicos.  
- HTTP es más complejo: además de datos, maneja cabeceras, estados, tipos de contenido y rutas.  
- HTTP es “pregunta-respuesta”, mientras que el serial puede ser más directo (stream continuo).  

Razón de la complejidad: la web no solo transmite texto, también imágenes, videos, formularios, archivos. Se necesita un estándar más robusto para garantizar compatibilidad entre millones de clientes y servidores distintos.  

---

### HTML, CSS y JavaScript  

Ejemplo: un formulario de login en una página.  

- **HTML**: define los campos de usuario y contraseña, y el botón de “Ingresar”.  
- **CSS**: da estilo al botón (color, tipografía, tamaño).  
- **JavaScript**: valida que los campos no estén vacíos y muestra el mensaje “contraseña incorrecta” sin recargar la página.  

---

### Modelo imperativo (p5.js) vs basado en eventos (JS en la web)  

- En p5.js, la función `draw()` repite continuamente el código, incluso si nada cambia. Es útil para animaciones en tiempo real.  
- En una web, este modelo sería ineficiente. En lugar de redibujar todo 60 veces por segundo, se usan **eventos**: solo se ejecuta código cuando algo ocurre (clic, mensaje, resize).  

Ventaja: se ahorran recursos y la aplicación responde justo en el momento necesario.  

---

### ¿Por qué usar JavaScript en cliente y servidor?  

Porque unifica el lenguaje: el programador puede usar la misma sintaxis, librerías y mentalidad tanto en el navegador como en el servidor. Esto reduce la curva de aprendizaje y permite compartir código (por ejemplo, validaciones de formularios).  

---

### HTTP vs WebSockets / Socket.IO  

- **HTTP**: comunicación por turnos. El cliente pide y el servidor responde. Es como mandar cartas.  
- **WebSockets/Socket.IO**: se establece un canal permanente, bidireccional. Es como una llamada telefónica en la que ambos pueden hablar sin esperar turno.  

Ejemplos de uso en la vida real:  
- Chats en vivo (WhatsApp Web, Messenger).  
- Videojuegos en línea.  
- Herramientas colaborativas (Google Docs, Figma).  
- Seguimiento en tiempo real (cursos online con puntero compartido).  




