# Motion Handoff — Progress Indicator (App)

| | |
|---|---|
| **Componente** | Progress Indicator |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token cambio de %** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-07-01 |
| **Figma** | [Core — App Components](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=7989-1225) |

---

## Motion Specification

Indicador de progreso **determinado** (con porcentaje numérico). Variantes **Bar** y **Circular** con paridad Web/App. Props opcionales: label, description, percentage.

**Única animación documentada:** cuando cambia el valor de `percentage`.

- **Bar:** `width` (o `FractionallySizedBox`) del fill de 0 a N%.
- **Circular:** `stroke-dashoffset` del `CustomPainter` / `CircularProgressIndicator` custom de 0 a N%.
- **Texto de porcentaje:** anima en **paralelo** con el fill usando el mismo spring.

Token: `motion-spring-sm` (mass 1.0, stiffness 400, damping 35). Mismo spring al subir o bajar el valor.

**Sin animación** en: montaje inicial, cambio Bar ↔ Circular, toggle label/description, skeleton (excluido).

Sin viewport enter/exit. Sin haptics.

> **Nota Figma:** este handoff documenta solo motion para implementación en código.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Cambio de `percentage` | — | — | — | — | — | — | * | — |
| 2 | Response | Bar fill | fill widget | width / progress | % anterior | % nuevo | `motion-spring-sm` | ~spring | * | *+T |
| 3 | Response | Circular arc | arc painter | stroke-dashoffset | anterior | nuevo | `motion-spring-sm` | ~spring | * | *+T |
| 4 | Response | Texto % | Text | valor | % anterior | % nuevo | `motion-spring-sm` | ~spring | * | *+T |
| 5 | Trigger | Cambio Type Bar ↔ Circular | — | — | — | — | — | — | * | — |
| 6 | Response | Swap variante | root | — | — | — | **sin animación** | — | — | — |

> `*` = momento variable. `T` = settle del spring.

---

## Token Mapping

| Momento | Token semántico | mass | stiffness | damping |
|---|---|---|---|---|
| Cambio de % | `motion-spring-sm` | 1.0 | 400 | 35 |

---

## Implementación Dart (Flutter)

```dart
const motionSpringSm = SpringDescription(mass: 1.0, stiffness: 400, damping: 35);

void animateProgress({
  required double from,
  required double to,
  required ValueNotifier<double> progress,
}) {
  if (disableAnimations) {
    progress.value = to;
    return;
  }
  // Un AnimationController + SpringSimulation para fill y texto en paralelo
  final simulation = SpringSimulation(motionSpringSm, from, to, 0.0);
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Determinate only | Sin modo indeterminado |
| Paridad Web | Mismo comportamiento; Web usa `motion-curve-sm` 150ms |
| Type swap | Instantáneo |
| Skeleton | Excluido |
| Accesibilidad | `Semantics` con `value`; `disableAnimations`: valor final |
| Haptics | Ninguno |
