---
component: Multiple Select
component_slug: multiple-select
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

Field de seleccion multiple con dropdown integrado. Estados internos del field y apertura/cierre del panel. Contenido del panel: Option Items. Persistencia como Chips (handoff Chip).

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

### MultipleSelectWrapper
| propiedad | de | a | token |
|---|---|---|---|
| border-color | neutral | hover/focus/error | motion-curve-md |
| opacity | 1 | 0.45 (disabled) | none |

### MultipleSelectRing
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
- evento: focus o click en field
- tipo: user-action
- delay_ms: 0

### Panel close
- evento: Escape, click fuera
- tipo: user-action
- auto_delay_ms: null

## Estados intermedios

### Hovered
- trigger: pointer enter
- propiedades: border-color
- token: motion-curve-md

### Focused
- trigger: focus-visible
- propiedades: ring opacity, ring border-color
- token: motion-curve-md

### Filled
- trigger: chips presentes en el field
- propiedades: ver handoff Chip
- token: none (en este componente)

### Error
- trigger: validation fail
- propiedades: ring border-color, helper color
- token: motion-curve-md

### Disabled
- trigger: disabled true
- propiedades: opacity
- token: none

## Coherencia sistemica

- componentes_relacionados: [Field Text, Field Phone Number, Option Item]
- token_mismo_que: [Field Text, Field Phone Number]
- razon: misma familia fields; panel usa par curve-md / motion-exit.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion

## Referencias

- material_design: https://m3.material.io/components/menus/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/pickers
- tokens_file: tokens-motion.md
- related_handoffs: [Option Item, Chip]
