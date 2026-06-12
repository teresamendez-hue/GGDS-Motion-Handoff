# Motion Handoff — Icon Button (App)

| | |
|---|---|
| **Componente** | Icon Button |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-10 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./icon-button_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/icon-button_motion-handoff_web.html) | Misma intención de motion. En App implementar con token spring, no curvas CSS. |

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Estados internos | default, pressed, focus, disabled, loading, skeleton | default, pressed, focus, disabled, loading, skeleton |
| Disabled | sin transición | instantáneo |

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS.
- **Preview Web companion:** referencia de intención, no copiar CSS.
- **QA final:** validar en dispositivo con `flutter_animate` / `SpringDescription`.

---

## Motion Specification

Icon Button en App usa estados internos animados con `motion-spring-sm`.

`disabled` es instantáneo y `disableAnimations` aplica estado final sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Highlight | Icon Button | — | — | — | — |
| 3 | Trigger | Tap down | Icon Button | — | — | — | — |
| 4 | Response | Pressed | Icon Button | scale/color | 1/default | 0.96/pressed | motion-spring-sm |
| 5 | Trigger | Focus visible | Icon Button | — | — | — | — |
| 6 | Response | Focus ring | ring | opacity | 0 | 1 | motion-spring-sm |
| 7 | Trigger | Loading | Icon Button | — | — | — | — |
| 8 | Response | Loading crossfade | icon/spinner | opacity | 1/0 | 0.12/1 | motion-spring-sm |
| 9 | Trigger | Skeleton | Icon Button | — | — | — | — |
| 10 | Response | Skeleton | icon/background | visible/default | hidden/skeleton | motion-spring-sm |
| 11 | Trigger | Disabled | Icon Button | — | — | — | — |
| 12 | Response | Disabled | todas | — | final | — | instantáneo |

---

## Token Mapping

| Token semántico | mass | stiffness | damping |
|---|---|---|---|
| `motion-spring-sm` | 1.0 | 400 | 35 |

---

## Implementación Dart

```dart
final motion = context.motionTokens;
final spring = motion.springSm; // motion-spring-sm

// Usar `spring` en pressed/focus/loading/skeleton.
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-spring-sm` para estados internos |
| Tap rebound | rebote corto y sutil al release |
| Disabled | instantáneo |
| A11y | `MediaQuery.disableAnimations` |
| Radius | `4px` (match con Button en Figma) |
