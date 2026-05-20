---
title: "Sistema de Inventario — 6 Slots"
tags:
  - sistema
  - inventario
  - kaosuarina
status: implementado
---

# Sistema de Inventario — 6 Slots

## Motivación

Inspirado en Elden Ring Nightreign. Antes el jugador solo podía tener 2 armas simultáneas y cuando recogía una tercera era forzado a elegir cuál descartar de inmediato (presión cognitiva alta). Con 6 slots:

- Los slots 0-1 son **activos** (armas equipadas con cooldown funcional e inscripción activa)
- Los slots 2-5 son **almacenamiento** (guardan armas con sus inscripciones, sin cooldown, sin uso en combate)
- El jugador puede reorganizar libremente abriendo el inventario con TAB

## Panel Izquierdo (siempre visible en HUD)

Posicionado en x=10 con slots apilados verticalmente. Constantes en `HUD.java`:

```java
LSLOT_X    = 10f
LSLOT_SIZE = 46f
LSLOT_GAP  = 8f
LSLOT_SEP  = 14f   // espacio extra entre activos y almacenamiento
```

Posiciones Y (`lslotY(int i)`):

| Slot | Tipo | Y |
|---|---|---|
| 0 | Activo 1 | 66 |
| 1 | Activo 2 | 120 |
| — | Línea separadora | — |
| 2 | Guardado 1 | 200 |
| 3 | Guardado 2 | 244 |
| 4 | Guardado 3 | 288 |
| 5 | Guardado 4 | 332 |

Los slots activos tienen borde blanco (`Color.WHITE`). Los slots de almacenamiento tienen borde gris (`Color.GRAY`). El icono del arma y la primera línea del nombre se muestran a la derecha del cuadro.

## Inventario TAB (overlay)

Al pulsar **TAB** se abre un overlay oscuro semitransparente con 6 cards en rejilla:

```
┌──────────────┬──────────────┐
│  ACTIVO 1    │  ACTIVO 2    │  ← Fila 0
├──────────────┼──────────────┤
│  GUARDADO 1  │  GUARDADO 2  │  ← Fila 1
├──────────────┼──────────────┤
│  GUARDADO 3  │  GUARDADO 4  │  ← Fila 2
└──────────────┴──────────────┘
```

Cada card muestra:
- Etiqueta de tipo (ACTIVO / GUARDADO)
- Icono del arma (forma de color)
- Nombre completo del arma
- Tier (T1-T5) con color de rarity
- Inscripción activa (si tiene)

### Interacción con ratón

1. **Hover** → borde del card en amarillo claro
2. **Click** → selección (borde dorado, `inventorySelected = slot`)
3. **Click en otro slot** → intercambio de armas entre los dos slots
4. **Click en mismo slot** → deselecciona
5. **TAB o ESC** → cierra inventario (ESC primero comprueba si hay overlay antes de pausar)

El hover se detecta con `HUD.getInventoryHoveredSlot(screenX, screenY)` que convierte coordenadas de pantalla a coordenadas HUD via `hudViewport.unproject()`.

### Tipos de intercambio

- Activo ↔ Activo: intercambia los 2 slots activos
- Activo ↔ Almacenamiento: el arma guardada pasa a estar equipada y viceversa
- Almacenamiento ↔ Almacenamiento: reordena dentro de los 4 slots de guardado

## Flujo de recogida de arma

```
Arma nueva recogida del suelo/cofre
│
├── ¿Slot activo libre? → equipa en slot activo libre
│
├── ¿Almacenamiento libre? → guarda en primer slot libre
│
└── ¿Todo lleno? → muestra menú de swap
        │
        ├── Elige ACTIVO 1 → nuevo arma reemplaza slot 0
        ├── Elige ACTIVO 2 → nuevo arma reemplaza slot 1
        └── CANCELAR → descarta el arma nueva
```

El menú de swap solo muestra los 2 slots activos (no almacenamiento) para simplificar la decisión en combate.

## Archivos modificados

| Archivo | Cambio clave |
|---|---|
| `entities/Player.java` | `storageWeapons[4]`, métodos `storeWeapon()`, `getStorageWeapon()`, `swapActiveWithStorage()`, `swapStorageSlots()`, `isStorageFull()` |
| `ui/HUD.java` | Slot arrays expandidos `[2]→[6]`, `renderWeaponSlots()` en panel izquierdo, `renderInventoryOverlay()`, `inventoryCardRect()`, `getInventoryHoveredSlot()` |
| `screens/GameScreen.java` | Campos `inventoryOpen`, `inventorySelected`; TAB toggle en `render()`; `actualizarHudSlots()` llamado cada frame; `procesarClickInventario()`; condición `anyOverlay` pausar/desbloquear click |
