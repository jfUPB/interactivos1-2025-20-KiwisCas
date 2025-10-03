
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


## Actividad 3

### Experimento 1 — Rutas y respuestas (app.get)
- Objetivo: Ver cómo Express asocia URL exactas con respuestas.

Prerrequisitos:
- Servidor detenido.

Pasos realizados:
1) Cambié en `server.js` la ruta de `/page1` a `/pagina_uno`.
2) Inicié el servidor.
3) Probé: http://localhost:3000/page1
4) Probé: http://localhost:3000/pagina_uno

Observaciones:
- /page1 → 404 (no debe funcionar).
  
<img width="2559" height="950" alt="image" src="https://github.com/user-attachments/assets/28299d19-df6d-43c6-994d-386b012de29b" />

    Efectivamente la página no funciona habiendo hecho los cambios para luego ingresar a la dirección inicial, es decir `http://localhost:3000/page1`
  
  
- /pagina_uno → devuelve el mismo contenido que antes (de page1.html).
  
<img width="2559" height="1002" alt="image" src="https://github.com/user-attachments/assets/7f35d76b-1228-4d79-b229-ca7868022703" />

  Ahora si, la página descarga el contenido de page1.html

Explicación
  Express hace matching exacto de la URL declarada en app.get(...). Al renombrar la ruta, cambias el “camino” que devuelve el archivo. La capa de estáticos (app.use(express.static(...))) no crea la ruta /page1; solo sirve archivos por ruta directa de archivo (por ejemplo, /page1.html si existiera con ese nombre exacto).

### Experimento 2 — Conexiones (socket.id) y desconexiones
- Objetivo: Observar los IDs de conexión y desconexión de Socket.IO.

Pasos realizados:
1) Inicié servidor.
2) Abrí http://localhost:3000/page1 → ID conectado: M6537eZbTS44EhoaAAAD
3) Abrí http://localhost:3000/page2 → ID conectado: JTnmvTO3e0AtxWHbAAAF
4) Cerré pestaña de page1 → ID desconectado: M6537eZbTS44EhoaAAAD (coincide con el de page1)
5) Cerré pestaña de page2 → ID desconectado: JTnmvTO3e0AtxWHbAAAF

Esto tiene su observación y es que cada conexión WebSocket tiene un socket.id único. Al cerrar la pestaña, el servidor recibe el evento disconnect y lo registra con el mismo ID.

### Experimento 3 — Eventos win1update / win2update y broadcast
- Objetivo: Ver qué evento llega con cada página y cómo difiere `socket.emit` vs `socket.broadcast.emit`.

Pasos realizados:
1) Servidor iniciado; abrí page1 y page2.
2) Moví la ventana de page1 → vi en servidor: `Received win1update from ID: 10xHQ5KqKO6aes71AAAD Data: { x: 1043, y: 56, width:
 1239, height: 916 }
Debug - Connected clients: 2, Page1: 1, Page2: 1, Synced: 2
All clients are fully synced `
3) Moví la ventana de page2 → vi: `Received win2update from ID: wMoua2zXNJM9ROfUAAAF Data: { x: 0, y: 161, width: 1262, height: 916 }
Debug - Connected clients: 2, Page1: 1, Page2: 1, Synced: 2
All clients are fully synced`
4) Cambié en `server.js` los `socket.broadcast.emit('getdata', ...)` a `socket.emit('getdata', ...)`.
5) Reinicié y repetí movimientos.

Explicación
- socket.emit envía el evento solo al cliente que originó el mensaje (el mismo socket).
  <img width="2501" height="885" alt="image" src="https://github.com/user-attachments/assets/61ca766e-b072-4f25-b697-40f3ea6632a9" />
  Acá podemos ver que page2 no se actualiza puesto que socket.emit envía el evento solo al mismo socket emisor; no llega a los demás clientes. Para sincronizar la otra pestaña necesitas socket.broadcast.emit, que envía a todos excepto al emisor. Restaura a broadcast.emit.
  
- socket.broadcast.emit envía a “todos menos al emisor”. Para sincronizar con la otra pestaña necesitas broadcast.
  <img width="2042" height="870" alt="image" src="https://github.com/user-attachments/assets/98ceca00-9fb2-4a50-b747-865769f1ebcc" />

### Experimento 4 — Puerto del servidor (listen)
- Objetivo: Ver el impacto del puerto en la URL.

Pasos realizados:
1) Detuve servidor.
2) Cambié `const port = 3000;` a `const port = 3001;`.
3) Inicié servidor → confirmé mensaje “listening on 3001”.
   
   <img width="566" height="108" alt="image" src="https://github.com/user-attachments/assets/9ef5af27-c292-4371-9811-bfb0ce8db389" />
   
5) Probé http://localhost:3000/page1 y falló.
   <img width="1232" height="842" alt="image" src="https://github.com/user-attachments/assets/3bd8824d-1872-4994-a798-6daf36416d9e" />
6) Probé http://localhost:3001/page1 y ahora si, la página responde como debe de ser.
   <img width="1228" height="913" alt="image" src="https://github.com/user-attachments/assets/58dce227-cbca-4c06-ac0d-b4b0635605c7" />

Cuando se cambia const port = 3000 a 3001 y se ejecuta el servidor, Node hace server.listen(3001), lo que significa que el proceso se “adhiere” únicamente al puerto TCP 3001 del host (por eso la consola muestra algo como “listening on 3001”). En la web, la URL incluye host y puerto; http://localhost:3000/page1 intenta conectarse al puerto 3000, pero ahí no hay ningún proceso escuchando, así que falla (p. ej., ERR_CONNECTION_REFUSED). En cambio, http://localhost:3001/page1 sí funciona porque coincide con el puerto donde el servidor está escuchando. El punto es que la variable port define el punto de enlace de la aplicación, y listen solo atiende exactamente ese puerto; si el puerto en la URL no coincide, la conexión no puede establecerse.


## Actividad 04 

### Experimiento 1

**Refresca la página page2.html. Cuando el servidor se cierra observa la consola del navegador. ¿Ves algún error relacionado con la conexión? ¿Qué indica?** 

<img width="1254" height="916" alt="image" src="https://github.com/user-attachments/assets/d29fab4e-95f6-40ca-88f0-f08303027505" />


Habiendo detenido el el servidor Node.js y actualizada la página, el cliente de Socket.IO ya no podrá contactar con el backend. En la imagen se pueden observar errores de red del intento de reconectar, como solicitudes GET al endpoint de polling de Socket.IO rechazadas con “net::ERR_CONNECTION_REFUSED” y fallos de establecimiento de WebSocket con el mismo motivo. Esto simplemente indica que el puerto 3000 ya no está atendiendo porque el proceso del servidor se detuvo. Además, por el código del servidor, aparecerá un mensaje del tipo “Disconnected from server” y el estado de sincronización pasará a no sincronizado. Es normal que el cliente de Socket.IO siga reintentando automáticamente la conexión con backoff mientras el servidor no esté disponible.


**Vuelve a iniciar el servidor y refresca la página. ¿Desaparecen los errores?** 

<img width="1262" height="892" alt="image" src="https://github.com/user-attachments/assets/d52295a0-b085-4064-b519-f0034aef17e8" />


Cuando se vuelve a iniciar el servidor y se recarga la página, esos errores desaparecen y se ve de nuevo el mensaje de conexión exitosa y los eventos normales, como el estado de sincronización que volverá a cambiar cuando haya otra ventana conectada. Es más, incluso sin recargar, Socket.IO suele reconectar automáticamente al detectar que el servidor volvió a estar disponible, aunque recargar la página limpia el estado del cliente más rápido.

### Experimento 2 

<img width="2495" height="949" alt="image" src="https://github.com/user-attachments/assets/2f021ab0-86c4-4935-b40a-7ceb007a91ee" />


**¿Qué pasó? ¿Por qué?** 

Al comentar la línea que emite “win2update” dentro del listener de “connect”, tras reiniciar el servidor y refrescar page1.html y page2.html no hubo sincronización inicial: page1 no recibió el estado actual de page2 y el indicador de sincronización permaneció en “NOT SYNCED”. Esa emisión en “connect” funciona como el arranque o bootstrap de estado: en cuanto el cliente se conecta, anuncia su estado actual (currentPageData) e identifica al emisor (socket.id) para que el servidor o el otro cliente se alineen desde el primer instante. Al comentarla, el cliente deja de publicar su estado al conectarse, así que nadie tiene datos con los que inicializar la sincronización y el sistema queda “sin estado”

### Experimento 3 

Acá básicamente lo que ocurre es que al mover la ventana de page2, en la consola de page1 se debería de ver el evento de actualización correspondiente con el payload que emite page2 (su currentPageData y el socket.id del emisor). Esto ocurre porque cada cliente, al detectar un cambio, emite su estado por Socket.IO y el servidor lo reenvía al resto de sockets conectados; el cliente receptor registra el log y actualiza su UI para reflejar el nuevo estado, lo mismo debe de ocurrir al revés.

Experimento 4 
<img width="803" height="625" alt="image" src="https://github.com/user-attachments/assets/c6e8c8a9-eda3-4385-9385-89dee9813a68" />

¿Qué puedes concluir y por qué? 

Con este mensaje se puede ver que al esta parte de la función checkWindowPosition si se esta utilizando y se activa con el movimeinto de la pestaña o el cambiar el tamaño de esta.


Experimento 5 
Cambia el background(220) para que dependa de la distancia entre las ventanas. Puedes calcular la magnitud del resultingVector usando let distancia = resultingVector.mag(); y luego usa map() para convertir esa distancia a un valor de gris o color. background(map(distancia, 0, 1000, 255, 0)); (ajusta el rango 0-1000 según sea necesario). 

