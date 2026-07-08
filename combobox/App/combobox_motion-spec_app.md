---
component: Combobox
component_slug: combobox
platform: app
category: default
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Teresa Mendez
date: 2026-06-23
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-md
companion_web_handoff: autocomplete_motion-handoff_web.html
web_parity_token: motion-curve-md
---

## Descripcion

Field de seleccion unica con busqueda para App. Tap en el field colapsado abre Bottom Sheet con input focused y teclado. Filtrado desde el 1er caracter. Valor persiste en el field. Motion del field colapsado en este handoff; sheet y filas en handoffs enlazados.

## Tokens

### Field colapsado
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: "Mismo token en entrada y salida de estados del field"

### Bottom Sheet (referencia)
- semantic: motion-spring-md
- note: "Ver handoff Bottom Sheet"

## Propiedades animadas

### ComboboxFieldTrigger
| propiedad | de | a | token |
|---|---|---|---|
| opacity | default | pressed | motion-spring-md |
| opacity | 1 | 0.45 (disabled) | none |

### ComboboxRing
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |
| borderColor | neutral | focus/error | motion-spring-md |

## Triggers

### Sheet open
- evento: tap en field colapsado
- tipo: user-action
- delay_ms: 0

### Sheet close
- evento: seleccion de Option Item, dismiss del sheet
- tipo: user-action
- auto_delay_ms: null

### Filtrado
- evento: primer caracter en input del sheet
- tipo: user-action
- delay_ms: 0

## Estados intermedios

### Focused
- trigger: focus visible en field colapsado
- propiedades: ring opacity, ring borderColor
- token: motion-spring-md

### Pressed
- trigger: tap down en field
- propiedades: opacity
- token: motion-spring-md

### Filled
- trigger: opcion seleccionada y sheet cerrado
- propiedades: none en field colapsado
- token: none

### Error
- trigger: validation fail
- propiedades: ring borderColor, helper color
- token: motion-spring-md

### Disabled
- trigger: disabled true
- propiedades: opacity
- token: none

## Coherencia sistemica

- componentes_relacionados: [Autocomplete, Bottom Sheet, Option Item, Multiple Select, Field Phone Number]
- token_mismo_que: [Multiple Select, Field Phone Number, Bottom Sheet]
- razon: misma familia fields App; sheet reutiliza motion-spring-md del sistema.

## Accesibilidad

- prefers_reduced_motion: no aplica en App
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- material_design: https://m3.material.io/components/menus/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/pickers
- tokens_file: tokens-motion.md
- related_handoffs: [Autocomplete Web, Bottom Sheet, Option Item]
