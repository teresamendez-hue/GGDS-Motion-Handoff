# Motion Handoff — Link (App)

| | |
|---|---|
| **Componente** | Link |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-17 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./link_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/link_motion-handoff_web.html) | Misma intención de motion. En App implementar con tokens spring, no curvas CSS. |

> El preview Web muestra cómo debe sentirse el componente.
> El preview App y el Token Mapping muestran cómo implementarlo en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física | `easing-standard` · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Entrada / salida (viewport) | no aplica | no aplica |
| Estados internos | hover, pressed, focus, visited | pressed, focus, visited |
| Disabled | sin transición | instantáneo (`disableAnimations`) |

Regla: no traducir ms de Web a duración fija en Flutter. El spring define el asentamiento.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **Preview Web companion:** solo referencia de intención; no copiar CSS.
- **QA final:** validar en dispositivo o simulador con `flutter_animate` / `SpringDescription` del design system.

---

## Motion Specification

Link en App usa motion de estado interno. No aplica trigger de salida.

Estados cubiertos: `Enabled`, `Focused`, `Pressed`, `Disabled`, `Visited`.

`Visited` es estado visual estable sin motion protagonista. `Disabled` es instantáneo.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer down | `.link` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Pressed | `.link` | `opacity`, `translateY` | 1, 0px | 0.85, 1px | `motion-spring-sm` | mass 1.0 · stiffness 400 · damping 35 | 0 | settle |
| 3 | Trigger | Focus visible | `.link` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focused | `.link` | `outline` | hidden | visible | `motion-spring-sm` | mass 1.0 · stiffness 400 · damping 35 | 0 | settle |
| 5 | Trigger | Toggle visited | `.link` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Visited | `.link` | `color` | enabled | visited | none | estable | 0 | 0 |
| 7 | Trigger | Toggle disabled | `.link` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Disabled | `.link` | `opacity` | estado actual | 0.45 | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Token semántico | mass | stiffness | damping | Nota |
|---|---:|---:|---:|---|
| `motion-spring-sm` | 1.0 | 400 | 35 | mismo token para pressed/focus |

---

## Implementación Dart

```dart
final motion = context.motionTokens;
final spring = motion.springSm; // motion-spring-sm
final disableAnimations = MediaQuery.of(context).disableAnimations;

// visited: estado visual estable (sin motion protagonista)
// disabled: estado final instantáneo
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo motion de estado interno |
| Hover | no aplica en App |
| Visited | estado visual estable |
| Disabled | instantáneo |
| A11y | `MediaQuery.of(context).disableAnimations` |
