# Motion Handoff — Tabs (App)

| | |
|---|---|
| **Componente** | Tabs |
| **Plataforma** | App (Flutter) |
| **Owner** | Ezequiel Arguello |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` (estado) · `motion-spring-md` (indicador) |
| **Categoría** | Default (estado) / Guía (indicador) |
| **Variantes** | Segmented Item · Line Item |
| **Fecha** | 2026-07-02 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./tabs_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/tabs_motion-handoff_web.html) | Misma intención de motion. En App implementar con tokens spring, no curvas CSS. |

> El preview Web muestra cómo debe sentirse el componente.
> El preview App y el Token Mapping muestran cómo implementarlo en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico — estado | `motion-curve-sm` | `motion-spring-sm` |
| Token semántico — indicador | `motion-curve-md` | `motion-spring-md` |
| Curva / física — estado | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Curva / física — indicador | easing-decelerate · 300ms | mass 1.0 · stiffness 300 · damping 28 |
| Estados internos | hovered, pressed, focused | pressed, focused (sin hover) |
| Selección | indicador via `motion-exit`-free CSS transition | mismo spring en ida y vuelta |
| Disabled | sin transición | instantáneo (`disableAnimations`) |

Regla: no traducir ms de Web a duración fija en Flutter — el spring define el asentamiento en ambos tokens.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos a los tokens.
- **Preview Web companion:** solo referencia de intención; no copiar CSS.
- **QA final:** validar en dispositivo con `flutter_animate` / `SpringDescription` del design system, incluyendo el gesto de swipe entre tabs si el TabBarView lo soporta.

---

## Motion Specification

Tabs en App usa motion de estados internos, sin enter/exit de viewport: Enabled, Pressed, Focused, Selected, Disabled (no hay `hover` en App). Aplica igual a Segmented Item y Line Item.

`Pressed` y `Focused` usan `motion-spring-sm`, igual que Button, para coherencia entre controles interactivos frecuentes.

El cambio de `Selected` mueve el indicador de posición (fondo relleno en Segmented, línea inferior en Line) con `motion-spring-md`, por tratarse de un desplazamiento espacial entre tabs — mismo criterio que en Web pero resuelto con spring en vez de curva. Entrada y salida del indicador usan el mismo token spring.

`Disabled` es instantáneo. Con `MediaQuery.disableAnimations` se aplica el estado final sin animar, incluido el indicador.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tap down | Tab item | — | — | — | — |
| 2 | Response | Pressed | Tab item | scale / color overlay | 1.0 / enabled | 0.98 / pressed | motion-spring-sm |
| 3 | Trigger | Focus visible (teclado externo / TalkBack) | Tab item | — | — | — | — |
| 4 | Response | Focus ring | Focus ring | opacity | 0.0 | 1.0 | motion-spring-sm |
| 5 | Trigger | Cambio de tab seleccionado | TabBar | — | — | — | — |
| 6 | Response | Indicador (Segmented/Line) | Indicator | position / size | posición anterior | tab activo | motion-spring-md |
| 7 | Response | Label/ícono | Tab item | color / fontWeight | enabled ↔ selected | — | motion-spring-sm |
| 8 | Trigger | Disabled | Tab item | — | — | — | — |
| 9 | Response | Disabled | Tab item | — | final | — | instantáneo |

---

## Token Mapping

| Token semántico | mass | stiffness | damping | Uso |
|---|---|---|---|---|
| `motion-spring-sm` | 1.0 | 400 | 35 | Pressed, Focused, fade de color |
| `motion-spring-md` | 1.0 | 300 | 28 | Movimiento del indicador de selección |

---

## Implementación Dart (Flutter)

```dart
// Consumir tokens semánticos desde el sistema de motion tokens
final motion = context.motionTokens;
final stateSpring = motion.springSm;      // motion-spring-sm
final indicatorSpring = motion.springMd;  // motion-spring-md
final disableAnimations = MediaQuery.of(context).disableAnimations;

void onTabSelected(int index) {
  if (disableAnimations) {
    indicatorController.value = targetOffsetFor(index);
    return;
  }

  indicatorController.animateWith(
    SpringSimulation(
      indicatorSpring, // motion-spring-md
      indicatorController.value,
      targetOffsetFor(index),
      0,
    ),
  );
}

// pressed/focus usan stateSpring (motion-spring-sm) sobre scale/opacity
// disabled: estado final instantáneo, sin animar
```

---

## Haptics Specification

Tabs dispara haptic en el cambio de selección usando `haptic-selection-change`, igual que otros controles de selección del sistema (Switch, Checkbox, Box Selector).

| Campo | Valor |
|---|---|
| **Token default** | `haptic-selection-change` |
| **Trigger** | `onChanged` (cambio de tab activo) |
| **Override** | no recomendado |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |

---

## Implementación Haptics

```dart
void onTabChanged(int index) {
  context.haptics.trigger(GgdsHapticSemantic.selectionChange); // haptic-selection-change
  onTabSelected(index);
}
```

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token de estado | `motion-spring-sm` para Pressed/Focused, coherente con Button |
| Token de indicador | `motion-spring-md` por ser desplazamiento espacial |
| Hover | No modelar — no existe en App |
| Disabled | Instantáneo (`disableAnimations`) |
| Haptics | `haptic-selection-change` en cada cambio de tab, sin override |
| A11y | Respetar `MediaQuery.disableAnimations` en estado y en indicador |
