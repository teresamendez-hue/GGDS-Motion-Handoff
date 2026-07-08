---
component: Accordion
component_slug: accordion
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Nehuén Benitez
date: 2026-06-30
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onHeaderTap
---

## Descripción

Ítem de acordeón único en Flutter. Expand/collapse del slot con **height + opacity inner + chevron rotate 180deg** vía `motion-spring-sm`. Sin viewport enter/exit. Type Default/Container sin animación. Haptic selection en toggle.

## Tokens

### Expand / collapse
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- nota: mismo token expand y collapse

### Header pressed
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

### Slot — Expand
| propiedad | de | a | token |
|---|---|---|---|
| height (body) | 0 | intrinsic | motion-spring-sm |
| opacity (body inner) | 0 | 1 | motion-spring-sm |
| rotation (chevron) | 0deg | 180deg | motion-spring-sm |

### Slot — Collapse
| propiedad | de | a | token |
|---|---|---|---|
| height (body) | intrinsic | 0 | motion-spring-sm |
| opacity (body inner) | 1 | 0 | motion-spring-sm |
| rotation (chevron) | 180deg | 0deg | motion-spring-sm |

### Header — Pressed
| propiedad | de | a | token |
|---|---|---|---|
| scale / opacity | 1, 1 | pressed | motion-spring-sm |

## Triggers

### Expand
- evento: tap en header (collapsed → expanded)
- tipo: user-action
- delay_ms: 0

### Collapse
- evento: tap en header (expanded → collapsed)
- tipo: user-action
- delay_ms: 0

### Cambio Type
- evento: swap Default ↔ Container
- tipo: programmatic
- animacion: ninguna

## Haptics

| acción | token |
|---|---|
| Tap header toggle | haptic-selection-change |
| Cambio Type | none |

## Coherencia sistémica

- componentes_relacionados: [Inline Message (Expand), Switch, Checkbox]
- token_mismo_que: [Inline Message expand, Button estados]
- razon: reveal/collapse de microinteracción; motion-spring-sm alineado con expand de Inline Message

## Accesibilidad

- flutter_disable_animations: aplicar estado final sin spring

## Referencias

- material_design: https://m3.material.io/components/lists/guidelines#expandable-lists
- apple_hig: no aplica
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=9840-1771
