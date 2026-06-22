# Motion Handoff — Quick Action (App)

| | |
|---|---|
| **Componente** | Quick Action |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-17 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./quick-action_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/quick-action_motion-handoff_web.html) | Misma intención de motion. En App implementar con tokens spring, no curvas CSS. |

> El preview Web muestra cómo debe sentirse el componente.
> El preview App y el Token Mapping muestran cómo implementarlo en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física | `easing-standard` · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Entrada / salida (viewport) | no aplica | no aplica |
| Estados internos | hover, pressed, focus, skeleton | pressed, focus, skeleton |
| Disabled | sin transición | instantáneo (`disableAnimations`) |

Regla: no traducir ms de Web a duración fija en Flutter. El spring define el asentamiento.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **Preview Web companion:** solo referencia de intención; no copiar CSS.
- **QA final:** validar en dispositivo o simulador con `flutter_animate` / `SpringDescription` del design system.

---

## Motion Specification

Quick Action en App usa motion de estado interno y no tiene trigger de salida de viewport.

Estados cubiertos: `Enabled`, `Focused`, `Pressed`, `Disabled`, `Skeleton`. No se modela `hover` en App.

Todos los estados animados comparten `motion-spring-sm`. En `Disabled` se aplica el estado final en forma instantánea.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Pointer down | `.quick-action` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Pressed | `.quick-action` | `scale`, `opacity` | `1`, `1` | `0.985`, `0.92` | `motion-spring-sm` | mass 1.0 · stiffness 400 · damping 35 | 0 | settle |
| 3 | Trigger | Focus visible | `.quick-action` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Focused | `.quick-action` | `outline` | 0 | visible | `motion-spring-sm` | mass 1.0 · stiffness 400 · damping 35 | 0 | settle |
| 5 | Trigger | Toggle skeleton | `.quick-action` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Skeleton | `.quick-action` | `surface` | normal | shimmer | `motion-spring-sm` | mass 1.0 · stiffness 400 · damping 35 | 0 | settle |
| 7 | Trigger | Toggle disabled | `.quick-action` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Disabled | `.quick-action` | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Token semántico | mass | stiffness | damping | Nota |
|---|---:|---:|---:|---|
| `motion-spring-sm` | 1.0 | 400 | 35 | mismo token para todos los estados internos |

---

## Implementación Dart

```dart
const motionSpringSm = SpringDescription(
  mass: 1.0,
  stiffness: 400,
  damping: 35,
);

Widget buildQuickAction(BuildContext context) {
  final disableAnimations = MediaQuery.of(context).disableAnimations;
  // no hover en App; usar same spring para pressed/focus/skeleton
  // disabled aplica estado final instantáneo
  return const SizedBox.shrink();
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo motion de estado interno |
| Hover | no aplica en App |
| Disabled | instantáneo |
| A11y | `MediaQuery.of(context).disableAnimations` |
| Coherencia | mismo patrón de `button` e `icon-button` App |
