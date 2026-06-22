---
component: Breadcrumb
component_slug: breadcrumb
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Teresa Mendez
date: 2026-06-17
design_system: GGDS
status: draft
---

## Descripción

Componente de navegación jerárquica por rutas internas de producto.

## Tokens

### Entrada
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Salida
- semantic: null
- primitive_curve: null
- css_value: null
- primitive_duration: null
- duration_ms: null

## Propiedades animadas

| propiedad | uso |
|---|---|
| color | hovered/active |
| opacity | pressed |
| opacity | focus ring |
| opacity + transform | reveal en collapse -> expanded |

## Triggers

- hovered: pointer enter/leave
- pressed: pointer down/up
- focused: focus-visible
- active: click en item
- type_variant: collapse/expanded
- expand_action: click en `...` (collapse -> expanded)
- exit_trigger: no aplica

## Estados intermedios

### State
- valores: Enabled, Hovered, Pressed, Focused, Active

### Type
- valores: Collapse, Expanded

## Coherencia sistémica

- componentes_relacionados: [link]
- token_mismo_que: [link]
- razon: microinteracción textual de navegación

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transición

## Referencias

- material_design: https://m3.material.io/components/breadcrumbs/overview
- apple_hig: no aplica
- tokens_file: references/tokens.md
