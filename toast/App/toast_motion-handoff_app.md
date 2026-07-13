# Motion Handoff — Toast (App)

| | |
|---|---|
| **Componente** | Toast |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-lg` (entrada) · paridad `motion-exit` (salida estándar) |
| **Categoría** | Comunicación |
| **Fecha** | 2026-06-24 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./toast_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |

> El preview App y el Token Mapping muestran **cómo implementarlo** en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico (entrada) | `motion-curve-lg` | `motion-spring-lg` |
| Curva / física entrada | `easing-emphasis` · 500ms | mass 1.0 · stiffness 200 · damping 30 |
| Salida estándar (X / timer) | `motion-exit` · 300ms · `translateY` | `Curves.easeIn` · 300ms · `translateY` (paridad `motion-exit`) |
| Salida swipe | `motion-exit` · 300ms · `translateX` | `motion-spring-lg` · `translateX` |
| Posición | abajo a la derecha (`right: 24px`, `bottom: 24px`) | abajo a la derecha, safe area 24px |
| Dirección entrada | `translateY` 100% → 0% | `translateY` desde borde inferior |
| Swipe dismiss | lateral (`translateX`) | lateral (`translateX`) |
| Disabled | no aplica | no aplica |

Regla: no traducir ms de Web a duración fija en Flutter. El spring define el asentamiento.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **QA final:** validar en dispositivo o simulador con `flutter_animate` / `SpringDescription` del design system.

---

## Motion Specification

El Toast en App informa sin bloquear el flujo principal. Se posiciona en la **parte inferior derecha** con safe area 24px, ancho 320px.

**Entrada:** `motion-spring-lg` (mass 1.0, stiffness 200, damping 30) en `opacity` + `translateY`.

**Salida estándar** (tap X, auto-dismiss): paridad con Web — `Curves.easeIn` · 300ms (`motion-exit`) en `opacity` + `translateY` hacia abajo. No reutilizar el spring de entrada: el recorrido vertical es el mismo y la salida debe sentirse ~40% más ágil.

**Salida swipe:** el toast sigue el gesto en `translateX`. Al superar el umbral, continúa **por el costado** con `opacity` + `translateX` y `motion-spring-lg` — sin `translateY`.

El timer de 6 segundos arranca al completar la entrada. Con Action Link no hay auto-dismiss. Long press pausa el timer. Un toast visible a la vez. Haptics según variante semántica al completar la entrada (ver sección Haptics).

## Variante semántica

Propiedad `semantic` del componente. Afecta ícono, estilo y haptic; **no** cambia el motion de entrada/salida.

| Variante | Valor | Borde | Badge | Haptic |
|---|---|---|---|---|
| Information | `ToastSemantic.information` | `#004799` | `#1d85fc` | none (default) |
| Success | `ToastSemantic.success` | `#146542` | `#16a366` | opcional · `haptic-feedback-success` · `lightImpact()` |
| Warning | `ToastSemantic.warning` | `#664400` | `#ea9e06` | opcional · `haptic-feedback-warning` · `mediumImpact()` |
| Error | `ToastSemantic.error` | `#d3315f` | `#d3315f` | según criterio · `haptic-feedback-error` · `heavyImpact()` |

`actionLink` es **opcional** (`false` por defecto). Si está activo, no hay auto-dismiss de 6s.

```dart
enum ToastSemantic { information, success, warning, error }

final semantic = toastData.semantic ?? ToastSemantic.information;
```

> **Delta Figma:** handoff visual indica fade-in 300ms. Este documento usa `motion-spring-lg` + slide, alineado al Motion System y paridad Web.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Sistema muestra toast (programmatic) | — | — | — | — | — | — | 0 | — |
| 2 | Response | Toast entra | `.toast` | `opacity` | 0 | 1 | `motion-spring-lg` | mass 1.0 · stiffness 200 · damping 30 | 0 | settle |
| 3 | Response | Toast entra | `.toast` | `translateY` | offset inferior | 0 | `motion-spring-lg` | mass 1.0 · stiffness 200 · damping 30 | 0 | settle |
| 4 | Trigger | Entrada completa → timer 6s (sin Action Link) | — | — | — | — | — | — | settle | — |
| 5 | Trigger | Auto-dismiss 6000ms post-entrada | — | — | — | — | — | — | settle+6000 | — |
| 6 | Response | Toast sale | `.toast` | `opacity` | 1 | 0 | paridad `motion-exit` | `Curves.easeIn` · 300ms | * | *+300 |
| 7 | Response | Toast sale | `.toast` | `translateY` | 0 | offset inferior | paridad `motion-exit` | `Curves.easeIn` · 300ms | * | *+300 |
| 8 | Trigger | Tap en X | — | — | — | — | — | — | * | — |
| 9 | Response | Salida estándar | `.toast` | `opacity`, `translateY` | visible | oculto | paridad `motion-exit` | `Curves.easeIn` · 300ms | * | *+300 |
| 10 | Trigger | Swipe lateral completado | — | — | — | — | — | — | * | — |
| 11 | Response | Salida por swipe (costado) | `.toast` | `opacity`, `translateX` | visible | fuera viewport lateral | `motion-spring-lg` | mass 1.0 · stiffness 200 · damping 30 | * | settle |
| 12 | Trigger | Long press → pausa timer | — | — | — | — | — | — | * | — |
| 13 | Trigger | Release → retoma timer | — | — | — | — | — | — | * | — |
| 14 | Trigger | Con Action Link → sin timer 6s | — | — | — | — | — | — | settle | — |

---

## Token Mapping

| Token semántico | mass | stiffness | damping | Nota |
|---|---:|---:|---:|---|
| `motion-spring-lg` | 1.0 | 200 | 30 | entrada |
| paridad `motion-exit` | — | — | — | salida estándar · `Curves.easeIn` · 300ms |
| `motion-spring-lg` | 1.0 | 200 | 30 | salida swipe |

---

## Implementación Dart

```dart
const motionSpringLg = SpringDescription(
  mass: 1.0,
  stiffness: 200,
  damping: 30,
);

final disableAnimations = MediaQuery.of(context).disableAnimations;

Future<void> showToast(ToastData data) async {
  if (disableAnimations) {
    toastController.setVisible();
    return;
  }

  await toastController.animateIn(
    spring: motionSpringLg,
    opacity: const Tuple2(0.0, 1.0),
    translateY: const Tuple2(1.0, 0.0),
  );
}

Future<void> dismissToast({required DismissReason reason}) async {
  if (disableAnimations) {
    toastController.setHidden();
    return;
  }

  await toastController.animateOut(
    curve: Curves.easeIn,
    duration: const Duration(milliseconds: 300),
    opacity: const Tuple2(1.0, 0.0),
    translateY: const Tuple2(0.0, 1.0),
  );
}

Future<void> dismissToastSwipe({
  required DismissReason reason,
  required double dragOffsetX,
}) async {
  if (disableAnimations) {
    toastController.setHidden();
    return;
  }

  final sign = dragOffsetX >= 0 ? 1.0 : -1.0;
  await toastController.animateOut(
    spring: motionSpringLg,
    opacity: const Tuple2(1.0, 0.0),
    translateX: Tuple2(dragOffsetX, sign * toastWidth),
    // translateY se mantiene en 0 — salida lateral, no hacia abajo
  );
}

// Swipe: seguir gesto en translateX; al threshold → dismissToastSwipe(...)
// Tap X / auto-dismiss: dismissToast() con translateY
// Long press: pausar timer auto-dismiss
```

---

## Haptics Specification

Haptics al completar la entrada (`onToastShown`), mapeados por variante semántica. Information no dispara haptic por defecto.

| Variante | ¿Haptic? | Token semántico | API Flutter | Criterio |
|---|---|---|---|---|
| Information | No | none | — | Mensaje neutro/informativo |
| Success | Opcional | `haptic-feedback-success` | `HapticFeedback.lightImpact()` | Confirmación positiva |
| Warning | Opcional | `haptic-feedback-warning` | `HapticFeedback.mediumImpact()` | Advertencia con énfasis moderado |
| Error | Según criterio | `haptic-feedback-error` | `HapticFeedback.heavyImpact()` | Solo errores críticos; sin repetición en validaciones menores |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| none | — | — |
| `haptic-feedback-success` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |
| `haptic-feedback-warning` | `haptic-impact-medium` | `HapticFeedback.mediumImpact()` |
| `haptic-feedback-error` | `haptic-impact-heavy` | `HapticFeedback.heavyImpact()` |

---

## Implementación Haptics

```dart
HapticFeedback? hapticForSemantic(ToastSemantic semantic) {
  switch (semantic) {
    case ToastSemantic.information:
      return null;
    case ToastSemantic.success:
      return () => HapticFeedback.lightImpact();
    case ToastSemantic.warning:
      return () => HapticFeedback.mediumImpact();
    case ToastSemantic.error:
      return () => HapticFeedback.heavyImpact();
  }
}

Future<void> onToastShown() async {
  final trigger = hapticForSemantic(toastData.semantic ?? ToastSemantic.information);
  if (trigger != null) trigger();
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-spring-lg` — paridad con Snackbar; no usar `motion-spring-xl` (reservado para modales) |
| Posición | Abajo a la derecha; safe area 24px; ancho 320px |
| Salida estándar | Paridad `motion-exit` · `Curves.easeIn` · 300ms — no reutilizar spring de entrada |
| Salida swipe | `motion-spring-lg` · `translateX` en dirección del gesto |
| Action Link | Sin auto-dismiss |
| Cola | Un toast; siguiente entra al cerrar el actual |
| Haptic | Por variante: none · success (light) · warning (medium) · error (heavy, solo crítico) |
| Accesibilidad | `MediaQuery.disableAnimations`: estado final instantáneo |
