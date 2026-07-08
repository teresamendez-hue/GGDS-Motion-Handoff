---
component: Calendar
component_slug: calendar
platform: app
category: default
semantic_token_month: motion-spring-md
semantic_token_day: motion-spring-sm
owner: Teresa Mendez
date: 2026-07-03
design_system: GGDS
status: draft
preview_type: grid
spring_token: motion-spring-md
companion_date_picker: ../date-picker/App/date-picker_motion-handoff_app.html
views: [month, year]
---

## Descripcion

Grid de fechas con header mes/año, navegacion y Day Item atomico. Vive en Bottom Sheet de Date Picker.

## Tokens

### Month transition
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28

### Day Item
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

### MonthGrid
| propiedad | de | a | token |
|---|---|---|---|
| translateX | offset | 0 | motion-spring-md |
| opacity | 0.6 | 1 | motion-spring-md |

### DayItem
| propiedad | de | a | token |
|---|---|---|---|
| backgroundColor | default | pressed/active | motion-spring-sm |

### RangeFill
| propiedad | de | a | token |
|---|---|---|---|
| backgroundColor | none | middle | none |

## Triggers

### Prev / next month
- evento: tap Icon Button header
- tipo: user-action
- delay_ms: 0

### Day select
- evento: tap celda habilitada
- tipo: user-action
- haptic: haptic-selection-change

### Range second bound
- evento: segundo tap en rango
- tipo: user-action
- fill: instantaneo

## Estados intermedios

### Present (hoy)
- marca visual sin motion extra

### Active borrador
- dia elegido antes de Aceptar en Date Picker

### Cross-month range
- estado persistido al paginar meses

## Coherencia sistemica

- Icon Button header: handoff icon-button
- Day Item atomo interno, no Option Item

## Accesibilidad

- disableAnimations: salto a estado final en mes y celda

## Referencias

- date-picker_motion-handoff_app.md
- icon-button_motion-handoff_app.md
