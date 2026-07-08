---
component: Date Picker
component_slug: date-picker
platform: web
category: default
semantic_token_enter: motion-curve-md
semantic_token_exit: motion-exit
semantic_token_atom: motion-curve-sm
owner: Teresa Mendez
date: 2026-07-06
design_system: GGDS
status: draft
preview_type: field_and_popover
companion_app: ../App/date-picker_motion-handoff_app.html
variants: [single, range]
internal_atoms: [_Datepicker/Day Item]
---

## Descripcion

Field de fecha Web con popover integrado y calendario embebido. Single cierra al elegir dia. Range usa footer Cancelar/Aceptar en el popover.

## Tokens

### Entrada / estados field / mes / year
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Salida popover
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-200
- duration_ms: 200

### Calendar Day Item
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

### DatePickerField
| propiedad | de | a | token |
|---|---|---|---|
| border-color | neutral | hover/focus/error | motion-curve-md |
| ring opacity | 0 | 1 | motion-curve-md |
| opacity (disabled) | 1 | 0.45 | none |

### PopoverPanel — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| translateY | -6px | 0 | motion-curve-md |

### PopoverPanel — Salida
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0 | -6px | motion-exit |

### MonthGrid
| propiedad | de | a | token |
|---|---|---|---|
| translateX | offset | 0 | motion-curve-md |
| opacity | 0.55–1 | 1 | motion-curve-md |

### YearView crossfade
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1/0 | swap | motion-curve-md |

### CalendarDayItem
| propiedad | de | a | token |
|---|---|---|---|
| background-color | default | hover/pressed/active | motion-curve-sm |
| range middle fill | none | middle | none |

## Triggers

### Panel open
- evento: focus o click en field (Enabled)
- tipo: user-action
- delay_ms: 0

### Single day select
- evento: click en Day Item
- tipo: user-action
- efecto: persiste fecha, cierra popover

### Range complete (borrador)
- evento: segundo click en Day Item
- tipo: user-action
- efecto: habilita Aceptar; no cierra panel

### Aceptar (Range)
- evento: click footer primario con inicio + fin
- tipo: user-action
- efecto: persiste rango, cierra popover

### Cancelar (Range)
- evento: click footer secundario
- tipo: user-action
- efecto: limpia borrador en calendario; input sin cambios

### Panel close (Single)
- evento: seleccion de dia, Escape, click fuera
- tipo: user-action
- token_salida: motion-exit

### Dismiss sin aplicar
- evento: Escape / click fuera en Range
- tipo: user-action
- efecto: descarta borrador; input sin cambios

## Estados intermedios

### Hovered (field)
- trigger: pointer enter
- propiedades: border-color
- token: motion-curve-md

### Range parcial
- trigger: primer dia en Range
- propiedades: start active; sin cierre
- token: motion-curve-sm en celda

### Present (hoy)
- trigger: render
- propiedades: dot debajo del numero
- token: none

## Coherencia sistemica

- componentes_relacionados: [Select, Search, Date Picker App, Icon Button]
- token_mismo_que: [Select — field/panel enter-exit]
- razon: misma familia fields con overlay integrado; par curve-md / motion-exit.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion
- focus_visible: ring en field y Day Items

## Referencias

- figma: Hand Off Date Picker (node 13:9332)
- tokens_file: tokens-motion.md
- related_handoffs: [Icon Button Web, Date Picker App, Calendar App]
