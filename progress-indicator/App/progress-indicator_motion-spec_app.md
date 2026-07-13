---
component: Progress Indicator
component_slug: progress-indicator
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: null
owner: Nehuén Benitez
date: 2026-07-01
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
web_parity_token: motion-curve-sm
haptic_semantic_default: none
haptic_override_allowed: false
---

## Descripción

Indicador de progreso determinado en Flutter. Única animación: cambio de `percentage` con `motion-spring-sm`. Bar: width; Circular: stroke-dashoffset; texto % en paralelo. Sin enter/exit, sin haptics, sin animación en swap de tipo. Skeleton excluido.

## Tokens

### Cambio de percentage
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

### Bar — Cambio de %
| propiedad | de | a | token |
|---|---|---|---|
| width (fill) | % anterior | % nuevo | motion-spring-sm |
| texto % | % anterior | % nuevo | motion-spring-sm |

### Circular — Cambio de %
| propiedad | de | a | token |
|---|---|---|---|
| stroke-dashoffset | anterior | nuevo | motion-spring-sm |
| texto % | anterior | nuevo | motion-spring-sm |

## Triggers

### Cambio de percentage
- evento: actualización de prop `percentage`
- tipo: programmatic
- delay_ms: 0

### Cambio Type
- evento: swap Bar ↔ Circular
- tipo: programmatic
- animacion: ninguna

## Haptics

| acción | token |
|---|---|
| Cambio de % | none |
| Cambio Type | none |

## Coherencia sistémica

- componentes_relacionados: [Accordion, Switch]
- token_mismo_que: [Accordion expand, Button estados]
- razon: microinteracción de valor; motion-spring-sm alineado con feedback interno App

## Accesibilidad

- flutter_disable_animations: aplicar valor final sin spring
- semantics: progress indicator con value

## Referencias

- material_design: https://m3.material.io/components/progress-indicators/overview
- apple_hig: no aplica
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=7989-1225
