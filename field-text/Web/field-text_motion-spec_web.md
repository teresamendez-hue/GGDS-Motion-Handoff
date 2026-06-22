---
component: Field Text
component_slug: field-text
platform: web
category: default
semantic_token_enter: motion-curve-md
semantic_token_exit: null
owner: Teresa Mendez
date: 2026-06-17
design_system: GGDS
status: draft
---

## Intent

- Campo de texto general para captura de datos con foco en legibilidad.
- Motion interno para foco, edición y error, sin animaciones de viewport.
- Ritmo `md` para una sensación estable durante escritura.

## State Model

- Enabled
- Hovered
- Focused
- Typing
- Empty
- Filled
- Error
- Disabled

## Motion Contract

| Trigger | Visual response | Token | Notes |
|---|---|---|---|
| Pointer enter | Ajuste de borde/superficie | `motion-curve-md` | hover solo Web |
| Focus visible | Ring externo aparece | `motion-curve-md` | no duplicar ring interno |
| Text input | Contenedor se asienta en estado activo | `motion-curve-md` | evitar distracción |
| Validation error | Ring/borde a estado error | `motion-curve-md` | mantener contraste AA |
| Disabled | Estado final instantáneo | `none` | sin transición |

## Token Mapping

| Semantic token | Primitive easing | Primitive duration |
|---|---|---|
| `motion-curve-md` | `easing-decelerate` | `duration-300` |

## Accessibility

- Con `prefers-reduced-motion: reduce`, aplicar estados finales instantáneos.
- El ring de foco externo debe verse sin depender de opacidad animada.
- Error/focus deben conservar distinción por color y grosor.
