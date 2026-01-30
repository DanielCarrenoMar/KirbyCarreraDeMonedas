## Formato general

- Los paquetes se envían como arreglos de objetos (`Object[]`).
- El primer elemento siempre es un valor de `Packet.Types` que indica el tipo de paquete.
- El resto de elementos siguen un orden y tipo fijo según el método constructor en `Packet`.

## Tipos y constructores

- `CONNECTPLAYER` — Packet.connectPlayer(String name)
  - Formato: {Types.CONNECTPLAYER, String name}
  - Enviado por cliente al conectarse.

- `DISCONNECTPLAYER` — Packet.disconnectPlayer(int id)
  - Formato: {Types.DISCONNECTPLAYER, int id}
  - Indica que un jugador se desconectó.

- `NEWPLAYER` — Packet.newPlayer(int id, String name)
  - Formato: {Types.NEWPLAYER, int id, String name}
  - Notifica la presencia de un nuevo jugador.

- `GAMESTART` — Packet.gameStart()
  - Formato: {Types.GAMESTART}
  - Indica el inicio de la partida multijugador.

- `ACTENTITYPOSITION` — Packet.actEntityPosition(int id, float x, float y, float fx, float fy)
  - Formato: {Types.ACTENTITYPOSITION, int id, float x, float y, float fx, float fy}
  - `id == -1` se usa para indicar la posición del jugador local que debe mapearse al id del cliente remitente.

- `NEWENTITY` — Packet.newEntity(int id, Entity.Type type, float x, float y, float fx, float fy, boolean flipX)
  - Formato: {Types.NEWENTITY, int id, Entity.Type type, float x, float y, float fx, float fy, boolean flipX}

- `REMOVEENTITY` — Packet.removeEntity(int id)
  - Formato: {Types.REMOVEENTITY, int id}

- `ACTENEMY` — Packet.actEnemy(int id, Enemy.StateType state, float cronno, boolean flipX)
  - Formato: {Types.ACTENEMY, int id, Enemy.StateType state, float cronno, boolean flipX}

- `ACTDAMAGEENEMY` — Packet.actDamageEnemy(int id, int damage, float forceX, float forceY, float knockback)
  - Formato: {Types.ACTDAMAGEENEMY, int id, int damage, float forceX, float forceY, float knockback}

- `ACTOTHERPLAYER` — Packet.actOtherPlayer(int id, Player.AnimationType animationType, boolean flipX, PlayerCommon.StateType stateType, PowerUp.Type powerType)
  - Formato: {Types.ACTOTHERPLAYER, int id, Player.AnimationType animationType, boolean flipX, PlayerCommon.StateType stateType, PowerUp.Type powerType}

- `ACTBREAKBLOCK` — Packet.actBlock(int id, Block.StateType stateType)
  - Formato: {Types.ACTBREAKBLOCK, int id, Block.StateType stateType}

- `ACTSCORE` — Packet.actScore(int id, int score)
  - Formato: {Types.ACTSCORE, int id, int score}

- `ACTENTITYCOLOR` — Packet.actEntityColor(int id, float r, float g, float b, float a)
  - Formato: {Types.ACTENTITYCOLOR, int id, float r, float g, float b, float a}

- `MESSAGE` — Packet.message(String name, String message)
  - Formato: {Types.MESSAGE, String name, String message}


