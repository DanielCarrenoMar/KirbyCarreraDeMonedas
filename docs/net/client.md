## Responsabilidades

- Establecer una conexión TCP con el servidor.
- Enviar paquetes (acciones del jugador, creación de entidades, mensajes) mediante una cola de envío segura.
- Recibir y procesar paquetes desde el servidor y actualizar el estado local (otros jugadores, entidades, puntuaciones, mensajes).
- Mantener información básica sobre jugadores conectados (`PlayerInfo`).

## Puntos de entrada clave

- `src.net.Client` — clase principal que ejecuta el ciclo de red en un hilo (`Runnable`).
  - Constructor: `Client(GameScreen game, String ip, int port, String name)`.
  - `run()` arranca la conexión, inicia los hilos de envío y lectura y procesa paquetes entrantes.
  - `send(Object[] data)` añade paquetes a la cola para envío asíncrono.
  - `close()` cierra la conexión y los streams.
  - `addListener(PacketListener listener)` / `notifyListenersPacket(Packet.Types type)` eventos para integrar la lógica del juego con la red.

## Flujo de conexión y comunicación

1. El cliente crea un socket TCP hacia el servidor (`Gdx.net.newClientSocket`) y abre `ObjectOutputStream` y `ObjectInputStream`.
2. Envía un paquete de conexión (`Packet.connectPlayer(name)`) y su color de jugador.
3. Un hilo dedicado toma objetos de `packetQueue` y los escribe por `out` hacia el servidor.
4. En el hilo principal de `Client.run()` se leen objetos desde `in` y se procesan según `Packet.Types`.
5. Para cada paquete recibido el cliente actualiza el `GameScreen` (añadir/eliminar entidades, actualizar posiciones, aplicar animaciones, mostrar mensajes).

## Paquetes relevantes que procesa el cliente

- `NEWPLAYER`, `DISCONNECTPLAYER` — gestión de jugadores conectados.
- `GAMESTART` — indica el inicio de la partida multijugador.
- `NEWENTITY`, `REMOVEENTITY` — creación y eliminación de entidades sincronizadas.
- `ACTENTITYPOSITION` — posición y estado de una entidad; puede aplicarse a otros jugadores o entidades.
- `ACTENEMY`, `ACTDAMAGEENEMY` — actualizaciones del estado de enemigos.
- `ACTOTHERPLAYER` — animaciones y estado de otros jugadores.
- `ACTBREAKBLOCK`, `ACTSCORE`, `ACTENTITYCOLOR`, `MESSAGE` — acciones diversas (bloques, puntuaciones, colores, chat).

## Detalles de implementación y consideraciones

- La cola `packetQueue` y el `sendThread` evitan bloqueos en el hilo principal de lectura.
- `playersConnected` almacena `PlayerInfo` por `id` y se actualiza al recibir `NEWPLAYER` o `ACTENTITYCOLOR`.
- `gameStart` se activa cuando se recibe `GAMESTART`; antes de esto algunos paquetes se ignoran (por ejemplo, mensajes de chat).
- Manejo de errores: si el socket se cierra o ocurre una excepción se llama a `close()` y se notifica a los listeners.

## Integración con el juego

- `GameScreen` registra el cliente (p. ej. en la pantalla de juego) y responde a los eventos invocados por los paquetes.
- El cliente usa métodos del `GameScreen` para crear `OtherPlayer` y actualizar entidades sin re-generar paquetes de red (métodos `addEntityNoPacket`, `removeEntityNoPacket`, etc.).
