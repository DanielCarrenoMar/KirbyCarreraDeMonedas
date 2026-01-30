## Visión general

El proyecto está dividido en capas y módulos con responsabilidades claras: arranque, red, pantallas/UI, lógica de juego, mundo/entidades, utilidades, gestores y recursos (assets). Esto facilita mantenimiento y extensión.

## Librerías y dependencias principales

- [LibGDX](https://libgdx.com/) — framework principal para el render, input y abstracción multiplataforma.
- [Box2D](https://box2d.org/) — motor de física (integrado con LibGDX).
- [Gradle](https://gradle.org/) — sistema de construcción.

## Puntos de entrada

- `src.main.Main` — clase principal que inicializa el `AssetManager`, carga sonidos, registra pantallas y arranca servidor/cliente.
- `src.net.Server` / `src.net.Client` — gestión de red y sincronización multijugador.
- Pantallas principales: `src.screens.GameScreen`, `src.screens.minigames.*`, `src.screens.uiScreens.*` — controlan la navegación y la interacción con el jugador.

## Paquetes y responsabilidades principales

- `src.main` — inicialización de la aplicación y gestión global (assets, sonidos, pantalla activa).
- `src.net` — capa de red: `Client`, `Server`, `ClientListener`, `PlayerInfo`, y `packets.Packet`.
- `src.screens` y `src.screens.uiScreens` — pantallas y componentes UI (`BaseScreen`, `GameScreen`, `MenuScreen`, `LobbyScreen`, etc.).
- `src.game` y `src.minigames` — lógica principal del juego y minijuegos.
- `src.world` — representación del mundo, entidades y objetos físicos (`ActorBox2d`, entidades, bloques, partículas).
- `src.statics` — objetos estáticos del nivel (plataformas, lava, pinchos, etc.).
- `src.utils` — utilidades y helpers (`Box2dUtils`, `ThreadSecureWorld`, `FontCreator`, timers).
- `src.managers`, `src.sound`, `src.indicators`, `src.particles` — sistemas transversales para audio, gestión de cámaras, particulas e indicadores.
- `src.constants` — constantes compartidas y filtros de colisión.
- `src.stateMachine` — implementación de máquinas de estado para entidades.

## Flujo de ejecución general

1. `Main.create()` inicializa `AssetManager`, sonidos y pantallas, y muestra la pantalla inicial (`IntroScreen`).
2. La navegación entre pantallas se realiza mediante `Main.changeScreen(...)` y cada `Screen` maneja su ciclo de render y actualización.
3. `GameScreen` coordina la actualización del `World`, la creación de entidades (vía `EntityFactory`) y la interacción con sistemas de físicas (Box2D) y UI.
4. La sincronización multijugador se realiza con `Client`/`Server` y los paquetes definidos en `src.net.packets.Packet`.
5. Al finalizar, `Main.dispose()` cierra clientes/servidores, libera assets y sonidos.

## Clases clave para entender el sistema

- `src.main.Main` — arranque y administración de pantallas; punto central de configuración.
- `src.screens.GameScreen`, `src.screens.BaseScreen` — ciclo principal de juego y gestión de capas.
- `src.world.ActorBox2d`, `src.world.entities.Entity`, `EntityFactory` — creación y representación de entidades.
- `src.utils.ThreadSecureWorld`, `src.utils.Box2dUtils` — acceso seguro y utilidades para la física.
- `src.net.Client`, `src.net.Server`, `src.net.ClientListener`, `src.net.packets.Packet` — comunicación en red y protocolos.
