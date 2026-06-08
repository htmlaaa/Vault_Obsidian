---
title: "Sesión 2026-06-04 — Sprint 5: Meta-progresión Tokens + Evolución de Armas"
tags:
  - sesion
  - sprint-5
  - mec-01-plan-a
  - cnt-05
  - meta-progresion
  - weapon-evolution
fecha: 2026-06-04
sprint: sprint-plan-s5
estado: completado
---

# Sesión 2026-06-04 — Sprint 5

## MEC-01 Plan A — Meta-progresión tokens

### DB — tabla `meta_tokens`
- Una sola fila persistente: `id=1, total=N`
- `INSERT OR IGNORE` garantiza que siempre existe aunque sea la primera run

### Fórmula de tokens
```
tokens = max(1, score/200 + level*2 + waves)
```
Ejemplo: score 5000 + nivel 15 + oleada 20 → 25+30+20 = **75 tokens**

### Archivos modificados
| Archivo | Cambio |
|---------|--------|
| `db/DBManager.java` | DDL `meta_tokens`, `addTokens(int)`, `getTokensTotal()`, `calcularTokens(score,level,waves)` |
| `screens/GameScreen.java` | `renderGameOver()` calcula tokens, llama `DBManager.addTokens()`, pasa a `GameOverScreen` |
| `screens/GameOverScreen.java` | Constructor sobrecargado con `tokensEarned`/`tokensTotal`, muestra en pantalla dorado |

---

## CNT-05 — Sistema de evolución de armas

### Mecánica
Al recoger un arma con el mismo `weapon_id` que ya tienes equipada → **evolución automática**:
- Se busca en `WeaponEvolutionCatalog` el `result_id`
- Se genera la arma evolucionada (depth + 2 para subir 1-2 tiers)
- **Hereda la inscripción** del arma base equipada
- HUD muestra `¡EVOLUCIÓN! [nombre]` parpadeante por 2.5s

### Pares de evolución implementados
| Trigger | Resultado | Cambio |
|---------|-----------|--------|
| W_SHORTSWORD ×2 | W_LONGSWORD | WT_GREATSWORD, mayor dmg |
| W_DUAL_PISTOLS ×2 | W_CUATRO_PISTOLAS | WT_PISTOL, mayor cadencia |
| W_APP_STAFF ×2 | W_ARCANE_CANON | WT_STAFF, mayor dmg mágico |
| W_FLAMEBLADE ×2 | W_INFERNO_BLADE | WT_SWORD, DMG_FIRE amplificado |
| W_HUNTBOW ×2 | W_WARBOW | WT_BOW, crit 12%, perforación |
| W_CHAOS_WAND ×2 | W_VOID_CANNON | WT_WAND, CAOS_PRIMORDIAL |

### Archivos nuevos/modificados
| Archivo | Cambio |
|---------|--------|
| `weapons/weapon_evolutions.json` | **nuevo** — 6 entradas de referencia (tipo duplicate) |
| `weapons.json` | +6 armas evolucionadas (W_LONGSWORD, W_CUATRO_PISTOLAS, W_ARCANE_CANON, W_INFERNO_BLADE, W_WARBOW, W_VOID_CANNON) |
| `weapons/WeaponEvolutionCatalog.java` | **nuevo** — mapa estático trigger→result |
| `weapons/WeaponDropper.java` | `generateById(weaponId, depth)` + TYPE_MAP/DMG_MAP para 6 evolucionadas |
| `screens/GameScreen.java` | Hook de evolución en `triggerWeaponPickup()` |
| `ui/HUD.java` | `showEvolutionNotification(name)`, render parpadeante centrado en pantalla |

---

## Build PASS — EXIT 0

---

## Estado del SPRINT_PLAN al cierre de Sprint 5

| Sprint | Estado |
|--------|--------|
| S1 (Bugs + Dificultad) | ✅ Completado |
| S2 (Visuales + MEC-03) | ✅ Completado |
| S3 (Amuletos + Upgrades + Contenido básico) | ✅ Completado |
| S4 (12 Armas + 8 Enemigos + Boss) | ✅ Completado |
| S5 (Meta-progresión + Evolución armas) | ✅ Completado |

---

## Referencias

- [[Sesion-2026-06-04-Sprint4]] — sesión anterior
- [[../Proyectos/Kaosuarina-Progreso]] — tracking general
