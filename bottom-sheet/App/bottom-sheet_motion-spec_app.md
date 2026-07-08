---
component: Bottom Sheet
component_slug: bottom-sheet
platform: app
category: guide
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Teresa Mendez
date: 2026-06-23
design_system: GGDS
status: draft
preview_type: viewport
spring_token: motion-spring-md
companion_web_handoff: null
web_parity_token: null
haptic_semantic_default: none
haptic_override_allowed: false
haptic_trigger: none
---

## Descripcion

Panel inferior en App que entra desde abajo con scrim. Altura segun contenido o variante con scroll interno cuando el contenido alcanza ~80% del viewport. Cierre programatico o por swipe down.

## Tokens

### App
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: "Mismo token en entrada y salida"

## Propiedades animadas

### BottomSheetPanel — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| translateY | 100% | 0% | motion-spring-md |
| opacity | 0 | 1 | motion-spring-md |

### BottomSheetScrim — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |

### BottomSheetPanel — Salida
| propiedad | de | a | token |
|---|---|---|---|
| translateY | 0% | 100% | motion-spring-md |
| opacity | 1 | 0 | motion-spring-md |

### BottomSheetScrim — Salida
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-spring-md |

### BottomSheetPanel — Swipe drag
| propiedad | de | a | token |
|---|---|---|---|
| translateY | posicion actual | offset dedo | none |

### BottomSheetPanel — Swipe release
| propiedad | de | a | token |
|---|---|---|---|
| translateY | offset | 0% o 100% | motion-spring-md |

## Triggers

### Apertura
- evento: user action abre sheet (ej. tap en country trigger)
- tipo: user-action
- delay_ms: 0

### Cierre
- evento: scrim tap, accion primaria, seleccion, swipe dismiss
- tipo: user-action
- auto_delay_ms: null

## Estados intermedios

### Swipe dragging
- trigger: pointer down + move en handle/header
- propiedades: translateY sigue dedo
- token: none

### Scroll variant (~80% viewport)
- trigger: contenido supera umbral de altura (~80% viewport)
- propiedades: max-height del panel, overflow del body
- token: none (layout; motion del sheet sin cambios)

## Coherencia sistemica

- componentes_relacionados: [Field Phone Number, Bottom Sheet consumers]
- token_mismo_que: [Drawer navigation patterns]
- razon: categoria guia para overlays de navegacion/seleccion en App.

## Accesibilidad

- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- material_design: https://m3.material.io/components/bottom-sheets/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/sheets
- tokens_file: tokens-motion.md
- related_handoffs: [Field Phone Number, Dropdown Web — pendiente]
