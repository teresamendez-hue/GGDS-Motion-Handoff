---
component: Radio Button
component_slug: radio-button
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Teresa Mendez
date: 2026-06-08
design_system: GGDS
status: draft
---

## Descripción

Componente con estados internos de interacción y selección en Web.

## Tokens

### Estados internos
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

| propiedad | uso |
|---|---|
| border-color | hover/pressed |
| background-color | selected/hover selected |
| transform | pressed |
| opacity | focus ring / indicador |

## Triggers

- hover: pointer enter/leave
- pressed: pointer down/up
- focus: focus-visible
- selected: toggle control
- disabled: estado programático sin transición

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transición

## Referencias

- tokens_file: references/tokens.md
