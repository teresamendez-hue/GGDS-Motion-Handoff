---
component: Amount Input
component_slug: amount-input
platform: app
category: default
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Nehuén Benitez
date: 2026-06-30
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-md
companion_web_handoff: null
web_parity_token: null
haptic_semantic_default: none
haptic_override_allowed: false
haptic_trigger: none
figma_url: "https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=7020-1468"
---

## Descripción

Campo de monto monetario para App con prefijo de moneda estático, icono de visibilidad y slots opcionales de badge y helper text. Motion limitado a estados internos; sin entrada/salida de viewport.

## Tokens

### Focus · typing · filled
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: "Color de tipografía neutral-alt → on-input. Sin borders ni ring."

### Muted · badge · helper
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- note: "Crossfade visible↔bullets; show/hide badge y helper (opacity + height)"

### Sin motion
- semantic: none
- note: "Disabled (opacity instantánea); cambio de currency o size"

## Propiedades animadas

### Amount text — Focus / typing / filled
| propiedad | de | a | token |
|---|---|---|---|
| color | `#6C6C78` (neutral-alt) | `#252529` (on-input) | motion-spring-md |

### Amount value — Muted toggle
| propiedad | de | a | token |
|---|---|---|---|
| opacity (visible layer) | 1 | 0 | motion-spring-sm |
| opacity (bullets layer) | 0 | 1 | motion-spring-sm |

### Eye icon — Muted toggle
| propiedad | de | a | token |
|---|---|---|---|
| asset | Eye Open | Eye Closed | none |

### TextBadge — Show / hide
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-sm |
| height | 0 | intrinsic | motion-spring-sm |

### HelperText — Show / hide
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-sm |
| height | 0 | intrinsic | motion-spring-sm |

### Root — Disabled
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0.45 | none |

## Triggers

### Focus
- evento: tap en campo / focus programático
- tipo: user-action
- delay_ms: 0

### Typing
- evento: cambio de valor numérico
- tipo: user-action
- delay_ms: 0

### Visibility toggle
- evento: tap en icono eye
- tipo: user-action
- delay_ms: 0

### Badge / helper
- evento: prop `badge` / `helperText` true|false
- tipo: programmatic
- delay_ms: 0

### Disabled
- evento: prop `enabled` false
- tipo: programmatic
- delay_ms: 0

## Estados intermedios

### Muted
- trigger: visibility off o prop input=Muted
- propiedades: cada dígito del monto formateado se reemplaza por `•` (separadores visibles); crossfade visible ↔ enmascarado
- token: motion-spring-sm

### Typing
- trigger: edición activa con caret
- propiedades: color de tipografía (mismo token que focus)
- token: motion-spring-md

## Variantes (sin motion)

| variante | valores | animación |
|---|---|---|
| Currency | None, Guaraníes (Gs.), Dólares (US$), Reales (R$) | instantáneo |
| Size | xs, s, m (tipografía + alto de fila + icon button) | instantáneo |

## Skeleton

- incluido_en_handoff: false
- razon: "Shimmer y proporciones de Figma (9311:2940) no replicables fielmente en preview HTML"

## Coherencia sistémica

- componentes_relacionados: [Field Text, Field Prefix, Text Area]
- token_mismo_que: [Field Text — motion-spring-md para color de tipografía en focus/typing]
- razon: "Campo de formulario sin borders; legibilidad en tipeo; microinteracciones de slot con motion-spring-sm"

## Haptics

- requiere: false
- nota: "Form input — sin haptic en campo ni icono de visibilidad"

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- material_design: "no aplica — componente custom GGDS"
- apple_hig: "no aplica"
- tokens_file: "references/tokens-motion.md"
- figma_node: "7020:1468"
