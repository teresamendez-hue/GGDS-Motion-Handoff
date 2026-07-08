---
component: Smart Select
component_slug: smart-select
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

Field de seleccion unica con dropdown integrado y Search interno para filtrar listas largas. Web only. Click en field abre panel; filtrado funcional al tipear; cierre por seleccion, click fuera o Tab.

## Tokens

### Entrada / estados field
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Salida panel
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-200
- duration_ms: 200

## Propiedades animadas

### SmartSelectWrapper
| propiedad | de | a | token |
|---|---|---|---|
| border-color | neutral | hover/focus/error | motion-curve-md |
| opacity | 1 | 0.45 (disabled) | none |

### SmartSelectRing
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| border-color | neutral | focus/error | motion-curve-md |

### FieldTrigger
| propiedad | de | a | token |
|---|---|---|---|
| opacity | default | pressed | motion-curve-md |

### DropdownPanel — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| translateY | -6px | 0 | motion-curve-md |

### DropdownPanel — Salida
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0 | -6px | motion-exit |

### ChevronIcon
| propiedad | de | a | token |
|---|---|---|---|
| transform | rotate(0) | rotate(180deg) | motion-curve-md / motion-exit |

## Triggers

### Panel open
- evento: click en field
- tipo: user-action
- delay_ms: 0

### Panel close
- evento: seleccion de opcion, click fuera, Tab al siguiente campo
- tipo: user-action
- auto_delay_ms: null

### Filtrado
- evento: tipeo en Search interno del panel
- tipo: user-action
- delay_ms: 0

## Estados intermedios

### Hovered
- trigger: pointer enter
- propiedades: border-color
- token: motion-curve-md

### Focused
- trigger: click en field
- propiedades: ring opacity, ring border-color
- token: motion-curve-md

### Filled
- trigger: opcion seleccionada
- propiedades: none en trigger
- token: none

### Error
- trigger: validation fail
- propiedades: ring border-color, helper color
- token: motion-curve-md

### Disabled
- trigger: disabled true
- propiedades: opacity
- token: none

## Coherencia sistemica

- componentes_relacionados: [Select, Autocomplete, Option Item]
- token_mismo_que: [Select, Multiple Select, Autocomplete]
- razon: misma familia fields con dropdown; Search interno enlazado a handoff Search (field embebido).

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion

## Referencias

- material_design: https://m3.material.io/components/menus/overview
- apple_hig: no aplica (componente Web)
- tokens_file: tokens-motion.md
- related_handoffs: [Search, Option Item]
