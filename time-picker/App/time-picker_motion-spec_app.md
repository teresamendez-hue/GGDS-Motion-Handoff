---
component: Time Picker
component_slug: time-picker
platform: app
category: default
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Teresa Mendez
date: 2026-07-06
design_system: GGDS
status: draft
preview_type: field_and_sheet
spring_token: motion-spring-md
companion_web_handoff: ../Web/time-picker_motion-handoff_web.html
web_parity_token: motion-curve-md
time_format: 12h_am_pm
slot_interval_minutes: 15
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onOptionItemTap
---

## Descripcion

Field de hora App que abre Bottom Sheet con lista de slots cada 15 min como Option Items. Seleccion en un solo tap cierra el sheet. Smart default redondea al proximo bloque de 15 min.

## Tokens

### Field trigger
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: Mismo token en entrada y salida

### Bottom Sheet (enlazado)
- semantic: motion-spring-md
- handoff: bottom-sheet_motion-handoff_app.md

### Option Item (enlazado)
- semantic: motion-spring-sm
- handoff: option-item_motion-handoff_app.md

## Propiedades animadas

### TimePickerField
| propiedad | de | a | token |
|---|---|---|---|
| ring opacity | 0 | 1 | motion-spring-md |
| borderColor | neutral | focus/error | motion-spring-md |
| opacity (pressed) | 1 | pressed | motion-spring-md |
| opacity (disabled) | 1 | 0.45 | none |

### Valor confirmado
| propiedad | de | a | token |
|---|---|---|---|
| text content | placeholder / previo | hora formateada | none |

### Auto-scroll lista
| propiedad | de | a | token |
|---|---|---|---|
| scrollOffset | 0 | target | none |

## Triggers

### Sheet open
- evento: tap en field (Enabled)
- tipo: user-action
- delay_ms: 0

### Smart default + scroll
- evento: sheet enter complete, input vacio
- tipo: programmatic
- efecto: jumpTo slot redondeado sin animacion

### Option selected
- evento: tap en Option Item
- tipo: user-action
- efecto: persiste hora, cierra sheet

### Dismiss
- evento: backdrop, swipe down, Atras sistema
- tipo: user-action
- efecto: descarta; input sin cambios

## Estados intermedios

### Focused
- trigger: focus en field
- propiedades: ring opacity, ring borderColor
- token: motion-spring-md

### Pressed
- trigger: tap down en field
- propiedades: opacity
- token: motion-spring-md

### Filled
- trigger: hora seleccionada
- propiedades: none en field colapsado
- token: none

## Coherencia sistemica

- componentes_relacionados: [Select App, Date Picker App, Bottom Sheet, Option Item]
- token_mismo_que: [Select App field, Date Picker App field]
- razon: misma familia fields App con overlay; seleccion directa como Select.

## Accesibilidad

- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- figma: Hand Off Time Picker
- tokens_file: tokens-motion.md
- related_handoffs: [Bottom Sheet App, Option Item App, Select App]
