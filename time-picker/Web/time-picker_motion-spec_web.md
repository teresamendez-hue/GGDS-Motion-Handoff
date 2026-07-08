---
component: Time Picker
component_slug: time-picker
platform: web
category: default
semantic_token_enter: motion-curve-md
semantic_token_exit: motion-exit
semantic_token_row: motion-curve-sm
owner: Teresa Mendez
date: 2026-07-06
design_system: GGDS
status: draft
preview_type: field_and_popover
companion_app: ../App/time-picker_motion-handoff_app.html
time_format: 12h_am_pm
slot_interval_minutes: 15
smart_default: round_next_15_visible_in_field
footer_confirm: false
flip_panel: true
---

## Descripcion

Field de hora Web con popover integrado y lista de slots cada 15 min como Option Items. Seleccion en un solo clic cierra el panel. Smart default redondea al proximo bloque de 15 min y se muestra en el field desde el inicio.

## Tokens

### Entrada / estados field / panel enter
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Salida panel
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-200
- duration_ms: 200

### Option Item (enlazado)
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150
- handoff: option-item_motion-handoff_web.md

## Propiedades animadas

### TimePickerField
| propiedad | de | a | token |
|---|---|---|---|
| border-color | neutral | hover/focus/error | motion-curve-md |
| ring opacity | 0 | 1 | motion-curve-md |
| opacity (pressed) | 1 | pressed | motion-curve-md |
| opacity (disabled) | 1 | 0.45 | none |

### PopoverPanel — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| translateY | ±6px | 0 | motion-curve-md |

### PopoverPanel — Salida
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0 | ±6px | motion-exit |

### Option Item fila
| propiedad | de | a | token |
|---|---|---|---|
| background-color | default | hover/pressed/selected | motion-curve-sm |

### Valor en field
| propiedad | de | a | token |
|---|---|---|---|
| text content | smart default / previo | hora elegida | none |

### Auto-scroll lista
| propiedad | de | a | token |
|---|---|---|---|
| scrollTop | offset | target | none (instantaneo) |

## Triggers

### Panel open
- evento: focus o click en field (Enabled)
- tipo: user-action
- delay_ms: 0

### Smart default + scroll
- evento: panel open
- tipo: programmatic
- efecto: scroll instantaneo al smart default o valor confirmado

### Option selected
- evento: click en Option Item
- tipo: user-action
- efecto: persiste hora, cierra panel con motion-exit

### Panel close
- evento: seleccion, Escape, click fuera
- tipo: user-action
- token_salida: motion-exit

### Flip panel
- evento: deteccion de colision (sin espacio abajo)
- tipo: programmatic
- efecto: ancla arriba del field; misma curva enter/exit

## Estados intermedios

### Hovered (field)
- trigger: pointer enter
- propiedades: border-color
- token: motion-curve-md

### Smart default visible
- trigger: mount / sin confirmacion previa
- propiedades: valor redondeado en field
- token: none

### Filled
- trigger: hora confirmada por usuario
- propiedades: valor persistido
- token: none

## Coherencia sistemica

- componentes_relacionados: [Select Web, Date Picker Web, Option Item Web, Time Picker App]
- token_mismo_que: [Select Web — field/panel enter-exit]
- razon: misma familia fields con overlay integrado; par curve-md / motion-exit.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion
- focus_visible: ring en field y Option Items

## Referencias

- figma: Hand Off Time Picker Web
- tokens_file: tokens-motion.md
- related_handoffs: [Select Web, Option Item Web, Time Picker App]
