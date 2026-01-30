## Visión general
Proyecto dividido por capas: arranque, red, pantallas/UI, lógica de juego, mundo/entidades, utilidades y gestores. Cada paquete contiene responsabilidades claras para facilitar mantenimiento y extensión.

## Librerias
- [LibGDX](https://libgdx.com/) — framework principal para desarrollo de juegos en Java.
- [Box2D](https://box2d.org/) — motor de física 2

## Puntos de entrada
- `main/Main.java` — inicialización de la aplicación.
- `net/Server.java` y `net/Client.java` — servidores y clientes para modo multijugador.
- Pantallas principales: `screens/*` (p. ej. `GameScreen`, `MinigameScreen`, `MenuScreen`) — controlan la navegación y UI.

## Paquetes y responsabilidades principales
- `main`
  - Inicialización de la aplicación y configuración general.
- `net`
    - Manejo de red, paquetes y listeners (`Client`, `Server`, `Packet`, `PacketListener`, `ClientListener`, `PlayerInfo`).
- `screens` y `uiScreens`
    - Sistema de pantallas / UI. `BaseScreen` provee comportamiento común.
- `game`, `minigames`
    - Lógica de juego principal y modos/minijuegos. `GameScreen` coordina capas de juego.
- `world`
    - Representación del mundo físico y entidades (`ActorBox2d`, `Entity`, `EntityFactory`, `player`, `enemies`, `items`, `projectiles`).
- `statics`
    - Objetos estáticos del nivel (suelo, plataformas, lava, spikes).
- `utils`
    - Utilidades comunes: Box2D helpers, fuentes, timers, `ThreadSecureWorld` (sincronización del mundo físico).
- `managers`, `indicators`, `sound`, `particles`
    - Gestores y sistemas transversales para cámaras, spawn, audio, partículas e indicadores.
- `constants`
    - Constantes de configuración y filtros de colisión.
- `stateMachine`
    - Máquina de estados para comportamientos reutilizables (clases `State`, `StateMachine`).

## Flujo de ejecución
1. `Main` arranca la aplicación y carga la pantalla inicial (`IntroScreen` / `MenuScreen`).
2. Navegación entre `screens` configura y muestra `GameScreen` o `MinigameScreen`.
3. `GameScreen` coordina `gameLayers` y `World` — delega creación de entidades a `EntityFactory`.
4. Física y actualización del mundo se hacen sobre Box2D; acceso seguro gestionado por `ThreadSecureWorld`.
5. En multijugador, `Client`/`Server` intercambian `Packet` para sincronizar estado y jugadores.
