---
tags:
  - sesion
  - sprint-12
  - mapas
  - biomas
  - tilesets
  - planificacion
fecha: 2026-05-20
sprint: sprint-12
estado: planificacion
dependencias:
  - sprint-11
---

# Sesión 2026-05-20 — Sprint 12: Mapas y Biomas

## Resumen

Sesión de planificación del Sprint 12. Se define el sistema de mapas por tiles para los 5 biomas del Vacío Kaótico. Este sprint es sustancial porque requiere:

1. **Nuevo código** — LibGDX actualmente usa gráficos procedurales (Pixmap). Añadir soporte para TiledMap es una incorporación no trivial al pipeline de render.
2. **34 assets de arte** — 20 tiles de suelo + 14 decoraciones, generadas con PixelLab MCP.
3. **Integración de datos** — 5 mapas TMX, uno por bioma, con transición entre ellos.

**Decisión: Sprint 12 independiente** (no pista paralela en S10/S11).

El trabajo de código (sistema TiledMap) es suficientemente sustancial como para necesitar su propio sprint. Añadirlo como pista en Sprint 10 o 11 crearía contención — esos sprints ya tienen carga completa con enemy redesign y weapon redesign respectivamente. Además, los biomas son un sistema con impacto en GameScreen, Camera y rendering que merece planificación y QA propios.

---

## Posición en el roadmap

```
Sprint 9  (completado) — Assets personajes + fix shooter
Sprint 10 (planificado) — Rediseño sistema enemigos
Sprint 11 (planificado) — Rediseño sistema armas
Sprint 12 (este) ——————— Mapas y biomas (nuevo código + assets)
```

`2026-08-10 → 2026-08-23` · 10 días estimados · depende de Sprint 11

> [!important] Dependencia de Sprint 11
> Sprint 12 depende de Sprint 11 porque el código de armas y enemigos refactorizado hace las GameScreen más estable antes de añadir el sistema de tiles. Especialmente el `SpawnManager` y `BossManager` — deben estar listos antes de que los biomas dicten la zona de spawn del jefe.

---

## Contexto técnico — Gráficos procedurales vs TiledMap

El juego usa actualmente **Pixmap** para dibujar el suelo: una grilla de celdas 32px dibujada en código, con grietas generadas proceduralmente. No hay carga de texturas de suelo.

Para implementar biomas visuales hay dos opciones:

| Opción | Ventaja | Desventaja |
|--------|---------|------------|
| **A — TiledMap + TMX** | Estándar LibGDX, Tiled editor, fácil de iterar el mapa | Requiere TmxMapLoader, OrthogonalTiledMapRenderer, nuevo render pass |
| **B — Pixmap bioma-aware** | Mínimo código nuevo, coherente con el sistema actual | Los tilesets PixelLab no se integran, suelo sigue siendo 100% procedural |

**Decisión: Opción A — TiledMap**

Los assets generados en PixelLab son PNGs. El sistema Pixmap no los puede usar sin trabajo adicional. TiledMap es el camino natural en LibGDX para tilesheets y permite al diseñador editar mapas sin tocar código. El coste técnico está justificado.

---

## Stories del Sprint 12

### Pista 1 — Código (TiledMap system)

#### S12-01 — BiomeMapManager: infraestructura TiledMap en LibGDX

**Estimación:** 2 días  
**Responsable:** gameplay-programmer / lead-programmer

**Descripción:**
Crear `BiomeMapManager.java` que cargue y renderice TiledMaps. Integrar con `GameScreen` sin romper el render Pixmap existente.

**Acceptance criteria:**
- [ ] `BiomeMapManager.load(BiomeType biome)` carga el TMX correspondiente desde `assets/maps/`
- [ ] `BiomeMapManager.render(SpriteBatch batch, Camera camera)` renderiza el mapa debajo de entidades
- [ ] El render de Pixmap (grid procedural) se desactiva cuando `BiomeMapManager` está activo
- [ ] La cámara sigue al jugador correctamente con el mapa TiledMap (OrthogonalTiledMapRenderer)
- [ ] Smoke test: el juego arranca, el mapa se ve, el jugador se mueve correctamente

**Notas técnicas:**
```java
// Dependencia Gradle — ya incluida en LibGDX 1.14.0
// No hay que añadir ningún artefacto adicional
TiledMap map = new TmxMapLoader().load("maps/abismo_central.tmx");
OrthogonalTiledMapRenderer renderer = new OrthogonalTiledMapRenderer(map);
// En render():
renderer.setView(camera);
renderer.render();
// Dispose en hide()/dispose():
map.dispose();
renderer.dispose();
```

**Archivos afectados:**
- `screens/GameScreen.java` — integrar `BiomeMapManager`, desactivar render Pixmap de suelo
- `utils/BiomeMapManager.java` — nuevo archivo
- `utils/Constants.java` — añadir enum `BiomeType { ABISMO, CRISTAL, PANTANO, FORJA, NUCLEO }`

---

#### S12-02 — Transición de bioma

**Estimación:** 1 día  
**Responsable:** gameplay-programmer

**Descripción:**
Lógica para cambiar de bioma en runtime. En v1 la transición es instantánea (sin fade) — el fade se puede añadir en polish si hay tiempo.

**Acceptance criteria:**
- [ ] `BiomeMapManager.transitionTo(BiomeType next)` descarga el mapa actual y carga el nuevo
- [ ] La transición no genera memory leak (dispose correcto del mapa anterior)
- [ ] `GameScreen` puede llamar la transición desde un trigger (por ahora: al superar oleada X)
- [ ] Log en consola cuando se produce la transición (debug)

**Notas técnicas:**
El trigger inicial puede ser tiempo de juego o número de oleada completada — algo ya rastreado en `GameScreen.timerDificultad`. Ejemplo: bioma cambia cada 5 minutos de partida.

---

#### S12-03 — Smoke test sistema mapas

**Estimación:** 0.5 días  
**Responsable:** gameplay-programmer / QA

**Acceptance criteria:**
- [ ] Juego arranca con Bioma 0 cargado
- [ ] Transición manual a los 5 biomas funciona sin crash
- [ ] Sin memory leak detectado (log de FPS estable)
- [ ] Colisiones de jugador y enemigos no se ven afectadas por el cambio de renderer
- [ ] Build limpio `gradlew build` pasa sin errores

---

### Pista 2 — Assets PixelLab (paralela al código)

> [!note] Esta pista la ejecuta el usuario en la terminal con PixelLab MCP
> Cada story de asset es independiente. Se pueden generar en cualquier orden dentro de la prioridad. Ver [[Mapa-Biomas-PixelLab]] para los prompts completos.

#### S12-A01 — Assets Bioma 0: Abismo Central

**Estimación:** 0.5 días  
**Prioridad:** máxima — es el bioma de inicio

**Entregables:**
- [ ] `abismo_suelo_base.png` (32×32)
- [ ] `abismo_suelo_grieta_leve.png` (32×32)
- [ ] `abismo_suelo_grieta_radial.png` (32×32)
- [ ] `abismo_suelo_placa.png` (32×32)
- [ ] `abismo_columna_rota.png` (32×32)
- [ ] `abismo_fragmento_losa.png` (32×32)
- [ ] Sprite sheet combinado: `abismo_central_tiles.png`
- [ ] TMX creado en Tiled editor: `maps/abismo_central.tmx`

**Ruta de destino:** `assets/tiles/bioma0/` para PNGs individuales, `assets/maps/` para el TMX

---

#### S12-A02 — Assets Bioma 4: Núcleo del Devastador

**Estimación:** 0.5 días  
**Prioridad:** alta — zona del boss final

**Entregables:**
- [ ] `nucleo_suelo_carne.png` (32×32)
- [ ] `nucleo_suelo_vena.png` (32×32)
- [ ] `nucleo_suelo_venas_densas.png` (32×32)
- [ ] `nucleo_suelo_tejido_sano.png` (32×32)
- [ ] `nucleo_garra_void.png` (32×32)
- [ ] `nucleo_quiste_void.png` (32×32)
- [ ] `nucleo_altar_absorbido.png` (32×32)
- [ ] Sprite sheet combinado: `nucleo_devastador_tiles.png`
- [ ] TMX creado en Tiled editor: `maps/nucleo_devastador.tmx`

**Ruta de destino:** `assets/tiles/bioma4/`

---

#### S12-A03 — Assets Bioma 3: Forja del Caos

**Estimación:** 0.5 días

**Entregables:**
- [ ] `forja_suelo_obsidiana.png` (32×32)
- [ ] `forja_suelo_grieta_magma.png` (32×32)
- [ ] `forja_suelo_caliente.png` (32×32)
- [ ] `forja_suelo_escoria.png` (32×32)
- [ ] `forja_pilar_escoria.png` (32×32)
- [ ] `forja_charco_magma.png` (32×32)
- [ ] `forja_metal_incrustado.png` (32×32)
- [ ] Sprite sheet + TMX

**Ruta de destino:** `assets/tiles/bioma3/`

---

#### S12-A04 — Assets Bioma 1: Bosque de Espinas Cristalinas

**Estimación:** 0.5 días

**Entregables:**
- [ ] `cristal_suelo_tierra.png` (32×32)
- [ ] `cristal_suelo_fragmentos.png` (32×32)
- [ ] `cristal_suelo_denso.png` (32×32)
- [ ] `cristal_suelo_musgo.png` (32×32)
- [ ] `cristal_espina_alta.png` (32×32)
- [ ] `cristal_racimo_bajo.png` (32×32)
- [ ] `cristal_raiz_horizontal.png` (32×32)
- [ ] Sprite sheet + TMX

**Ruta de destino:** `assets/tiles/bioma1/`

---

#### S12-A05 — Assets Bioma 2: Pantano de Corrupción

**Estimación:** 0.5 días

**Entregables:**
- [ ] `pantano_suelo_barro.png` (32×32)
- [ ] `pantano_charco_borde.png` (32×32)
- [ ] `pantano_charco_centro.png` (32×32)
- [ ] `pantano_grieta_verde.png` (32×32)
- [ ] `pantano_hongo_void.png` (32×32)
- [ ] `pantano_raiz_podrida.png` (32×32)
- [ ] `pantano_burbuja_gas.png` (32×32)
- [ ] Sprite sheet + TMX

**Ruta de destino:** `assets/tiles/bioma2/`

---

### Pista 3 — Integración final

#### S12-INT — Integración assets + código + smoke test final

**Estimación:** 1 día  
**Dependencia:** S12-01 + S12-02 + S12-A01 completados mínimo

**Descripción:**
Conectar los TMX generados con el `BiomeMapManager` y verificar que la experiencia completa funciona. Ajustes de escala visual si algún tile no encaja en proporción con los personajes.

**Acceptance criteria:**
- [ ] Los 5 biomas cargan sin crash
- [ ] Los tiles se ven a escala correcta respecto a los personajes (personajes ~64px = ~2 tiles ancho)
- [ ] El Bioma 0 es el estado inicial de `GameScreen`
- [ ] Las transiciones de bioma ocurren en los momentos correctos de la partida
- [ ] Sin artefactos visuales (bordes de tiles visibles, tiles desalineados)
- [ ] `gradlew lwjgl3:run` — partida completa desde menú hasta game over/victoria jugando en los 5 biomas

---

## Estimación total

| Categoría | Days |
|-----------|------|
| Código (S12-01 + S12-02 + S12-03) | 3.5 días |
| Assets PixelLab (S12-A01 a A05) | 2.5 días |
| Integración final (S12-INT) | 1 día |
| Buffer (ajustes visuales, bugs) | 1 día |
| **Total estimado** | **8 días** |

---

## Dependencias y riesgos

### Dependencias

| Dependencia | Tipo | Notas |
|-------------|------|-------|
| Sprint 11 completado | HARD | `GameScreen` debe estar estable antes de tocar el renderer |
| PixelLab MCP accesible | HARD | El usuario necesita sesión activa y créditos para generar los 34 assets |
| Tiled editor instalado | SOFT | Necesario para crear TMX. Alternativa: escribir TMX a mano o con script |
| LibGDX TiledMap support | NONE | Ya incluido en LibGDX 1.14.0 — no hay que añadir dependencias |

### Riesgos

> [!warning] Riesgo 1 — Rendimiento del renderer TiledMap
> OrthogonalTiledMapRenderer dibuja todos los tiles visibles por frame. Con una arena de radio 8000f el mapa es grande. Mitigación: usar `setView(camera)` correctamente — solo renderiza tiles en el frustum. Si hay caída de FPS, reducir el tamaño del mapa TMX o usar `SpriteBatch` directo con culling manual.

> [!warning] Riesgo 2 — Escala visual
> Los personajes son sprites de 64px a una escala que coincide con el mundo Box2D. Los tiles de 32px deben escalarse coherentemente. Verificar con `unitScale` del `OrthogonalTiledMapRenderer` — puede ser necesario `new OrthogonalTiledMapRenderer(map, 1f)` o ajustar la escala de la cámara.

> [!warning] Riesgo 3 — Calidad variable de PixelLab
> PixelLab puede generar tiles que no sean seamless o que no respeten la paleta exacta. Plan B: retocar en Aseprite los bordes de tiles antes de combinar en el sprite sheet. Siempre revisar antes de integrar.

> [!tip] Si el tiempo aprieta
> La pista de assets y la pista de código son paralelas. Si los assets del Bioma 0 están listos y el código de S12-01 está completo, se puede hacer el smoke test parcial con solo un bioma y entregar los demás en una iteración posterior. No bloquear el sprint entero por biomas de menor prioridad.

---

## Estructura de archivos esperada al finalizar

```
assets/
  maps/
    abismo_central.tmx
    bosque_cristalino.tmx
    pantano_corrupcion.tmx
    forja_caos.tmx
    nucleo_devastador.tmx
  tiles/
    bioma0/
      abismo_central_tiles.png   ← sprite sheet 32×32 tiles
      abismo_suelo_base.png      ← tile individual (fuente)
      ...
    bioma1/ ... bioma4/ (misma estructura)

core/src/main/java/com/milwar/kaosuarina/
  utils/
    BiomeMapManager.java         ← nuevo
    Constants.java               ← añadir BiomeType enum
  screens/
    GameScreen.java              ← integrar BiomeMapManager
```

---

## Referencias

- [[Ambientacion]] — paletas, concepto y 5 reglas de cohesión de cada bioma
- [[Mapa-Biomas-PixelLab]] — prompts PixelLab completos por asset
- [[Sesion-2026-05-18-Sprint8-Cierre-PostMVP]] — roadmap Sprint 9/10/11
- [[Sesion-2026-05-19-Sprint9-Assets-Implementacion]] — estructura de assets existente (characters/)
