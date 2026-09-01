# Motion Handoff — Box Selector (App)

| | |
|---|---|
| **Componente** | Box Selector |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-08 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./box-selector_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/box-selector_motion-handoff_web.html) | Misma intención de motion. En App implementar con token spring, no curvas CSS. |

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Estados internos | default, pressed, focus, disabled + control checked/unchecked | default, pressed, focus, disabled + control checked/unchecked |
| Disabled | sin transición | instantáneo |

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS.
- **Preview Web companion:** referencia de intención, no copiar CSS.
- **QA final:** validar en dispositivo con `flutter_animate` / `SpringDescription`.

---

## Motion Specification

Box Selector en App usa estados internos animados con `motion-spring-sm`.

`disabled` es instantáneo y `disableAnimations` aplica estado final sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 3 | Trigger | Tap down | componente | — | — | — | — |
| 4 | Response | Pressed | contenedor | scale/color | default | pressed | motion-spring-sm |
| 5 | Trigger | Focus visible | componente | — | — | — | — |
| 6 | Response | Focus ring | ring | opacity | 0 | 1 | motion-spring-sm |
| 7 | Trigger | Toggle selected | control | — | — | — | — |
| 8 | Response | Selected | indicador | opacity/fill | off | on | motion-spring-sm |
| 9 | Trigger | Disabled | componente | — | — | — | — |
| 10 | Response | Disabled | todas | — | final | — | instantáneo |

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

Box Selector dispara haptic en cambio de selección usando `haptic-selection-change`.

| Campo | Valor |
|---|---|
| **Token default** | `haptic-selection-change` |
| **Trigger** | `cambio de estado/selección` |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |

---

## Implementación Haptics

```dart
void onChanged(bool value) {
  context.haptics.trigger(GgdsHapticSemantic.selectionChange); // haptic-selection-change
}
```

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-spring-sm` para estados internos |
| Disabled | instantáneo |
| Focus | visible solo en navegación de teclado |
| A11y | `MediaQuery.disableAnimations` |
