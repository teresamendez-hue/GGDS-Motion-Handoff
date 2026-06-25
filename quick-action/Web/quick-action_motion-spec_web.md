---
component: Quick Action
component_slug: quick-action
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

Componente de acceso rápido a acciones frecuentes dentro de una pantalla.
Usa motion solo para cambios de estado internos.

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

### QuickAction — Estados
| propiedad | de | a | token |
|---|---|---|---|
| background-color | default | hover | motion-curve-sm |
| border-color | default | hover | motion-curve-sm |
| transform | scale(1) | scale(0.985) | motion-curve-sm |
| opacity | 1 | 0.92 (pressed) | motion-curve-sm |
| outline-opacity | 0 | 1 (focus) | motion-curve-sm |

## Triggers

### Apertura
- evento: no aplica (componente persistente)
- tipo: null
- delay_ms: 0

### Cierre
- evento: no aplica
- tipo: null
- auto_delay_ms: null

## Estados intermedios

### State
- trigger: interacción interna de puntero/teclado
- propiedades: hover, pressed, focused, disabled, skeleton
- token: motion-curve-sm

## Coherencia sistémica

- componentes_relacionados: [Button, Icon Button]
- token_mismo_que: [Button, Icon Button]
- razon: mismo tipo de feedback inmediato para control de acción

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transición
- flutter_disable_animations: no aplica

## Referencias

- material_design: https://m3.material.io/components/buttons/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/buttons
- tokens_file: references/tokens-motion.md
