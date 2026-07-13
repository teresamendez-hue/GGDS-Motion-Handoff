---
component: Progress Indicator
component_slug: progress-indicator
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Nehuén Benitez
date: 2026-07-01
design_system: GGDS
status: draft
preview_type: internal_state
web_parity_token: motion-spring-sm
---

## Descripción

Indicador de progreso determinado. Única animación: cambio de `percentage`. Bar anima `width`; Circular anima `stroke-dashoffset`; texto % en paralelo. Token `motion-curve-sm` (150ms). Sin enter/exit, sin animación en swap Bar/Circular ni toggles de label/description. Skeleton excluido.

## Tokens

### Cambio de percentage
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

### Bar — Cambio de %
| propiedad | de | a | token |
|---|---|---|---|
| width (fill) | % anterior | % nuevo | motion-curve-sm |
| texto % | % anterior | % nuevo | motion-curve-sm |

### Circular — Cambio de %
| propiedad | de | a | token |
|---|---|---|---|
| stroke-dashoffset | offset anterior | offset nuevo | motion-curve-sm |
| texto % (centro) | % anterior | % nuevo | motion-curve-sm |

## Triggers

### Cambio de percentage
- evento: actualización de prop `percentage` (data / programmatic)
- tipo: programmatic
- delay_ms: 0

### Cambio Type
- evento: swap Bar ↔ Circular
- tipo: programmatic
- animacion: ninguna

### Toggle label / description
- evento: cambio de props booleanas
- tipo: programmatic
- animacion: ninguna

## Coherencia sistémica

- componentes_relacionados: [Skeleton (excluido), Toast]
- token_mismo_que: [Button estados, Switch, Accordion micro]
- razon: feedback de valor numérico en microinteracción; motion-curve-sm para cambios de estado internos

## Accesibilidad

- prefers_reduced_motion: aplicar valor final sin transición
- aria: role progressbar, aria-valuenow / min / max

## Referencias

- material_design: https://m3.material.io/components/progress-indicators/overview
- apple_hig: no aplica
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=8353-3332
