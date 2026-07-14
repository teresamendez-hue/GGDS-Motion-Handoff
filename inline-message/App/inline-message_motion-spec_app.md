---
component: Inline Message
component_slug: inline-message
platform: app
category: guide
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Nehuén Benitez
date: 2026-06-29
design_system: GGDS
status: draft
spring_token: motion-spring-md
spring_token_expand: motion-spring-sm
web_parity_token: motion-curve-md
---

## Descripción

Mensaje contextual en flujo de layout en Flutter. Entrada y salida con **fade + expand/collapse vertical** (`opacity` + `height`), equivalente a `fadeIn + expandVertically` en Compose. **Sin translate** — aparece entre el contenido. Expand sin dismiss. Expand/collapse con motion-spring-sm.

## Tokens

### Entrada (bloque)
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28

### Salida (bloque)
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- nota: mismos parámetros, valores invertidos

### Expand / collapse
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

### Bloque — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |
| height (slot) | 0 | intrinsic | motion-spring-md |

### Bloque — Salida (solo Default)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-spring-md |
| height (slot) | intrinsic | 0 | motion-spring-md |

### Detalle Expand — Collapse / expand
| propiedad | de | a | token |
|---|---|---|---|
| height (details) | 0 | intrinsic | motion-spring-sm |
| opacity (details) | 0 | 1 | motion-spring-sm |
| rotation (chevron) | 0deg | 180deg | motion-spring-sm |

## Haptics

| acción | token |
|---|---|
| Aparición | none |
| Tap X (solo Default) | haptic-action-press |
| Tap chevron | haptic-action-press |
| Action Link | none |

## Coherencia sistémica

- componentes_relacionados: [Toast, Coachmark, Tooltip]
- token_mismo_que: [Coachmark]
- razon: categoría Guía; patrón in-flow fade + expand vertical (no spring-lg de Toast flotante)

## Accesibilidad

- flutter_disable_animations: aplicar estado final sin spring

## Referencias

- material_design: https://m3.material.io/components/banner/overview
- compose_animated_visibility: https://developer.android.com/reference/kotlin/androidx/compose/animation/package-summary#AnimatedVisibility(kotlin.Boolean,androidx.compose.ui.Modifier,androidx.compose.animation.EnterTransition,androidx.compose.animation.ExitTransition,kotlin.String,kotlin.Function1)
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=3573-1186
