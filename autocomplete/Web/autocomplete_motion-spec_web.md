---
component: Autocomplete
component_slug: autocomplete
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

Field de texto con sugerencias en tiempo real y dropdown integrado. Estados internos del field y apertura/cierre del panel de resultados. Panel abre al primer caracter. Contenido del panel: Option Items. Web only; equivalente App: Combobox.

## Tokens

### Entrada / estados field
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

## Propiedades animadas

### AutocompleteWrapper
| propiedad | de | a | token |
|---|---|---|---|
| border-color | neutral | hover/focus/error | motion-curve-md |
| opacity | 1 | 0.45 (disabled) | none |

### AutocompleteRing
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| border-color | neutral | focus/error | motion-curve-md |

### DropdownPanel — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| translateY | -6px | 0 | motion-curve-md |

### DropdownPanel — Salida
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0 | -6px | motion-exit |

## Triggers

### Panel open
- evento: primer caracter en input
- tipo: user-action
- delay_ms: 0

### Panel close
- evento: seleccion de opcion, Escape, click fuera, blur, input vacio
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

### Typing
- trigger: input con texto
- propiedades: border-color
- token: motion-curve-md

### Filled
- trigger: valor persistido en input (seleccion o entrada libre)
- propiedades: none
- token: none

### Error
- trigger: validation fail
- propiedades: ring border-color, helper color
- token: motion-curve-md

### Disabled
- trigger: disabled true
- propiedades: opacity
- token: none

## Coherencia sistemica

- componentes_relacionados: [Field Text, Field Phone Number, Multiple Select, Option Item]
- token_mismo_que: [Field Text, Field Phone Number, Multiple Select]
- razon: misma familia fields con panel; par curve-md / motion-exit; sin chevron.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transicion

## Referencias

- material_design: https://m3.material.io/components/autocomplete/overview
- apple_hig: no aplica (componente Web)
- tokens_file: tokens-motion.md
- related_handoffs: [Option Item]
