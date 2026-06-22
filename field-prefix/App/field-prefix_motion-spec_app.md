---
component: Field Prefix
component_slug: field-prefix
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
companion_web_handoff: field-prefix_motion-handoff_web.html
web_parity_token: motion-curve-md
---

## Descripcion

Campo de entrada con prefijo fijo para datos estructurados en App. El componente modela solo transiciones de estado interno y no animaciones de viewport.

## Tokens

### Entrada
- semantic: motion-spring-md
- primitive_curve: no_aplica
- css_value: no_aplica
- primitive_duration: no_aplica
- duration_ms: null

### Salida
- semantic: motion-spring-md
- primitive_curve: no_aplica
- css_value: no_aplica
- primitive_duration: no_aplica
- duration_ms: null

### App
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: "Mismo token en entrada y salida"

## Propiedades animadas

### FieldPrefixContainer — Estados internos
| propiedad | de | a | token |
|---|---|---|---|
| borderColor | neutral | focus/success/error | motion-spring-md |
| color | neutral | focused/success/error | motion-spring-md |
| opacity | 1 | 0.45 (disabled) | none |

### FieldPrefixRing
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |
| borderColor | neutral | focus/success/error | motion-spring-md |

### FieldPrefixHelper
| propiedad | de | a | token |
|---|---|---|---|
| color | neutral | error | motion-spring-md |

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

### Focused
- trigger: focus gained
- propiedades: ring opacity, ring borderColor
- token: motion-spring-md

### Typing
- trigger: text changed
- propiedades: container borderColor settle
- token: motion-spring-md

### Filled
- trigger: value not empty
- propiedades: helper visibility
- token: motion-spring-md

### Error
- trigger: validation fail
- propiedades: ring/helper color
- token: motion-spring-md

### Disabled
- trigger: disabled true
- propiedades: opacity
- token: none

## Coherencia sistemica

- componentes_relacionados: [Field Text, Field Pin Code]
- token_mismo_que: [Field Text, Field Pin Code]
- razon: misma familia de campos de entrada y mismo criterio de estabilidad para foco/validacion.

## Accesibilidad

- prefers_reduced_motion: "no aplica en app"
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- material_design: https://m3.material.io/components/text-fields/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/text-fields
- tokens_file: "references/tokens.md"
