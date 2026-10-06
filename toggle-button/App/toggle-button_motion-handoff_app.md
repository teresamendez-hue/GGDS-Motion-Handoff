# Motion Handoff — Toggle Button (App)

| | |
|---|---|
| **Componente** | Toggle Button |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuen Benitez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-10-06 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./toggle-button_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |

## Paridad Web ↔ App

| Aspecto | Web (referencia sistémica) | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Estados documentados | — (sin handoff Web) | Selected False ↔ True |
| Exit | no aplica | no aplica |

> No hay companion Web para este componente. La fila Web es solo paridad de tokens del sistema.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS.
- **QA final:** validar en dispositivo con `flutter_animate` / `SpringDescription`.

---

## Motion Specification

Toggle Button en App es un control de estado interno: alterna entre dos estados opuestos (`Selected` False / True) con un tap. No tiene entrada ni salida de viewport.

Al pasar a `Selected` True, el ícono outline se reemplaza por el ícono filled de la misma casuística (ej. Favorite en Figma). El crossfade usa `motion-spring-sm`. No hay cambio de color de superficie del control.

`disableAnimations` aplica el estado final sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tap / onChanged | Toggle Button | — | — | — | — |
| 2 | Response | Selected toggle | icon | opacity / glyph | outline (False) | filled (True) | motion-spring-sm |

---

## Token Mapping

| Token semántico | mass | stiffness | damping |
|---|---|---|---|
| `motion-spring-sm` | 1.0 | 400 | 35 |

---

## Implementación Dart

```dart
final motion = context.motionTokens;
final spring = motion.springSm; // motion-spring-sm (mass: 1.0, stiffness: 400, damping: 35)
final disableAnimations = MediaQuery.of(context).disableAnimations;

double selectedT = selected ? 1.0 : 0.0; // 0 = False, 1 = True

void onSelectedChanged(bool next) {
  final from = selectedT;
  final to = next ? 1.0 : 0.0;
  if (disableAnimations) {
    selectedT = to;
    return;
  }
  controller.animateWith(
    SpringSimulation(spring, from, to, 0),
  );
}

// Crossfade del ícono outline → filled con selectedT (casuística Favorite / Like / Mute / Hide).
```

---

## Haptics Specification

Toggle Button dispara haptic al cambiar `Selected` usando `haptic-selection-change`.

| Campo | Valor |
|---|---|
| **Token default** | `haptic-selection-change` |
| **Trigger** | `onChanged` (Selected False ↔ True) |
| **Override** | no recomendado |

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
| Token | `motion-spring-sm` — misma familia que Switch, Checkbox e Icon Button |
| Scope | Solo Selected True/False en este handoff |
| Ícono | Swap outline → filled de la casuística (Favorite, Like, Mute, Hide) |
| Exit | no aplica |
| A11y | `MediaQuery.disableAnimations` |
| Haptics | `haptic-selection-change` en cada toggle |
