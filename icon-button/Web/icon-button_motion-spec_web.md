---
component: Icon Button
component_slug: icon-button
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Teresa Mendez
date: 2026-06-10
design_system: GGDS
status: draft
---

## Descripción

Componente de acción compacta con estados internos de interacción.

## Tokens

### Estados internos
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

- corner_radius: 4px

| propiedad | uso |
|---|---|
| background-color | hover/pressed/loading |
| transform | pressed |
| opacity | focus ring / loading / skeleton |

## Triggers

- hover: pointer enter/leave
- pressed: pointer down/up
- focus: focus-visible
- loading: estado programático
- skeleton: estado programático
- disabled: estado programático instantáneo

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transición

## Referencias

- tokens_file: references/tokens.md
