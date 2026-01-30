## Arquitectura de red
El proyecto es una estructura de cliente-servidor utilizada.
Sockets TCP. Implementando un enfoque de cliente primero, donde cualquier dato de la
Partida del cliente se toma como verdad sin validarlo en el servidor.

## ¿Qué son sockets?
Los sockets son un mecanismo de comunicación entre dos computadoras.
A través de una red. Permiten el envío y recepción de datos.
Entre un cliente y un servidor.

### Tipos de protocolos
**TCP (Transmission Control Protocol)**. es un protocolo orientado a la conexión.
Lo que significa que establece una conexión confiable entre el cliente y el servidor.
Antes de enviar datos. Esto garantiza que los datos lleguen en el orden correcto.
Y sin pérdidas.
**UDP (User Datagram Protocol)**. es un protocolo sin conexión, lo que significa que no establece
Una conexión antes de enviar datos. Esto puede resultar en pérdidas de datos o en
Datos que llegan fuera de orden, pero es más rápido y eficiente para aplicaciones.
Que requieren baja latencia, como juegos en tiempo real.

### ¿Por qué TCP?
En este proyecto se utiliza TCP debido a que son más sencillos de trabajar.
Que no se debe manejar la pérdida de paquetes o el orden de los mismos. Además, al no estar
Orientado a mantener una gran cantidad de conexiones simultáneas, la velocidad adicional de
UDP no es necesaria.

## Arquitectura cliente-servidor
La arquitectura cliente-servidor es un modelo de diseño de software.
en el que las tareas se reparten entre los proveedores de recursos o
servicios, llamados servidores, y los demandantes, llamados clientes.
Un cliente realiza peticiones a otro programa, el servidor, quien le da respuesta.

### Tipos de arquitecturas básicas
- **Cliente-servidor**. En esta arquitectura, un servidor centralizado maneja las solicitudes de múltiples clientes. El servidor es responsable de procesar las solicitudes y enviar respuestas a los clientes.
- **Peer-to-Peer (P2P)**. En esta arquitectura, cada nodo en la red actúa como cliente y servidor. No hay un servidor centralizado, y los nodos se comunican directamente entre sí para compartir recursos y datos.

### ¿Por qué Cliente-Servidor?
Se eligió la arquitectura cliente-servidor debido a su escalabilidad, permitiendo N cantidad de clientes.
Su centralización, facilitando el manejo de datos y lógica del juego desde un solo punto.

## Enfoque Cliente Primero
El enfoque de cliente primero significa que el servidor acepta los datos enviados por los clientes.
Sin validarlos. Por ejemplo, si un cliente envía la posición de su personaje corriendo a velocidades supersónicas,
El servidor aceptará esa posición sin cuestionarla.

### Tipos de enfoques básicos
- **Cliente primero**. En este enfoque, el servidor acepta los datos enviados por los clientes.
- **Servidor Primero**. En este enfoque, el servidor valida y controla todos los datos del juego, asegurando que los clientes no puedan enviar datos incorrectos o maliciosos.

### ¿Por qué Cliente Primero?
Se eligió el enfoque de cliente primero debido a su simplicidad y facilidad de implementación. Además, es
El estándar para juegos cooperativos o locales donde no perjudican tanto las trampas.
