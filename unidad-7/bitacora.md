
# Evidencias de la unidad 7

¿Qué URL de Dev Tunnels obtuviste? ¿Por qué crees que necesitamos usar esta URL en lugar de http://localhost:3000 o la IP local de tu computador para que el celular se conecte?

obtuve la siguiente URL: https://f51lclcj-3000.use2.devtunnels.ms/ 
Cuando se trabaja con servidores locales, al usar `https://localhost:3000` esto significa que el servidor solamente va a funcionar dentro del mismo computador en donde se está corriendo el servidor debido a que cuando se usa local host este **apunta a la dirección de si mismo**, el loopback de la máquina, por lo que cuando el celular o cualquier otro dispositivo intenta acceder a esta misma dirección url, este va a buscar una dirección en su propio sistema, no en un sistema externo.


Describe brevemente qué hace npm install y npm start.

`npm install`

¿Qué mensajes observaste en la terminal del servidor al conectar el cliente de escritorio y el cliente móvil? ¿Eran diferentes los mensajes o identificadores?
Describe el comportamiento observado: ¿Funcionó la interacción? ¿Hubo algún retraso (latencia)?


