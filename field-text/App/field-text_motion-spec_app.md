---
component: Field Text
component_slug: field-text
platform: app
category: default
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Teresa Mendez
date: 2026-06-17
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-md
companion_web_handoff: field-text_motion-handoff_web.html
web_parity_token: motion-curve-md
---

## Intent

- Campo de texto para App con feedback de foco y validación consistente.
- Motion de estado interno, sin animaciones de entrada/salida de pantalla.
- `motion-spring-md` para estabilidad durante tipeo y corrección.

## State Model

- Enabled
- Focused
- Typing
- Filled
- Error
- Disabled

## Motion Contract

| Trigger | Visual response | Token | Notes |
|---|---|---|---|
| Focus gained | Ring externo aparece y estabiliza | `motion-spring-md` | evitar oscilación excesiva |
| Text changed | Borde/superficie se asientan | `motion-spring-md` | prioridad a legibilidad |
| Validation failed | Ring/borde migran a error | `motion-spring-md` | feedback semántico fuerte |
| Disabled | Estado final instantáneo | `none` | sin affordance activa |

## Spring Mapping

| Semantic token | Primitive spring | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

## Accessibility

- Respetar `MediaQuery.disableAnimations` con `Duration.zero`.
- Mantener señales visuales (foco/error/disabled) sin depender del motion.
- No modelar hover en App (interacción táctil).
