## Responsabilidades

- Aceptar conexiones TCP entrantes.
- Mantener y gestionar la lista de clientes conectados (`ClientListener`).
- Reenviar (broadcast) paquetes recibidos a los clientes pertinentes.
- Delegar la generación de IDs (a través de `Main`) para jugadores y entidades.
- Cerrar y limpiar conexiones de forma ordenada.

## Puntos de entrada clave

- `src.net.Server` — clase que ejecuta el listener del servidor en un hilo:
  - Constructor: `Server(GameScreen game, String ip, Integer port)`.
  - `run()` — bucle principal que acepta conexiones y crea `ClientListener`.
  - `sendAll(Object[] data, Integer id)` — reenvía paquetes a todos los clientes, opcionalmente omitiendo el `id` remitente.
  - `close()` — detiene el servidor y cierra todas las conexiones.

- `src.net.ClientListener` — represente una conexión cliente dentro del servidor:
  - Constructor: `ClientListener(Server server, Socket socket, Integer id)`.
  - `run()` — procesa paquetes entrantes y los reenvía según la lógica de juego.
  - `send(Object[] data)` — envía un paquete al cliente.
  - `close()` / `closeNoPacket()` — limpian recursos y notifican desconexión.

## Flujo de ejecución (alto nivel)

1. `Main.startServer(ip, port)` crea la instancia `Server` y la ejecuta en un hilo.
2. `Server.run()` abre un `ServerSocket` y espera conexiones (`accept`).
3. Por cada conexión entrante se crea un `ClientListener` con un ID único y se ejecuta en un pool de hilos.
4. Los `ClientListener` reciben paquetes serializados (`Object[]`) desde el cliente y, según su tipo, realizan reenvíos o acciones específicas (p. ej. notificar nuevos jugadores, propagar posiciones o eventos del juego).
5. El servidor utiliza `sendAll` para difundir eventos a todos los clientes conectados salvo el origen cuando procede.
6. Al cerrar el servidor, se invocan `closeNoPacket()` en los listeners y se liberan recursos.

## Paquetes y responsabilidades relacionadas

- `CONNECTPLAYER` → el servidor asigna o registra el jugador y notifica a los demás con `NEWPLAYER`.
- `GAMESTART` → difusión del inicio de la partida.
- `ACTENTITYPOSITION`, `NEWENTITY`, `REMOVEENTITY` → sincronización de entidades entre clientes.
- `ACTENEMY`, `ACTDAMAGEENEMY`, `ACTOTHERPLAYER`, `ACTBREAKBLOCK`, `ACTSCORE`, `ACTENTITYCOLOR` → eventos de juego reenviados a clientes.
- `MESSAGE` → chat entre jugadores, retransmitido por el servidor.

## Detalles de implementación

- Concurrencia: se usa un `ExecutorService` (pool) para manejar `ClientListener` y `CopyOnWriteArrayList` para la lista de usuarios.
- Seguridad/validación: la implementación actual aplica un modelo "cliente primero"; no se validan los datos recibidos. Para servidores públicos se recomienda validar posiciones, límites de velocidad y tasas de mensajes.
- Robustez: en caso de error de E/S cada `ClientListener` cierra su socket y el servidor elimina la referencia al usuario.
- IDs: `Server` solicita IDs a `Main` (`game.main.getIds()`), por lo que la generación centralizada evita colisiones.

### ClientListener
- Cada instancia maneja la comunicación con un cliente específico.
- Lee paquetes entrantes y los procesa según su tipo.
- Una vez procesados, reenvía los paquetes al servidor para su difusión a todos los demás clientes
conectados.
