# Motion Handoff — Modal (App)

| | |
|---|---|
| **Componente** | Modal |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token — Default / Illustration** | `motion-spring-xl` |
| **Token — Full Screen** | `motion-spring-md` |
| **Categoría** | Interactivos · Guía |
| **Fecha** | 2026-07-01 |
| **Figma** | [Core — App Components · Modal](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=9209-462) |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./modal_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |

> El preview App y el Token Mapping muestran **cómo implementarlo** en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico (Default) | `motion-curve-xl` | `motion-spring-xl` |
| Curva / física | `easing-standard` · 700ms | mass 1.0 · stiffness 100 · damping 20 |
| Panel Default | `translateY` 8% → 0, sin scale | `scale` 0.92 → 1, sin translateY |
| Scrim | opacity 0 → 0.7 | opacity 0 → 1 (implementación nativa) |
| Full Screen | no aplica en Web handoff | `translateY` 100% → 0 · `motion-spring-md` |
| Entrada / salida | `motion-curve-xl` + `motion-exit` | **mismo spring** ida y vuelta por variante |
| Skeleton | visual, sin swap | visual, sin swap |
| Haptics modal | no aplica | **ninguno** — botones usan sus tokens |

Regla: no traducir ms de Web a duración fija en Flutter. El spring define el asentamiento.

**Excepción documentada:** Web usa translateY sin scale; App Default usa scale sin translateY. Ambos producen sensación de diálogo centrado emergente.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **QA final:** validar en dispositivo o simulador con `flutter_animate` / `SpringDescription` del design system.

---

## Motion Specification

El Modal en App es un overlay bloqueante que cubre entrada y salida del viewport (scrim + panel). En **Default** e **Illustration**, el panel aparece centrado: se animan opacity del scrim y del panel, y **scale** del panel (0.92 → 1.0). No se usa `translateY` en estas variantes — un deslizamiento vertical se confundiría con Bottom Sheet.

En **Full Screen**, el panel entra desde abajo (`Offset Y` 1.0 → 0.0). El scrim solo hace fade; el gesto principal lo lleva el panel con `motion-spring-md`.

Entrada y salida usan **el mismo spring** de cada variante. La física del spring hace que el cierre se sienta ligeramente más ágil sin token de salida distinto.

**Skeleton:** estado visual al abrir; **sin animación de swap** al cargar contenido. **Scroll:** comportamiento nativo; sin motion adicional.

**Haptics:** el Modal **no** tiene haptic propio. Los botones de acción usan `haptic-action-press` según sus handoffs.

**Cierre:** botón X, tap en scrim, `Navigator.pop`/back, o botones de acción que cierran. Sin auto-dismiss.

Variantes de contenido (title, description, slot, close, **_Overlays Actions**) no alteran el motion de overlay.

---

## Timeline de interacción

### A — Default / Illustration · Entrada y salida

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Modal se abre | — | — | — | — | — | — | 0 | — |
| 2 | Response | Scrim aparece | Scrim | `opacity` | 0.0 | 1.0 | `motion-spring-xl` | mass: 1, stiffness: 100, damping: 20 | 0 | ~650 |
| 3 | Response | Panel aparece | Modal panel | `opacity` | 0.0 | 1.0 | `motion-spring-xl` | mass: 1, stiffness: 100, damping: 20 | 0 | ~650 |
| 4 | Response | Panel escala | Modal panel | `scale` | 0.92 | 1.0 | `motion-spring-xl` | mass: 1, stiffness: 100, damping: 20 | 0 | ~650 |
| 5 | Trigger | Usuario cierra | — | — | — | — | — | — | 0 | — |
| 6 | Response | Scrim desaparece | Scrim | `opacity` | 1.0 | 0.0 | `motion-spring-xl` | mass: 1, stiffness: 100, damping: 20 | 0 | ~480 |
| 7 | Response | Panel desaparece | Modal panel | `opacity` | 1.0 | 0.0 | `motion-spring-xl` | mass: 1, stiffness: 100, damping: 20 | 0 | ~480 |
| 8 | Response | Panel escala al ocultarse | Modal panel | `scale` | 1.0 | 0.92 | `motion-spring-xl` | mass: 1, stiffness: 100, damping: 20 | 0 | ~480 |

### B — Full Screen · Entrada y salida

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Modal Full Screen se abre | — | — | — | — | — | — | 0 | — |
| 2 | Response | Scrim aparece | Scrim | `opacity` | 0.0 | 1.0 | `motion-spring-md` | mass: 1, stiffness: 300, damping: 28 | 0 | ~400 |
| 3 | Response | Panel entra desde abajo | Modal panel | `Offset Y` | Offset(0, 1.0) | Offset(0, 0.0) | `motion-spring-md` | mass: 1, stiffness: 300, damping: 28 | 0 | ~400 |
| 4 | Response | Panel visible | Modal panel | `opacity` | 0.0 | 1.0 | `motion-spring-md` | mass: 1, stiffness: 300, damping: 28 | 0 | ~400 |
| 5 | Trigger | Usuario cierra | — | — | — | — | — | — | 0 | — |
| 6 | Response | Panel sale hacia abajo | Modal panel | `Offset Y` | Offset(0, 0.0) | Offset(0, 1.0) | `motion-spring-md` | mass: 1, stiffness: 300, damping: 28 | 0 | ~280 |
| 7 | Response | Panel y scrim desvanecen | Modal panel / Scrim | `opacity` | 1.0 | 0.0 | `motion-spring-md` | mass: 1, stiffness: 300, damping: 28 | 0 | ~280 |

> Fin (ms) son estimaciones de asentamiento para Figma. En código no fijar `Duration`.

---

## Token Mapping

### Default / Illustration

| Token semántico | mass | stiffness | damping |
|---|---:|---:|---:|
| `motion-spring-xl` | 1.0 | 100 | 20 |

### Full Screen

| Token semántico | mass | stiffness | damping |
|---|---:|---:|---:|
| `motion-spring-md` | 1.0 | 300 | 28 |

---

## Implementación Dart

```dart
SpringDescription _springXl() => const SpringDescription(
  mass: 1.0,
  stiffness: 100,
  damping: 20,
);

SpringDescription _springMd() => const SpringDescription(
  mass: 1.0,
  stiffness: 300,
  damping: 28,
);

// Default / Illustration — entrada
Widget modalDefaultEnter(Widget child) => Stack(
  children: [
    Container(color: Colors.black54)
        .animate()
        .fadeIn(curve: SpringCurve(spring: _springXl())),
    Center(
      child: child
          .animate()
          .fadeIn(curve: SpringCurve(spring: _springXl()))
          .scale(
            begin: const Offset(0.92, 0.92),
            end: const Offset(1, 1),
            curve: SpringCurve(spring: _springXl()),
          ),
    ),
  ],
);

// Default / Illustration — salida (target: 0)
child.animate(target: 0)
  .fadeOut(curve: SpringCurve(spring: _springXl()))
  .scale(
    end: const Offset(0.92, 0.92),
    curve: SpringCurve(spring: _springXl()),
  );

// Full Screen — entrada
child.animate()
  .fadeIn(curve: SpringCurve(spring: _springMd()))
  .move(
    begin: const Offset(0, 1.0),
    end: Offset.zero,
    curve: SpringCurve(spring: _springMd()),
  );

// Full Screen — salida
child.animate(target: 0)
  .fadeOut(curve: SpringCurve(spring: _springMd()))
  .moveY(end: 1.0, curve: SpringCurve(spring: _springMd()));

// Accesibilidad
if (MediaQuery.of(context).disableAnimations) {
  // opacity: 1, scale: 1, offset: zero — sin spring
}
```

> No forzar `Duration` en springs. Skeleton: mostrar placeholders sin crossfade animado.

---

## Notas para Figma (prototipado)

1. **Default / Illustration — cerrado:** scrim `Opacity: 0`; panel `Opacity: 0`, `Scale: 92%`.
2. **Default / Illustration — abierto:** scrim `Opacity: 100%`; panel `Opacity: 100%`, `Scale: 100%`.
3. **Full Screen — cerrado:** panel `Y: +100%` del contenedor + `Opacity: 0`.
4. **Full Screen — abierto:** `Y: 0`, `Opacity: 100%`.
5. Aproximación spring: `motion-spring-xl` → Bezier `0.2, 0, 0, 1` ~650ms entrada / ~480ms salida; `motion-spring-md` → `0.0, 0, 0.2, 1` ~400ms / ~280ms.
6. **Skeleton:** frame abierto con placeholders; swap a contenido **sin** animación.
7. Cierre: On click en X, scrim o botón de acción.

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Illustration = Default | Misma animación de overlay |
| No `motion-spring-lg` | Reservado para toast/snackbar |
| Haptics | Modal sin haptic; botones con `haptic-action-press` |
| Accesibilidad | `MediaQuery.disableAnimations` → estado final directo |
| Scroll | Sin tokens de motion |

### Referencias externas

- [Material Design Motion](https://m3.material.io/styles/motion/overview)
- [Apple HIG — Animation](https://developer.apple.com/design/human-interface-guidelines/animation)
- [flutter_animate](https://pub.dev/packages/flutter_animate)
