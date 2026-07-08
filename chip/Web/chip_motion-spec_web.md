---
component: Chip
component_slug: chip
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: motion-exit
owner: Teresa Mendez
date: 2026-06-23
design_system: GGDS
status: draft
variants: [informational, removable, disabled]
---

## Descripcion

Pill compacto para atributos, selecciones o filtros. Enter al montar, exit al dismiss, estados internos en variante removible e informativa.

## Tokens

### Enter / estados internos
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Exit dismiss
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-100
- duration_ms: 100

## Propiedades animadas

### Chip — Enter
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-sm |
| transform | scale(0.96) | scale(1) | motion-curve-sm |

### Chip — Exit
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| transform | scale(1) | scale(0.96) | motion-exit |

### Chip — Hover
| propiedad | de | a | token |
|---|---|---|---|
| background-color | default | hover | motion-curve-sm |

### ChipDismiss — Pressed
| propiedad | de | a | token |
|---|---|---|---|
| transform | scale(1) | scale(0.92) | motion-curve-sm |

### ChipDismiss — Focus ring
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-sm |

## Triggers

### Enter
- evento: consumidor monta chip en DOM
- tipo: programmatic
- delay_ms: 0

### Exit
- evento: click en dismiss (variante removible)
- tipo: user-action
- delay_ms: 0

## Estados intermedios

### Disabled
- trigger: disabled true
- propiedades: opacity
- token: none

## Coherencia sistemica

- componentes_relacionados: [Multiple Select, Button, Icon Button]
- token_mismo_que: [Button, Icon Button, Option Item]
- razon: microinteraccion compacta; exit usa par sm / motion-exit.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion

## Referencias

- tokens_file: tokens-motion.md
- related_handoffs: [Multiple Select]
