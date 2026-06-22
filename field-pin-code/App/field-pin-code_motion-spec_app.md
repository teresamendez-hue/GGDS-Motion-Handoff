---
component: Field Pin Code
component_slug: field-pin-code
platform: app
category: default
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Teresa Mendez
date: 2026-06-18
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-md
companion_web_handoff: field-pin-code_motion-handoff_web.html
web_parity_token: motion-curve-md
---

## Intent

- Input multi-celda para PIN/OTP con feedback claro y controlado.
- Motion interno enfocado en foco, ingreso de dígitos y validación.
- Sin animación de entrada/salida de pantalla.

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
| Focus gained | Ring externo aparece | `motion-spring-md` | estabilidad por encima de rebote |
| Digit typed | Celda activa se asienta | `motion-spring-md` | feedback breve por celda |
| Validation failed | Ring/borde migran a error | `motion-spring-md` | mantener legibilidad |
| Disabled | Estado final instantáneo | `none` | evitar affordance falsa |

## Spring Mapping

| Semantic token | Primitive spring | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

## Accessibility

- Con `MediaQuery.disableAnimations` usar `Duration.zero`.
- Mantener feedback semántico por color y contenido, no solo por animación.
- No depender de hover (patrón mobile-first).
