# Motion Handoff — Carousel (App)

| | |
|---|---|
| **Componente** | Carousel |
| **Plataforma** | App (Flutter) |
| **Owner** | Ezequiel Arguello |
| **Design system** | GGDS |
| **Variantes** | `infinite` · `finite` |
| **Token semántico (slide)** | `motion-spring-md` |
| **Token semántico (dots)** | `motion-spring-sm` |
| **Categoría** | Guía (slide) · Default (dots) |
| **Fecha** | 2026-07-08 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./carousel_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/carousel_motion-handoff_web.html) | Misma intención de motion. En App implementar con tokens spring, no curvas CSS. |

> El preview Web muestra cómo debe sentirse el componente.  
> El preview App y el Token Mapping muestran cómo implementarlo en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Variantes | `infinite` · `finite` | `infinite` · `finite` |
| Token slide | `motion-curve-md` | `motion-spring-md` |
| Física slide | easing-decelerate · 300ms | mass 1.0 · stiffness 300 · damping 28 |
| Token dots | `motion-curve-sm` | `motion-spring-sm` |
| Física dots | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Controles | Flechas + dots | Solo dots (sin flechas) |
| Navegación slide | Swipe, flecha, dot | Swipe, dot |
| Infinite reset | `transition: none` | `jumpToPage` sin animación |
| Peek | slides adyacentes visibles | `viewportFraction` ~0.82 |
| Autoplay | no | no |
| Hover | sí (flechas Web) | no aplica |

Regla: no traducir ms de Web a duración fija en Flutter.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS.
- **Preview Web companion:** solo referencia de intención; no copiar CSS.
- **QA final:** validar swipe, snap, peek y loop infinito en dispositivo con `PageView` / `SpringSimulation`.

---

## Motion Specification

Carousel en App desplaza el track con `motion-spring-md` en modo peek. La navegación es por **swipe** o **tap en dots** — no incluye flechas. Swipe y dot comparten el mismo spring para el cambio de slide en ambas variantes.

Dos variantes: **`infinite`** hace loop continuo (reset instantáneo al clonar); **`finite`** pagina con extremos (el swipe no avanza más allá del primero/último slide). Los **dots** animan el indicador activo con `motion-spring-sm`. Sin `hover`. Sin autoplay.

`MediaQuery.disableAnimations` aplica estado final sin transición en track y dots.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Swipe / dot | Track / PageView | — | — | — | — |
| 2 | Response | Slide change | Track | offset | slide N | slide N±1 | motion-spring-md |
| 3 | Trigger | Dot selected | Dot | — | — | — | — |
| 4 | Response | Active dot | Dot | scale, opacity | inactive | active | motion-spring-sm |
| 5 | Trigger | Loop boundary (`infinite`) | Track | — | — | — | — |
| 6 | Response | Clone reset | Track | offset | clone | real slide | instantáneo |
| 7 | Trigger | `disableAnimations` | Track, dots | — | — | — | — |
| 8 | Response | Sin animación | track + dots | todas | — | final | instantáneo |

---

## Token Mapping

| Token semántico | mass | stiffness | damping | Uso |
|---|---|---|---|---|
| `motion-spring-md` | 1.0 | 300 | 28 | Cambio de slide (entrada y salida) |
| `motion-spring-sm` | 1.0 | 400 | 35 | Indicador activo en dots |

---

## Implementación Dart

```dart
final motion = context.motionTokens;
final springSlide = motion.springMd;   // motion-spring-md
final springDot = motion.springSm;     // motion-spring-sm
final disableAnimations = MediaQuery.of(context).disableAnimations;

final _pageController = PageController(viewportFraction: 0.82); // peek

void animateToSlide(int index) {
  if (disableAnimations) {
    _pageController.jumpToPage(index);
    return;
  }
  // Resolver snap con SpringSimulation + springSlide
  _slideController.animateWith(
    SpringSimulation(springSlide, _slideController.value, index.toDouble(), 0),
  );
}

// infinite: tras animar al clone, jumpToPage al slide real sin animación
void onInfiniteLoopReset(int realIndex) {
  _pageController.jumpToPage(realIndex);
}

// Dots — indicador activo con motion-spring-sm
void animateDotActive(bool active, AnimationController dotController) {
  if (disableAnimations) {
    dotController.value = active ? 1.0 : 0.0;
    return;
  }
  dotController.animateWith(
    SpringSimulation(springDot, dotController.value, active ? 1.0 : 0.0, 0),
  );
}
```

---

## Haptics Specification

Haptic **solo** en tap de dot. Swipe sin haptic (prohibido en drag/scroll continuo).

| Campo | Valor |
|---|---|
| **Token default** | `haptic-selection-change` |
| **Trigger** | `onDotTapped` (solo si el índice cambia) |
| **Override** | no recomendado |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |

---

## Implementación Haptics

```dart
void onDotTapped(int index) {
  if (index != _currentPage) {
    context.haptics.trigger(GgdsHapticSemantic.selectionChange);
    animateToSlide(index);
  }
}

// Swipe: sin haptic
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Slide | `motion-spring-md` — navegación guiada, mismo token entrada/salida |
| Dots | `motion-spring-sm` — separado del slide |
| Controles | Solo dots — sin flechas en App |
| `infinite` | Spring entre adyacentes; `jumpToPage` en reset de clone |
| `finite` | Mismo spring; swipe bloqueado en extremos |
| Peek | `viewportFraction` ~0.82 |
| Haptics | solo dot tap; nunca en swipe |
| A11y | `MediaQuery.disableAnimations` → salto instantáneo |
