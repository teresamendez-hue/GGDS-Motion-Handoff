# Motion Handoff — Option Item (App)

| | |
|---|---|
| **Componente** | Option Item |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./option-item_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/option-item_motion-handoff_web.html) | Misma intención de motion. En App implementar con token spring, no curvas CSS. |

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Estados de fila | enabled, hover, pressed, focus, selected, selected+focus | enabled, pressed, focus, selected, selected+focus |
| Disabled | no aplica | no aplica |
| Viewport enter/exit | no aplica | no aplica |
| Hijos opcionales | Badge, Thumbnail, Checkbox | Badge, Thumbnail, Checkbox |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Scope:** solo la fila (`OptionItem`). Badge, Thumbnail y Checkbox anidados usan sus handoffs.
- **Sin hover en App:** no modelar hover; solo pressed, focus, selected.
- **Sin disabled:** este componente no define estado disabled.
- **Skeleton:** no aplica en este handoff; loading/skeleton lo resuelve implementación del consumidor.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Composición / Consumidores

| Consumidor | Uso |
|---|---|
| [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) | Lista de Option Items en el body del sheet. |
| [Field Phone Number](../../field-phone-number/App/field-phone-number_motion-handoff_app.html) | Países como Option Items dentro del sheet. |
| **Search** | Filas de resultados. |

El motion de apertura/cierre del Bottom Sheet no va en este handoff.

---

## Motion Specification

Option Item en App anima estados internos de la fila con `motion-spring-sm` sobre `background-color` y `opacity` del focus ring.

Sin enter/exit de viewport. Sin hover. Sin disabled.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tap down | fila | — | — | — | — |
| 2 | Response | Pressed | fila | `background-color` | default | pressed | motion-spring-sm |
| 3 | Trigger | Focus visible | fila | — | — | — | — |
| 4 | Response | Focus ring | ring | `opacity` | 0 | 1 | motion-spring-sm |
| 5 | Trigger | Select option | fila | — | — | — | — |
| 6 | Response | Selected | fila | `background-color` | default | selected | motion-spring-sm |
| 7 | Trigger | Focus + selected | fila | — | — | — | — |
| 8 | Response | Selected focused | fila + ring | `background-color`, `opacity` | selected | selected+focus | motion-spring-sm |

---

## Token Mapping

| Token semántico | mass | stiffness | damping |
|---|---|---|
| `motion-spring-sm` | 1.0 | 400 | 35 |

---

## Implementación Dart

```dart
const _optionItemSpring = SpringDescription(
  mass: 1.0,
  stiffness: 400.0,
  damping: 35.0,
);

void animateOptionItemState({
  required AnimationController controller,
  required double target,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) {
    controller.value = target;
    return;
  }
  controller.animateWith(
    SpringSimulation(
      _optionItemSpring,
      controller.value,
      target,
      0.0,
    ),
  );
}

// target mapea estado visual: enabled, pressed, selected, etc.
// Checkbox anidado: usar handoff Checkbox para el control
```

---

## Haptics Specification

Option Item dispara haptic al cambiar selección con `haptic-selection-change`.

| Campo | Valor |
|---|---|
| **Token default** | `haptic-selection-change` |
| **Trigger** | selección de opción (tap que cambia selected) |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |

---

## Implementación Haptics

```dart
void onOptionSelected() {
  context.haptics.trigger(GgdsHapticSemantic.selectionChange);
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-spring-sm` para estados de fila |
| Hijos | Badge / Thumbnail / Checkbox — handoffs propios |
| Hover | no en App |
| Disabled | no aplica |
| Haptics | solo al cambiar selección |
| A11y | `MediaQuery.disableAnimations` |
| Contenedor | Bottom Sheet / listas — handoff aparte |
