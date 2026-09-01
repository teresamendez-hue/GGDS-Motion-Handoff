---
component: Number Picker
component_slug: number-picker
platform: app
category: default
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Nehuen David Benitez
date: 2026-08-26
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-md
companion_web_handoff: null
web_parity_token: null
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onChanged
figma_url: "https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/branch/8Dk0SpxqvWWQZuq9tko3mU/Core---App-Components?node-id=13041-1553"
---

## Descripción

Campo numérico para App con botones de incremento/decremento (44×44px) y entrada directa opcional. Tira de input 110×44px con focus ring externo. Motion limitado a estados internos; sin entrada/salida de viewport.

## Tokens

### Field — focus · error · border
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: "Focus ring, borde del input y helper en validación"

### Action icons · value slide (+/-)
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- note: "Pressed: solo backgroundColor (sin scale), 44×44px, alineado al borde del input. Slide del dígito: crossfade sincronizado opacity + translateY ±6px"

### Sin motion
- semantic: none
- note: "Disabled (estado final instantáneo); tipeo directo sin motion por tecla; skeleton fuera de contrato"

## Propiedades animadas

### Input container — Focus
| propiedad | de | a | token |
|---|---|---|---|
| ring opacity | 0 | 1 | motion-spring-md |
| ring borderColor | — | focus (#0059BF) o error-focus (#D3315F) | motion-spring-md |
| borderColor | input-enabled | focus/error | motion-spring-md |

### Input container — Error
| propiedad | de | a | token |
|---|---|---|---|
| borderColor | input-enabled/focus | error (#D3315F) | motion-spring-md |
| helper color | neutral | error (#D3315F) | motion-spring-md |

### ActionIcon (Decrease / Increase) — Pressed
| propiedad | de | a | token |
|---|---|---|---|
| backgroundColor | action-3-enabled / transparent | action-3-pressed | motion-spring-sm |
| size | 44×44px | 44×44px (sin scale) | — |

### Value display — Increment / decrement (solo botones)
| propiedad | de | a | token |
|---|---|---|---|
| opacity (outgoing) | 1 | 0 | motion-spring-sm |
| translateY (outgoing) | 0 | -6px (increment) / +6px (decrement) | motion-spring-sm |
| opacity (incoming) | 0 | 1 | motion-spring-sm |
| translateY (incoming) | +6px (increment) / -6px (decrement) | 0 | motion-spring-sm |
| nota | progreso único t 0→1 en un solo SpringSimulation | — | — |

### ActionIcon (Decrease / Increase) — Disabled (min/max)
| propiedad | de | a | token |
|---|---|---|---|
| color | on-input (#252529) | on-disabled (#BDBDC7) | none |
| backgroundColor | transparent | transparent (sin cambio) | none |

### Root — Disabled
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | estado disabled del DS | none |

## Triggers

### Focus gained
- evento: tap en área de valor / focus programático
- tipo: user-action
- delay_ms: 0

### Increment / decrement
- evento: tap en botón + o − (valor dentro de min/max)
- tipo: user-action
- delay_ms: 0

### Direct typing
- evento: edición de texto en el valor
- tipo: user-action
- delay_ms: 0
- motion: none

### Validation failed
- evento: prop error true / validación fallida
- tipo: programmatic
- delay_ms: 0

### Disabled
- evento: prop state Disabled o enabled false
- tipo: programmatic
- delay_ms: 0

### Min / max reached
- evento: valor en límite; botón opuesto deshabilitado
- tipo: programmatic
- delay_ms: 0
- motion: none
- visual: solo `color` del ícono a on-disabled; sin `backgroundColor` en el botón

## Estados intermedios

### Focused
- trigger: focus gained
- propiedades: ring opacity, ring borderColor
- token: motion-spring-md

### Error
- trigger: validation fail
- propiedades: borderColor, helper color, ring color si focused
- token: motion-spring-md

### Action pressed
- trigger: onTapDown en ActionIcon habilitado (44×44px)
- propiedades: backgroundColor únicamente; sin scale
- token: motion-spring-sm

### Value change (buttons)
- trigger: onChanged vía +/−
- propiedades: crossfade sincronizado outgoing/incoming (opacity + translateY ±6px)
- token: motion-spring-sm

## Haptics

- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onChanged exitoso vía botones +/−
- override_allowed: false
- nota: "Sin haptic en tipeo directo ni en tap de botón deshabilitado (min/max)"

## Skeleton

- incluido_en_handoff: false
- razon: "Shimmer de Figma no replicable fielmente en preview HTML; sin contrato de motion"

## Coherencia sistémica

- componentes_relacionados: [Field Text, Field Prefix, Icon Button, Switch, Box Selector]
- token_mismo_que: [Field Text/Field Prefix — motion-spring-md para focus/error; Icon Button — motion-spring-sm para pressed; Switch/Box Selector — haptic-selection-change]
- razon: "Campo de formulario 110×44px; pressed +/- sin scale al borde; focus/error con motion-spring-md; slide de valor sincronizado con motion-spring-sm"

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- material_design: "https://m3.material.io/components/text-fields/overview"
- apple_hig: "https://developer.apple.com/design/human-interface-guidelines/steppers"
- tokens_file: "references/tokens-motion.md"
- figma_node: "13041:1553"
