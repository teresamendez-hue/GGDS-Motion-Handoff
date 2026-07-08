---
component: Search
component_slug: search
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
companion_web_handoff: search_motion-handoff_web.html
web_parity_token: motion-curve-md
interaction_modes: [fullscreen, inpage]
---

## Descripcion

Field de busqueda App con dos modos: fullscreen (modal pantalla completa) e inpage (overlay en sitio). Clear condicional, umbral 3 caracteres, historial con showMenuOnFocus.

## Tokens

### Field / overlay / modal
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: Mismo token en entrada y salida

### Clear button
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- note: Mismo token en entrada y salida

## Propiedades animadas

### SearchField
| propiedad | de | a | token |
|---|---|---|---|
| ring opacity | 0 | 1 | motion-spring-md |
| borderColor | neutral | focus/error | motion-spring-md |
| opacity | 1 | 0.45 (disabled) | none |

### ClearButton
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-sm |

### FullScreenModal
| propiedad | de | a | token |
|---|---|---|---|
| translateY | off-screen | 0 | motion-spring-md |
| opacity | 0 | 1 | motion-spring-md |

### InPageOverlay
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |
| translateY | -8 | 0 | motion-spring-md |

## Triggers

### Fullscreen open
- evento: tap en Search colapsado
- tipo: user-action
- delay_ms: 0

### Fullscreen close
- evento: Atras/Cancelar, seleccion de item
- tipo: user-action
- auto_delay_ms: null

### Inpage overlay open
- evento: tap en Search; >=3 chars o showMenuOnFocus
- tipo: user-action
- delay_ms: 0

### Inpage overlay close
- evento: tap fuera, Back del sistema, seleccion, clear
- tipo: user-action
- auto_delay_ms: null

## Estados intermedios

### Focused
- trigger: tap / focus en input
- propiedades: ring opacity
- token: motion-spring-md

### Typing
- trigger: texto en input
- propiedades: borderColor
- token: motion-spring-md

### Historial visible
- trigger: showMenuOnFocus y input vacio
- propiedades: lista contenido
- token: none

### Resultados activos
- trigger: >=3 caracteres
- propiedades: lista contenido
- token: none

## Coherencia sistemica

- componentes_relacionados: [Combobox, Autocomplete, Bottom Sheet]
- token_mismo_que: [Combobox, Field Text App]
- razon: field guia con motion-spring-md; fullscreen distinto de Bottom Sheet de Combobox.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion
- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- material_design: https://m3.material.io/components/search/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/search-fields
- tokens_file: tokens-motion.md
- related_handoffs: [Combobox]
