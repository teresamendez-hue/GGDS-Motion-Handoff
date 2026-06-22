---
component: Field Pin Code
component_slug: field-pin-code
platform: web
category: default
semantic_token_enter: motion-curve-md
semantic_token_exit: null
owner: Teresa Mendez
date: 2026-06-18
design_system: GGDS
status: draft
---

## Intent

- Componente de entrada de alta precisión para OTP/PIN.
- Motion interno para reforzar foco, confirmación de ingreso y errores.
- Sin animaciones de entrada/salida de viewport.

## State Model

- Enabled
- Hovered
- Focused
- Typing
- Filled
- Error
- Disabled

## Motion Contract

| Trigger | Visual response | Token | Notes |
|---|---|---|---|
| Focus visible | Ring externo aparece y cambia color | `motion-curve-md` | evitar doble focus ring |
| Digit input | Celda activa hace settle corto | `motion-curve-md` | micro-feedback sin rebote |
| Validation error | Ring y borde migran a error | `motion-curve-md` | priorizar contraste |
| Disabled | Estado final sin transición | `none` | no sugerir interactividad |

## Token Mapping

| Semantic token | Primitive easing | Primitive duration |
|---|---|---|
| `motion-curve-md` | `easing-decelerate` | `duration-300` |

## Accessibility

- Con `prefers-reduced-motion: reduce` aplicar estado final instantáneo.
- Mantener feedback de color/contraste aunque se desactive la animación.
- El focus ring externo debe permanecer visible y no depender solo de motion.
