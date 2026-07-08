---
component: Search
component_slug: search
platform: web
category: default
semantic_token_enter: motion-curve-md
semantic_token_exit: motion-exit
owner: Teresa Mendez
date: 2026-06-23
design_system: GGDS
status: draft
---

## Descripcion

Field de busqueda con lupa fija, clear condicional y panel overlay de resultados. Panel abre con >=3 caracteres o showMenuOnFocus (historial). Web only para este spec; App tiene modos fullscreen e inpage.

## Tokens

### Entrada / estados field
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Clear button
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Salida panel
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-200
- duration_ms: 200

## Propiedades animadas

### SearchWrapper
| propiedad | de | a | token |
|---|---|---|---|
| border-color | neutral | hover/focus/error | motion-curve-md |
| opacity | 1 | 0.45 (disabled) | none |

### SearchRing
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| border-color | neutral | focus/error | motion-curve-md |

### ClearButton
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-sm |

### OverlayPanel — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| translateY | -6px | 0 | motion-curve-md |

### OverlayPanel — Salida
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0 | -6px | motion-exit |

### SearchResultItem
| propiedad | de | a | token |
|---|---|---|---|
| background-color | default | hover/pressed | motion-curve-sm |

Iconos leading (clock, lupa): sin motion. Swap de variante al tipear: sin motion.

## Triggers

### Panel open
- evento: >=3 caracteres en input o showMenuOnFocus con focus vacio
- tipo: user-action
- delay_ms: 0

### Panel close
- evento: seleccion, Escape, click fuera, blur, clear (input vacio)
- tipo: user-action
- auto_delay_ms: null

### Clear
- evento: click en icon button X
- tipo: user-action
- delay_ms: 0

### Result row selected
- evento: click en Search Result Item
- tipo: user-action
- delay_ms: 0

## Estados intermedios

### Hovered
- trigger: pointer enter
- propiedades: border-color
- token: motion-curve-md

### Focused
- trigger: focus-visible
- propiedades: ring opacity, ring border-color
- token: motion-curve-md

### Typing
- trigger: input con texto
- propiedades: border-color
- token: motion-curve-md

### Filled
- trigger: texto en input
- propiedades: clear opacity
- token: motion-curve-sm

### Row hovered
- trigger: pointer enter en fila
- propiedades: background-color
- token: motion-curve-sm

### Row pressed
- trigger: pointer down en fila
- propiedades: background-color
- token: motion-curve-sm

### Row focused
- trigger: focus-visible en fila
- propiedades: focus ring
- token: motion-curve-sm

### Historial visible
- trigger: showMenuOnFocus true y focus sin texto
- propiedades: panel opacity, translateY
- token: motion-curve-md

### Error
- trigger: validation fail
- propiedades: ring border-color, helper color
- token: motion-curve-md

### Disabled
- trigger: disabled true
- propiedades: opacity
- token: none

## Coherencia sistemica

- componentes_relacionados: [Autocomplete, Field Text, Smart Select]
- token_mismo_que: [Autocomplete, Field Text, Option Item (solo token de fila — scope distinto)]
- razon: familia fields; Search Result Item es atomo interno con motion-curve-sm alineado a Option Item pero sin handoff aparte.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion

## Referencias

- material_design: https://m3.material.io/components/search/overview
- apple_hig: no aplica (componente Web)
- tokens_file: tokens-motion.md
- related_handoffs: [Smart Select]
- internal_atoms: [Search Result Item]
