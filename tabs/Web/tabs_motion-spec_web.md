---
component: Tab
component_slug: tabs
platform: web
category: guide
semantic_token_enter: motion-curve-md
semantic_token_exit: null
owner: Ezequiel Arguello
date: 2026-07-08
design_system: GGDS
status: draft
preview_type: internal_state
variants: [line, segmented]
---

## Descripción

Control de navegación por tabs con indicador Line o Segmented. Al seleccionar un tab, el indicador y el panel se desplazan horizontalmente. Lista con scroll horizontal en overflow y reveal del tab activo.

## Tokens

### Indicador y panel
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Tab item (estados interactivos)
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

### Indicador — tab change (Line y Segmented)
| propiedad | de | a | token |
|---|---|---|---|
| transform | posición tab N | posición tab N±1 | motion-curve-md |
| width | ancho tab N | ancho tab N±1 | motion-curve-md |

### Panel — tab change
| propiedad | de | a | token |
|---|---|---|---|
| transform | translateX(panel N) | translateX(panel N±1) | motion-curve-md |

### Tab item — hover
| propiedad | de | a | token |
|---|---|---|---|
| color | default | hover | motion-curve-sm |

### Tab item — pressed
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0.85 | motion-curve-sm |

### Tab item — focus ring
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-sm |

## Triggers

### tab_select
- evento: click o teclado en tab
- tipo: user-action

### scroll_manual
- evento: wheel, trackpad o drag en lista de tabs
- tipo: user-action
- note: scroll nativo sin token; sync indicador en scroll event

### scroll_reveal_active
- evento: tab seleccionado fuera del viewport
- tipo: programmatic
- behavior: smooth
- reduced_motion_behavior: auto

## Estados intermedios

### scroll_overflow
- trigger: tabs exceden ancho del contenedor
- propiedades: overflow-x auto, scroll horizontal
- token: ninguno

### selected
- trigger: tab activo
- propiedades: aria-selected, estilo selected en label
- token: ninguno en label

## Coherencia sistémica

- componentes_relacionados: [Carousel, Sidesheet, Segmented Control]
- token_mismo_que: [Carousel — navegación espacial con motion-curve-md]
- razon: cambio de vista guiado usa Guía; estados de item usan Default como Button

## Accesibilidad

- prefers_reduced_motion: indicador, panel e items sin transición; scroll reveal instantáneo

## Referencias

- material_design: https://m3.material.io/components/tabs
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/tab-bars
- tokens_file: references/tokens-motion.md
