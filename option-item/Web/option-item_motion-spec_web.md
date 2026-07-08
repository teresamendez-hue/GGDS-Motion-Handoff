---
component: Option Item
component_slug: option-item
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Teresa Mendez
date: 2026-06-23
design_system: GGDS
status: draft
preview_type: internal_state
optional_children: [Badge, Thumbnail, Checkbox]
disabled_state: false
---

## Descripcion

Fila atomica de seleccion en Web. Estados internos de la fila con motion-curve-sm. Hijos opcionales con motion documentado por separado.

## Tokens

### Web
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

### OptionItemRow
| propiedad | de | a | token |
|---|---|---|---|
| background-color | enabled | hover | motion-curve-sm |
| background-color | hover/default | pressed | motion-curve-sm |
| background-color | default | selected | motion-curve-sm |
| background-color | selected | selected+focus | motion-curve-sm |
| opacity (focus ring) | 0 | 1 | motion-curve-sm |

## Triggers

### Hover
- evento: pointer enter / leave
- tipo: user-action
- delay_ms: 0

### Pressed
- evento: pointer down / up
- tipo: user-action
- delay_ms: 0

### Focus
- evento: focus-visible (teclado)
- tipo: user-action
- delay_ms: 0

### Selected
- evento: click / Enter / Space en fila o consumidor
- tipo: user-action
- delay_ms: 0

## Estados intermedios

### Selected focused
- trigger: fila selected + focus-visible
- propiedades: background-color, focus ring opacity
- token: motion-curve-sm

## Coherencia sistemica

- componentes_relacionados: [Checkbox, Select, Multiple Select, Dropdown, Field Phone Number, Search]
- token_mismo_que: [Checkbox, Radio Button, Box Selector]
- razon: seleccion atomica con estados internos; misma categoria default.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion

## Referencias

- tokens_file: tokens-motion.md
- related_handoffs: [Checkbox, Bottom Sheet App, Field Phone Number]
