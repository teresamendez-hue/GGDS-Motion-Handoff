---
component: Inline Message
component_slug: inline-message
platform: web
category: guide
semantic_token_enter: motion-curve-md
semantic_token_exit: motion-exit
owner: Nehuén Benitez
date: 2026-06-29
design_system: GGDS
status: draft
---

## Descripción

Mensaje contextual en flujo de layout (no flotante). Entrada y salida con **fade + expand/collapse vertical** del slot (`grid-template-rows` 0fr↔1fr + `opacity`), patrón equivalente a `fadeIn + expandVertically` en Compose / M3. **Sin translate** — el bloque aparece entre el contenido y empuja a los hermanos. Variante Expand sin cierre.

## Tokens

### Entrada (bloque)
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Salida (bloque)
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-200
- duration_ms: 200

### Expand / collapse
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

### Bloque — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| grid-template-rows (slot) | 0fr | 1fr | motion-curve-md |
| opacity | 0 | 1 | motion-curve-md |

### Bloque — Salida (solo Default)
| propiedad | de | a | token |
|---|---|---|---|
| grid-template-rows (slot) | 1fr | 0fr | motion-exit |
| opacity | 1 | 0 | motion-exit |

### Detalle Expand — Collapse / expand
| propiedad | de | a | token |
|---|---|---|---|
| grid-template-rows (details) | 0fr | 1fr | motion-curve-sm |
| opacity (details inner) | 0 | 1 | motion-curve-sm |
| transform (chevron svg) | 0deg | 180deg | motion-curve-sm |

## Triggers

### Apertura
- evento: mount con mensaje visible o show programático (ej. error de validación)
- tipo: programmatic
- delay_ms: 0

### Cierre (solo Default)
- evento: tap en botón cerrar (X)
- tipo: user-action
- auto_delay_ms: null

### Variante Expand
- dismiss: no aplica
- close_button: false

### Expand / collapse
- evento: tap en chevron (variante Expand)
- tipo: user-action
- delay_ms: 0

## Coherencia sistémica

- componentes_relacionados: [Toast, Coachmark, Tooltip]
- token_mismo_que: [Coachmark]
- razon: categoría Guía; mensaje en layout usa motion-curve-md (no lg de Toast flotante). Patrón in-flow alineado con M3/Compose fade + expand vertical.

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: no aplica

## Referencias

- material_design: https://m3.material.io/components/banner/overview
- compose_animated_visibility: https://developer.android.com/reference/kotlin/androidx/compose/animation/package-summary#AnimatedVisibility(kotlin.Boolean,androidx.compose.ui.Modifier,androidx.compose.animation.EnterTransition,androidx.compose.animation.ExitTransition,kotlin.String,kotlin.Function1)
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=4698-346
