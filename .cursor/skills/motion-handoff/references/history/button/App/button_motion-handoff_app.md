# Motion Handoff — Button (App)

| | |
|---|---|
| **Componente** | Button |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Variantes** | Size S, M |
| **Fecha** | 2026-06-05 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./button_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/button_motion-handoff_web.html) | Misma intención de motion. En App implementar con tokens spring, no curvas CSS. |

> El preview Web muestra cómo debe sentirse el componente.  
> El preview App y el Token Mapping muestran cómo implementarlo en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Estados internos | pressed, focus, loading, skeleton | highlight, pressed, focus, loading, skeleton |
| Disabled | sin transición | instantáneo |

Regla: no traducir ms de Web a duración fija en Flutter.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS.
- **Preview Web companion:** solo referencia de intención; no copiar CSS.
- **QA final:** validar en dispositivo con `flutter_animate` / `SpringDescription`.

---

## Motion Specification

Button en App usa motion de estados internos: `highlight`, `pressed`, `focus`, `loading`, `disabled`, `skeleton`.

Todos los estados animados usan `motion-spring-sm`. `disabled` es instantáneo y reduce motion aplica estado final.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Highlight | Button | — | — | — | — |
| 3 | Trigger | Tap down | Button | — | — | — | — |
| 4 | Response | Pressed | Button | scale/color | 1/default | 0.98/pressed | motion-spring-sm |
| 5 | Trigger | Focus visible | Button | — | — | — | — |
| 6 | Response | Focus ring | Focus ring | opacity | 0 | 1 | motion-spring-sm |
| 7 | Trigger | Loading | Button | — | — | — | — |
| 8 | Response | Loading crossfade | label/spinner | opacity | 1/0 | 0.4/1 | motion-spring-sm |
| 9 | Trigger | Skeleton resolve | Button | — | — | — | — |
| 10 | Response | Skeleton -> content | skeleton/content | opacity | 1/0 | 0/1 | motion-spring-sm |
| 11 | Trigger | Disabled | Button | — | — | — | — |
| 12 | Response | Disabled | todas | — | final | — | instantáneo |

---

## Token Mapping

| Token semántico | mass | stiffness | damping |
|---|---|---|---|
| `motion-spring-sm` | 1.0 | 400 | 35 |

---

## Implementación Dart

```dart
// Consumir token semántico desde el sistema de motion tokens
final motion = context.motionTokens;
final spring = motion.springSm; // motion-spring-sm

// Ejemplo con flutter_animate o controller propio
// Usar `spring` como fuente de verdad para pressed/focus/selection
```

---

## Haptics Specification

Button dispara haptic en press exitoso usando `haptic-action-press`.

El diseñador puede desactivarlo por instancia con override `none` (ej. onboarding intermedio).

| Campo | Valor |
|---|---|
| **Token default** | `haptic-action-press` |
| **Trigger** | `onPressed exitoso` |
| **Override** | `none` \| `default` |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |

---

## Implementación Haptics

```dart
final haptic = widget.haptic ?? GgdsHapticSemantic.actionPress;

void onPressed() {
  if (haptic != GgdsHapticSemantic.none) {
    context.haptics.trigger(haptic); // haptic-action-press
  }
}
```

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-spring-sm` para todos los estados internos |
| Pressed | scale sutil + color |
| Disabled | instantáneo |
| A11y | `MediaQuery.disableAnimations` |
