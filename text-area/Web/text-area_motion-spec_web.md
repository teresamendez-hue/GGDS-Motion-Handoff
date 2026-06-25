---
component: Text Area
component_slug: text-area
platform: web
category: default
semantic_token_enter: motion-curve-md
semantic_token_exit: null
owner: Teresa Mendez
date: 2026-06-22
design_system: GGDS
status: draft
---

## Descripcion

Campo de texto multilinea para ingreso de contenido largo. Solo modela transiciones internas de estado y no usa animaciones de viewport.

## Tokens

### Entrada
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Salida
- semantic: no_aplica
- primitive_curve: no_aplica
- css_value: no_aplica
- primitive_duration: no_aplica
- duration_ms: null

## Propiedades animadas

### TextAreaContainer — Estados internos
| propiedad | de | a | token |
|---|---|---|---|
| border-color | neutral | hover/focus/error | motion-curve-md |
| opacity | 1 | 0.45 (disabled) | none |

### TextAreaRing
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| border-color | neutral | focus/error | motion-curve-md |

## Triggers

### Apertura
- evento: no_aplica
- tipo: no_aplica
- delay_ms: null

### Cierre
- evento: no_aplica
- tipo: no_aplica
- auto_delay_ms: null

## Estados intermedios

### Hovered
- trigger: pointer enter
- propiedades: border-color
- token: motion-curve-md

### Focused
- trigger: focus-visible
- propiedades: ring opacity, ring border-color
- token: motion-curve-md

### Typing
- trigger: input change
- propiedades: border-color settle
- token: motion-curve-md

### Filled
- trigger: value not empty
- propiedades: state readout settle
- token: motion-curve-md

### Error
- trigger: validation fail
- propiedades: ring border-color, helper opacity
- token: motion-curve-md

### Disabled
- trigger: disabled true
- propiedades: opacity
- token: none

## Coherencia sistemica

- componentes_relacionados: [Field Text, Field Prefix]
- token_mismo_que: [Field Text, Field Prefix]
- razon: mismo tipo de control de texto en la familia fields y mismo ritmo de interaccion.

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transicion"
- flutter_disable_animations: "no aplica en web"

## Referencias

- material_design: https://m3.material.io/components/text-fields/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/text-fields
- tokens_file: "references/tokens-motion.md"
