---
component: Date Picker
component_slug: date-picker
platform: app
category: default
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Teresa Mendez
date: 2026-07-03
design_system: GGDS
status: draft
preview_type: field_and_sheet
spring_token: motion-spring-md
companion_calendar: ../calendar/App/calendar_motion-handoff_app.html
companion_bottom_sheet: ../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html
variants: [single, range]
---

## Descripcion

Field de fecha App que abre Bottom Sheet con Calendar y footer Borrar/Aceptar. Seleccion en borrador hasta confirmar.

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

## Propiedades animadas

### DatePickerField
| propiedad | de | a | token |
|---|---|---|---|
| ring opacity | 0 | 1 | motion-spring-md |
| borderColor | neutral | focus/error | motion-spring-md |
| opacity (pressed) | 1 | pressed | motion-spring-md |
| opacity (disabled) | 1 | 0.45 | none |

### Valor confirmado
| propiedad | de | a | token |
|---|---|---|---|
| text content | placeholder / previo | fecha Aceptar | none |

## Triggers

### Sheet open
- evento: tap en field (Enabled)
- tipo: user-action
- delay_ms: 0

### Draft day tap
- evento: tap en Day Item dentro del Calendar
- tipo: user-action
- motion: ver calendar_motion-spec_app.md

### Aceptar
- evento: onPressed footer primario
- tipo: user-action
- efecto: persiste borrador, cierra sheet

### Borrar
- evento: onPressed footer secundario
- tipo: user-action
- efecto: limpia borrador y field

### Dismiss
- evento: backdrop tap, Atras sistema
- tipo: user-action
- efecto: descarta borrador, input sin cambios

## Estados intermedios

### Borrador activo
- sheet abierto, dia(s) en borrador, input aun sin commit

### Range incompleto
- Aceptar deshabilitado hasta inicio + fin

## Coherencia sistemica

- Field alineado con Select/Combobox App (motion-spring-md)
- Sheet reutiliza Bottom Sheet handoff
- Confirmacion explicita vs Web auto-close

## Accesibilidad

- disableAnimations: estados finales sin spring
- Semantica de confirmacion en footer

## Referencias

- calendar_motion-handoff_app.md
- bottom-sheet_motion-handoff_app.md
- button_motion-handoff_app.md
