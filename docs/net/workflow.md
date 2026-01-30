## Arranque de partida

1. El host inicia un servidor: `Main.startServer(ip, port)`.
2. Otros jugadores inician un cliente: `Main.startClient(ip, port)` que crea `Client` y ejecuta su hilo.
3. Cliente → Servidor: `CONNECTPLAYER` con el nombre del jugador.
4. Servidor asigna un ID al nuevo jugador y envía `NEWPLAYER` a los demás clientes.
5. Cliente recibe `NEWPLAYER` y crea una instancia de `OtherPlayer` en el `GameScreen`.
6. Cliente envía `ACTENTITYCOLOR` para actualizar el color de su avatar y `actEntityPosition` según movimiento.
7. Cuando el anfitrión inicia la partida envía `GAMESTART` y el servidor lo retransmite a todos.

## Comunicación durante la partida

- Posiciones y movimientos: `ACTENTITYPOSITION` (frecuencia alta; el cliente envía su propia posición con `id == -1` y el servidor retransmite con el id del cliente).
- Creación/Eliminación de entidades: `NEWENTITY` / `REMOVEENTITY` cuando un objeto del mundo aparece o desaparece.
- Eventos de enemigos y objetos: `ACTENEMY`, `ACTDAMAGEENEMY`, `ACTBREAKBLOCK`, `ACTSCORE`.
- Animaciones y estados de otros jugadores: `ACTOTHERPLAYER`.
- Chat: `MESSAGE`.

## Cierre y desconexión

- Cliente desconecta: envía `DISCONNECTPLAYER` o cierra el socket; el `ClientListener` o `Client` notifica la desconexión.
- Servidor notifica la desconexión con `DISCONNECTPLAYER` y opcionalmente envía un `MESSAGE` informando a los demás.
- Recursos: `Server.close()` y `Client.close()` liberan sockets y streams.
