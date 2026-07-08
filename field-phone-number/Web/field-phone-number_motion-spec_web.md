---
component: Field Phone Number
component_slug: field-phone-number
platform: web
category: default
semantic_token_enter: motion-curve-md
semantic_token_exit: motion-exit
owner: Teresa Mendez
date: 2026-06-23
design_system: GGDS
status: draft
---

## Descripcion

Campo compuesto para numero telefonico con selector de pais. Estados internos del field y apertura/cierre del dropdown integrado del prefix. Contenido del panel: Option Items (handoff aparte).

## Tokens

### Entrada / estados field
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Salida panel prefix
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-200
- duration_ms: 200

## Propiedades animadas

### FieldPhoneNumberWrapper
| propiedad | de | a | token |
|---|---|---|---|
| border-color | neutral | hover/focus/error | motion-curve-md |
| opacity | 1 | 0.45 (disabled) | none |

### FieldPhoneNumberRing
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| border-color | neutral | focus/error | motion-curve-md |

### CountryTrigger
| propiedad | de | a | token |
|---|---|---|---|
| opacity | default | pressed | motion-curve-md |
| color | muted | active | motion-curve-md |

### PrefixPanel — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| translateY | -6px | 0 | motion-curve-md |

### PrefixPanel — Salida
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0 | -6px | motion-exit |

## Triggers

### Prefix panel open
- evento: click en country trigger
- tipo: user-action
- delay_ms: 0

### Prefix panel close
- evento: click en trigger, Escape, focus en input numerico
- tipo: user-action
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

### Country trigger pressed
- trigger: pointer down on country selector
- propiedades: opacity
- token: motion-curve-md

### Prefix active
- trigger: focus en country o panel abierto
- propiedades: color del trigger
- token: motion-curve-md

### Typing
- trigger: input change
- propiedades: border-color settle
- token: motion-curve-md

### Error
- trigger: validation fail
- propiedades: ring border-color, helper color
- token: motion-curve-md

### Disabled
- trigger: disabled true
- propiedades: opacity
- token: none

## Coherencia sistemica

- componentes_relacionados: [Field Prefix, Field Text, Option Item]
- token_mismo_que: [Field Prefix, Field Text]
- razon: misma familia fields; panel del prefix usa par curve-md / motion-exit.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion

## Referencias

- material_design: https://m3.material.io/components/text-fields/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/text-fields
- tokens_file: tokens-motion.md
- related_handoffs: [Option Item]
