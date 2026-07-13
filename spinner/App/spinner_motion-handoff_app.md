# Motion Handoff — Spinner (App)

| | |
|---|---|
| **Componente** | Spinner |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token rotación** | *(sin semántico)* → `Curves.linear` · 800ms / vuelta |
| **Categoría** | Default |
| **Fecha** | 2026-06-29 |
| **Figma** | [Core — App Components](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=5358-336) |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./spinner_motion-handoff_app.html) | Rotación lineal simulada en JS (800ms / vuelta). No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/spinner_motion-handoff_web.html) | Misma **intención** de motion (linear · 800ms). En App implementar con `AnimationController.repeat()`, no springs. |

> El preview Web muestra **cómo debe sentirse** el componente.  
> El preview App y el Token Mapping muestran **cómo implementarlo** en Flutter.

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | *(ninguno)* | *(ninguno — no usar spring)* |
| Curva / física | `easing-linear` · 800ms / vuelta | `Curves.linear` · `Duration(milliseconds: 800)` |
| Rotación | `@keyframes` infinite | `AnimationController.repeat()` |
| Montaje / desmontaje | instantáneo | instantáneo |
| Cambio color / size | instantáneo | instantáneo |
| Accesibilidad | `prefers-reduced-motion` → `animation: none` | `MediaQuery.disableAnimations` → `controller.stop()` |

**Paridad perceptual:** misma velocidad angular (1 revolución / 800ms), misma pausa bajo reduced motion. **No** traducir a `motion-spring-*` — el spinner es rotación lineal continua, no un spring de asentamiento.

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** rotación lineal en JS; mismo período 800ms que Web.
- **Preview Web companion:** referencia de intención; no copiar CSS literal en Flutter.
- **QA final:** validar en dispositivo con `AnimationController` + `Curves.linear`.

---

## Motion Specification

Spinner **indeterminado** de carga pasiva. Variantes **Color** (accent, neutral, white) y **Size** (S → 5XL). Sin highlight/pressed/focus. Sin haptics.

**Única animación documentada:** rotación continua del arco mientras el widget está visible.

- **Motor:** `AnimationController` con `duration: Duration(milliseconds: 800)` y `repeat()`.
- **Curva:** `Curves.linear` (no spring, no `flutter_animate` spring para este loop).
- **Transform:** `RotationTransition` o `Transform.rotate` con ángulo 0→2π.

**Sin animación** en: montaje/desmontaje, cambio de color/size, skeleton (excluido).

> **Nota Figma:** este handoff documenta solo motion para implementación en código.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Montaje / visible | — | — | — | — | — | — | 0 | — |
| 2 | Response | Aparición | `Spinner` | — | — | — | **sin animación** | — | 0 | 0 |
| 3 | Response | Rotación continua | arco SVG / `CustomPaint` | `rotation` | 0 | 2π (loop) | `Curves.linear` · 800ms | 800ms / vuelta | 0 | ∞ |
| 4 | Trigger | Cambio `color` o `size` | — | — | — | — | — | — | * | — |
| 5 | Response | Swap variante | `Spinner` | color / size | anterior | nuevo | **sin animación** | — | — | — |
| 6 | Trigger | Desmontaje | — | — | — | — | — | — | * | — |
| 7 | Response | Salida | `Spinner` | — | — | — | **sin animación** | — | — | — |
| 8 | Trigger | `MediaQuery.disableAnimations` | — | — | — | — | — | — | * | — |
| 9 | Response | Pausa rotación | `AnimationController` | `isAnimating` | true | false (stop) | — | — | * | — |

> `*` = momento variable.

---

## Token Mapping

| Momento | Token semántico | Curva / motor | Período |
|---|---|---|---|
| Rotación continua | *(ninguno)* | `Curves.linear` + `AnimationController.repeat()` | 800ms / revolución |
| Montaje / desmontaje | — | — | instantáneo |
| Cambio color / size | — | — | instantáneo |
| `disableAnimations` | — | `controller.stop()` | rotación pausada |

---

## Implementación Dart (Flutter)

```dart
class GgdsSpinner extends StatefulWidget {
  const GgdsSpinner({super.key});

  @override
  State<GgdsSpinner> createState() => _GgdsSpinnerState();
}

class _GgdsSpinnerState extends State<GgdsSpinner>
    with SingleTickerProviderStateMixin {
  late final AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      duration: const Duration(milliseconds: 800),
    );
    _maybeStart();
  }

  void _maybeStart() {
    final disable = MediaQuery.disableAnimationsOf(context);
    if (disable) {
      _controller.stop();
      return;
    }
    _controller.repeat();
  }

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    _maybeStart();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return RotationTransition(
      turns: _controller,
      child: /* arco indeterminado */,
    );
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| No usar spring | `motion-spring-*` no aplica a loops lineales continuos |
| Coherencia Button / Icon Button | Mismo período 800ms y `Curves.linear` en spinners embebidos |
| Cambio de variante | `color` / `size`: rebuild instantáneo, sin `AnimatedSwitcher` |
| Skeleton | Excluido |
| Accesibilidad | `Semantics(label: 'Loading', liveRegion: true)`; `disableAnimations`: detener controller, mantener widget visible |
| Haptics | Ninguno |
