# Sesión 2026-05-20 — Sprint 10: Cierre · Rediseño Sistema Enemigos

**Estado:** ✅ Completado  
**Build:** PASS (gradlew :core:compileJava → EXIT 0)  
**Duración real:** ~1 sesión intensiva (junto a Sprint 12 planificación, balance review, y fix Shooter)

---

## Contexto de la sesión

Esta sesión cerró varios frentes en paralelo:
- **Fix Shooter** (no ocupar slot, auto-shoot por rango, bloquear solo PISTOLAS_GEMELAS del suelo)
- **Balance tweaks** (HPs, IDs de enemigos, invulnerabilidad por rol)
- **Sprint 10 fullmine** — todo el rediseño de enemigos en una sola sesión
- **Sprint 12 planificación** — plan de mapas y biomas (ver [[Sesion-2026-05-20-Sprint12-Mapas-Plan]])

---

## Fix Shooter (pre-Sprint 10)

| Item | Cambio |
|------|--------|
| Slot inicial | Shooter **no equipa** PISTOLAS_GEMELAS al empezar — sin slot ocupado |
| Auto-shoot | `Player.updateShooterAutoFire()` — detecta enemigo más cercano en `SHOOTER_AUTO_RANGE = 600f` y dispara 2 balas (±3°) con `SHOOTER_AUTO_DAMAGE = 18` |
| Cooldown | Usa `stats.baseShootCooldown` del rol |
| Weapon restriction | Solo bloquea coger `WeaponType.PISTOLAS_GEMELAS` del suelo — otras armas OK |

**Archivos:** `Player.java` (nuevo método `updateShooterAutoFire`), `GameScreen.java` (llamada en loop para rol SHOOTER, fix en `triggerWeaponPickup`)

---

## Balance tweaks aplicados

| Cambio | Valor anterior | Valor nuevo |
|--------|----------------|-------------|
| TANQUE HP | 180 | 200 |
| RAPIDO HP | 12 | 25 |
| SHOOTER-enemy HP | 50 | 35 |
| GUARDIAN_HP | 800 | 900 |
| ARQUERO_HP | 500 | 300 |
| DEVASTADOR_HP | 2000 | 2200 |
| Invulnerabilidad Caballero | 1.0s (fija) | 0.6s |
| Invulnerabilidad Mago | 1.0s (fija) | 0.7s |
| Invulnerabilidad Shooter | 1.0s (fija) | 0.45s |

La invulnerabilidad ahora es **por rol** (`invulnerabilityTime` como `final float` inicializado en el constructor via switch). Motivación: robo de vida era demasiado abusivo con i-frames largos.

---

## Sprint 10 — Stories completadas

### S10-01: FUEGO y CAOS_PRIMORDIAL reworks

**FUEGO:**
- Amplificador de daño base: ×1.15 (`STATUS_FUEGO_DMG_MULT`) antes de reducciones
- DoT: 4.5s duración (`STATUS_FIRE_DURATION`), 2-3 dmg/tick aleatorio por hit (`STATUS_FIRE_DMG_MIN/MAX`)
- Tinto visual: rojo-naranja (sin cambio — ya existía)

**CAOS_PRIMORDIAL:**
- **Daño verdadero** — devuelve `Math.max(1, raw)` antes del switch de reducciones, ignorando TODA defensa plana y porcentual
- **Burn secuela:** 4.5s (BURN_DURATION × 1.5), 12 dmg/tick (BURN_DAMAGE + 4)
- **Slow:** 2s al 50% velocidad (`CAOS_PRIMORDIAL_SLOW_DURATION/MULT`)
  - Respeta inmunidad JSON `EFF_SLOW` (EN_MINIBOSS, EN_BOSS_FINAL, EN_BOSS_SECRET inmunes)

**Nuevas constantes en `Constants.java`:**
```
STATUS_FIRE_DURATION      = 4.5f
STATUS_FIRE_DMG_MIN       = 2
STATUS_FIRE_DMG_MAX       = 3
STATUS_FUEGO_DMG_MULT     = 1.15f
CAOS_PRIMORDIAL_SLOW_DURATION = 2f
CAOS_PRIMORDIAL_SLOW_MULT     = 0.5f
```

**Nuevos campos en `Enemy.java`:**
```java
public String enemyId = null;   // ID JSON del catálogo
public float  slowTimer = 0f;   // tiempo restante del slow
public float  slowMult  = 1f;   // multiplicador de velocidad (0.5 = mitad)
```

La lógica de slow en `update()` escala la velocidad justo antes de `position.add(...)`:
```java
if (slowTimer > 0) { slowTimer -= delta; velocity.scl(slowMult); }
```

---

### S10-02: Resistencias/inmunidades JSON en calcularDaño

**Flujo de daño completo (ColisionManager.calcularDaño):**
1. Si FUEGO → `raw *= 1.15f`
2. Casos especiales ESPECTRAL (inmune a FISICO, ×1.5 a FUEGO) y GUARDIAN MAGICO
3. Si `ignoresDefense` → devuelve `raw` directo
4. Si CAOS_PRIMORDIAL → devuelve `raw` (true damage, salta todo)
5. Switch de reducción plana (defensa/resistenciaMagica según tipo)
6. **Capa JSON:** `DataManager.getResistPct(enemyId, dmgId)` → aplica `× (1 - pct/100f)`

**Helper `dmgToId(DamageType)`:**
```
FISICO      → "DMG_PHYSICAL"
MAGICO      → "DMG_MAGIC"
A_DISTANCIA → "DMG_RANGED"
FUEGO       → "DMG_FIRE"
VENENO      → "DMG_POISON"
CAOS        → "DMG_CHAOS"
```

**Efectos en juego (ejemplos de enemyresistances.json):**
- EN_TANK: –40% a FISICO, –25% a A_DISTANCIA, +15% a MAGICO (vulnerable)
- EN_ELITE_TP (Arquero): –30% a MAGICO, –15% a A_DISTANCIA
- EN_MINIBOSS: –35% a FISICO
- EN_BOSS_FINAL: –45% a MAGICO, –30% a FUEGO, –20% a FISICO

---

### S10-03: Depth Scaling activo en spawn

**`PoolEnemigos.java`:**
- Nuevo campo `private int currentDepth = 1`
- `setCurrentDepth(int depth)` — llamado desde GameScreen cada frame
- `spawn()` aplica `enemy.applyDepthScaling(DataManager.getInstance().getDepthScaling(currentDepth))` tras activate

**`Enemy.applyDepthScaling(DepthScalingData d)`:**
```java
if (d == null || d.hpMult <= 0) return;
health = maxHealth = Math.max(1, Math.round(health * d.hpMult));
speed *= d.speedMult;
```

**`GameScreen.actualizarJuego()`:**
```java
poolEnemigos.setCurrentDepth(hud.getLevel());
```
Se llama justo antes de `spawnManager.update()`.

**depthscaling.json — curva completa (depths 1-10):**

| Depth | hp_mult | dmg_mult | speed_mult | elite_chance |
|-------|---------|----------|------------|--------------|
| 1 | 1.00 | 1.00 | 1.00 | 5% |
| 2 | 1.15 | 1.10 | 1.00 | 8% |
| 3 | 1.35 | 1.20 | 1.05 | 12% |
| 4 | 1.60 | 1.35 | 1.05 | 16% |
| 5 | 1.90 | 1.50 | 1.10 | 20% |
| 6 | 2.25 | 1.65 | 1.13 | 24% |
| 7 | 2.65 | 1.82 | 1.17 | 28% |
| 8 | 3.10 | 2.00 | 1.20 | 32% |
| 9 | 3.60 | 2.20 | 1.22 | 36% |
| 10 | 4.20 | 2.40 | 1.25 | 40% |

> Depths 9-10 tenían `""` vacíos (bug: GSON fallaba y la lista entera quedaba vacía). Corregido con valores extrapolados de la curva.

---

### S10-04: Inmunidades a CC (stun/slow)

**InscripcionSismica:** comprueba `DataManager.isImmune(enemyId, "EFF_STUN")` antes de aplicar `stunTimer`. Enemigos inmunes a stun: EN_TANK, EN_ELITE_AOE, EN_MINIBOSS, EN_BOSS_FINAL, EN_BOSS_SECRET.

**CAOS_PRIMORDIAL slow:** comprueba `EFF_SLOW` antes de setear `slowTimer`. Inmunes al slow: EN_ELITE_AOE, EN_MINIBOSS, EN_BOSS_FINAL, EN_BOSS_SECRET.

---

## Archivos modificados

| Archivo | Cambios |
|---------|---------|
| `utils/Constants.java` | +6 constantes FUEGO/CAOS_PRIMORDIAL, balance stats |
| `entities/Enemy.java` | +`enemyId`, `slowTimer`, `slowMult`, `applyDepthScaling()`, reset en `activate()`, slow en `update()` |
| `utils/ColisionManager.java` | FUEGO ×1.15, CAOS_PRIMORDIAL true damage, capa JSON, CAOS_PRIMORDIAL slow, fire DoT aleatorio, `dmgToId()` helper |
| `entities/PoolEnemigos.java` | `currentDepth`, `setCurrentDepth()`, depth scaling en `spawn()` |
| `weapons/inscriptions/InscripcionSismica.java` | Check inmunidad EFF_STUN |
| `screens/GameScreen.java` | Shooter auto-fire, fix weapon pickup, `setCurrentDepth()` |
| `assets/game_data_json/enemies/depthscaling.json` | Depths 9-10 poblados (fix bug GSON) |

---

## Pendientes / Deuda técnica detectada

- `dmg_mult` del depth scaling **no afecta** daño de contacto ni proyectiles enemigos — los valores de daño en `Constants.java` son fijos. Sprint posterior si se quiere escalar daño enemigo.
- El `enemyId` del **DEVASTADOR** apunta a `"EN_BOSS_FINAL"` — si se añade un secreto `EN_BOSS_SECRET` habrá que crear un nuevo `Enemy.Tipo` o reutilizar el existente.
- Audio pendiente desde Sprint 7 — ver [[../Pendientes/Sprint7-Assets-Diseño]].

---

## Siguiente sprint recomendado

→ **Sprint 11: Rediseño Sistema Armas** — generación procedural, tiers T1-T5, afijos, loot drops  
Ver plan en [[../Proyectos/Kaosuarina-Progreso]] y GDD en `Code_Game/design/gdd/weapon-system-redesign`
