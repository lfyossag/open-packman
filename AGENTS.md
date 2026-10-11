# AGENTS.md

Pac-Man en vanilla JS/HTML/CSS, sin build ni dependencias. El proyecto existe para practicar Spec Driven Development (ver README).

## Comandos y verificación

- No hay `package.json`, build, lint, typecheck ni tests. No inventar comandos: la verificación es manual en el navegador.
- Ejecutar: abrir `src/index.html` directo en el navegador (no hay fetch ni módulos ES; no hace falta servidor).

## Flujo de trabajo: specs

Las features nuevas pasan por los skills `/spec` y `/spec-impl` (definidos en `.agents/skills/`, versionados en `skills-lock.json`), no por implementación directa:

1. `/spec <descripción>` → crea `specs/NN-slug.md` en estado `Draft` (la carpeta `specs/` aún no existe; la primera spec será `01-`).
2. El humano cambia el estado a `Approved`/`Aprobado` a mano; el agente nunca lo hace solo.
3. `/spec-impl NN-slug` → valida el estado, crea la rama `spec-NN-slug` e implementa paso a paso con pausas. Nunca commitea automáticamente.

Config de ramas: `specs/.spec-config.yml` (`AutoCreateBranch`, default `true`).

## Arquitectura

- 4 scripts cargados por `src/index.html` en orden fijo, sin módulos ES: comparten globals y el orden importa (`maze.js` → `game.js` → `render.js` → `main.js`). Un JS nuevo exige añadir su `<script>` en `index.html`.
- `maze.js`: laberinto 28×31 escrito como strings y parseado a números. Define los globals `MAZE`, `TUNNEL_ROW`, `PACMAN_START`, `GHOST_STARTS`.
- `game.js`: estado y reglas (`createGame`, `update`). Estados del juego: `start` | `playing` | `won` | `lost`.
- `render.js`: dibujo en canvas. Lee `game.grid`, nunca `MAZE` (para reflejar los dots comidos).
- `main.js`: bucle `requestAnimationFrame`, teclado (flechas) y overlay.

Semántica de celdas del grid: `0` vacío transitable · `1` pared · `2` dot · `3` puerta del corral (bloquea a Pac-Man; los fantasmas la cruzan). `game.grid` es una copia mutable de `MAZE` (comer dots y reiniciar sin destruir el original).

## Idioma

- Comentarios, README y UI en español. Los specs nuevos siguen el idioma de los existentes. Responder en el idioma del prompt del usuario.
