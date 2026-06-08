---
title: "Sesión 2026-06-04 — Sprint 4: 12 Armas + 8 Enemigos + El Fragmentado"
tags:
  - sesion
  - sprint-4
  - cnt-01
  - cnt-02
  - armas
  - enemigos
fecha: 2026-06-04
sprint: sprint-plan-s4
estado: completado
---

# Sesión 2026-06-04 — Sprint 4

## CNT-01 — 12 nuevas armas (pool 12 → 24)

### Melee
| weapon_id | Nombre | DmgType | Nota |
|-----------|--------|---------|------|
| W_GREATAXE | Hacha de Guerra | FÍSICO | dmg 22-34, atk 0.55 |
| W_RAPIER | Estoque | FÍSICO | crit 15% base, multi 2.0× |
| W_WARSCYTHE | Guadaña de Guerra | FÍSICO | dmg 18-27 |
| W_FLAIL | Mayal | FÍSICO | varianza máxima (6-28) |

### Ranged
| weapon_id | Nombre | DmgType | Nota |
|-----------|--------|---------|------|
| W_SNIPER | Rifle de Precisión | A_DISTANCIA | dmg 28-40, lento |
| W_SHOTGUN | Escopeta de Combate | A_DISTANCIA | rápida, 5-9 dmg |
| W_EXPLOSIVE | Lanzagranadas | FUEGO | on-hit FUEGO |
| W_BOLAS | Boleadoras | A_DISTANCIA | slow on-hit |

### Magic
| weapon_id | Nombre | DmgType | Nota |
|-----------|--------|---------|------|
| W_CHAINSTAFF | Vara del Rayo | MÁGICO | chain lightning |
| W_BLOODTOME | Grimorio de Sangre | CAOS_PRIMORDIAL | lifesteal |
| W_VOIDSTAFF | Bastón del Vacío | CAOS | |
| W_FROSTORB | Orbe Glacial | MÁGICO | slow on-hit |

**Archivos:** `weapons.json` (+12 entradas), `WeaponDropper.java` (TYPE_MAP + DMG_MAP + arrays)

---

## CNT-02 — 8 enemigos nuevos + El Fragmentado

| Tipo | AI | Mecánica clave |
|------|----|----------------|
| BERSERKER | Carga directa | <20% HP → berserkActive → speed 420f |
| SPLITTER | Normal → split | pendingSplit → 2 BASICO al morir |
| HEALER | Huye player | healThisFrame/s → cura radio 150u |
| SHIELDER | Lento | defensa 35f base |
| ELITE_CHARGE | 3 estados | orbit→telegraph 1.5s→charge 800f |
| ELITE_SUMMON | Huye | summonThisFrame cada 8s → 3 BASICO |
| ELITE_ZONE | Estacionario | slow 45% al player en radio 120u |
| FRAGMENTADO | Boss wave 30 | 3 fases (67%/33% HP): físico→mágico→caos+espiral |

**Archivos:** `Enemy.java` (enum + activate + update), `PoolEnemigos.java` (spawn + tipoAleatorio), `SpawnManager.java` (eliteTipo + spawnBossesForWave), `Constants.java`, `GameScreen.java` (procesarSeñalesEnemigosSprint4)

---

## Build PASS — EXIT 0

---

## Referencias

- [[Sesion-2026-06-04-Sprint3]] — sesión anterior
- [[../Proyectos/Kaosuarina-Progreso]] — tracking general
