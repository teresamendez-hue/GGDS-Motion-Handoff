# Motion Handoff — Carousel (App)

| | |
|---|---|
| **Componente** | Carousel |
| **Plataforma** | App (Flutter) |
| **Owner** | Ezequiel Arguello |
| **Design system** | GGDS |
| **Token slide** | `motion-spring-md` |
| **Token dot indicator** | `motion-spring-sm` |
| **Categoría** | Guía (slide) · Default (dot indicator) |
| **Behavior** | Normal · Infinite Scroll |
| **Fecha** | 2026-07-02 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./carousel_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/carousel_motion-handoff_web.html) | Misma **intención** de motion para el slide. En App implementar con tokens **spring**, no curvas CSS. La navegación por flechas y el reveal-on-hover son exclusivos de Web — no tienen equivalente en App. |

> El preview Web muestra **cómo debe sentirse** el desplazamiento del track.
> El preview App y el Token Mapping muestran **cómo implementarlo** en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico (slide) | `motion-curve-md` | `motion-spring-md` |
| Curva / física (slide) | `easing-decelerate` · 300ms | mass 1.0 · stiffness 300 · damping 28 |
| Token semántico (dot) | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física (dot) | `easing-standard` · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Trigger de navegación | Click en flecha (reveal on hover) o dot | Drag / swipe o tap en dot — **sin flechas** |
| Límite (Normal) | Flecha llega a `Disabled` | Resistencia ("rubber-band") al arrastrar más allá del extremo, luego el spring devuelve al slide límite |
| Límite (Infinite Scroll) | Nunca se deshabilita, loop continuo | Nunca hay resistencia, loop continuo |

Regla: no traducir ms de Web a duración fija en Flutter — el spring define el asentamiento según velocidad del gesto.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **Preview Web companion:** solo referencia de intención del slide; no copiar CSS ni implementar flechas en App.
- **QA final:** validar en dispositivo o simulador con `flutter_animate` / `SpringDescription` del design system, con foco en la sensación del rubber-band en `Normal`.

---

## Motion Specification

Carousel en App navega mediante gesto: drag/swipe sobre el track o tap directo en un dot. **Nunca** se muestran flechas de navegación — el `type=Navigation` existe como variante visual en Figma pero no se traduce a interacción real en producto, ya que no existe estado `hover` en touch.

El desplazamiento del track (posición) siempre usa `motion-spring-md`, con el **mismo spring en ambas direcciones** y también al asentarse tras soltar el gesto — no hay distinción entrada/salida como en overlays.

En **Normal**, arrastrar más allá del primer o último slide aplica resistencia (rubber-band, factor ~0.35 sobre el delta) y el spring devuelve el track al slide límite al soltar. En **Infinite Scroll**, el índice hace loop sin resistencia ni límite.

El dot indicator activo cambia de color con `motion-spring-sm` cuando el slide activo cambia. `disableAnimations` aplica el estado final sin animación.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Drag / swipe sobre el track | Carousel | — | — | — | — |
| 2 | Response | Reposición durante el gesto | Track | posición X | posición actual | delta del gesto (1:1, con resistencia si aplica) | — (raw, sin spring) |
| 3 | Trigger | Gesto soltado por encima del umbral | Carousel | — | — | — | — |
| 4 | Response | Asentamiento a slide siguiente/anterior | Track | posición X | posición del gesto | posición del slide destino | `motion-spring-md` |
| 5 | Trigger | Gesto soltado por debajo del umbral | Carousel | — | — | — | — |
| 6 | Response | Vuelta al slide actual | Track | posición X | posición del gesto | posición del slide actual | `motion-spring-md` |
| 7 | Trigger | Tap en dot | Dot | — | — | — | — |
| 8 | Response | Salto directo a slide | Track | posición X | posición actual | posición del slide seleccionado | `motion-spring-md` |
| 9 | Trigger | Cambio de slide activo | Carousel | — | — | — | — |
| 10 | Response | Dot activo | Dot | color | indicator | indicator-active | `motion-spring-sm` |
| 11 | Trigger | Límite alcanzado (Normal) | Track | — | — | — | — |
| 12 | Response | Resistencia + retorno | Track | posición X | posición de resistencia | posición del slide límite | `motion-spring-md` |

---

## Token Mapping

| Token semántico | mass | stiffness | damping |
|---|---|---|---|
| `motion-spring-md` (slide) | 1.0 | 300 | 28 |
| `motion-spring-sm` (dot indicator) | 1.0 | 400 | 35 |

---

## Implementación Dart

```dart
// Consumir tokens semánticos desde el sistema de motion tokens
final motion = context.motionTokens;
final spring = motion.springMd;     // motion-spring-md (mass: 1.0, stiffness: 300, damping: 28)
final dotSpring = motion.springSm;  // motion-spring-sm (mass: 1.0, stiffness: 400, damping: 35)
final disableAnimations = MediaQuery.of(context).disableAnimations;

double dragExtent = 0.0;
int currentIndex = 0;

void onPanUpdate(DragUpdateDetails details) {
  final atStart = currentIndex == 0 && details.delta.dx > 0;
  final atEnd = currentIndex == slideCount - 1 && details.delta.dx < 0;
  final resist = (behavior == CarouselBehavior.normal && (atStart || atEnd)) ? 0.35 : 1.0;
  dragExtent += details.delta.dx * resist;
  controller.value = dragExtent;
}

void onPanEnd(DragEndDetails details) {
  final committed = dragExtent.abs() > settleThreshold;
  var targetIndex = currentIndex;

  if (committed) {
    final direction = dragExtent.isNegative ? 1 : -1;
    targetIndex = behavior == CarouselBehavior.infinite
        ? (currentIndex + direction) % slideCount
        : (currentIndex + direction).clamp(0, slideCount - 1);
  }

  if (disableAnimations) {
    controller.value = targetIndex.toDouble();
  } else {
    controller.animateWith(SpringSimulation(
      spring,
      controller.value,
      targetIndex.toDouble(),
      details.velocity.pixelsPerSecond.dx,
    ));
  }

  if (targetIndex != currentIndex) {
    currentIndex = targetIndex;
    onSlideSettled(currentIndex); // ver sección Haptics
  }
}
```

---

## Haptics Specification

Carousel dispara haptic al completarse un cambio de slide, usando `haptic-selection-change` — mismo criterio que Tabs/Segmented Control, ya que funcionalmente el Carousel cambia cuál "página" está activa.

| Campo | Valor |
|---|---|
| **Token default** | `haptic-selection-change` |
| **Trigger** | Al completarse el cambio de slide (swipe soltado por encima del umbral, o tap en dot) |
| **Override** | No recomendado |

**Restricción crítica:** no disparar en cada frame del drag — un evento = un haptic, solo al asentarse el nuevo índice.

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |

---

## Implementación Haptics

```dart
void onSlideSettled(int newIndex) {
  if (newIndex != previousIndex) {
    context.haptics.trigger(GgdsHapticSemantic.selectionChange); // haptic-selection-change
    previousIndex = newIndex;
  }
}
```

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token único | `motion-spring-md` en ambas direcciones y en el asentamiento; nunca mezclar con curvas bezier |
| Sin flechas | App no muestra flechas de navegación en ningún caso — toda la interacción es gestual |
| Límite (Normal) | Resistencia ("rubber-band") al arrastrar más allá del extremo, no un estado `Disabled` visual |
| Haptics | `haptic-selection-change` solo al completarse el cambio, nunca durante el drag |
| A11y | `MediaQuery.disableAnimations` aplica el estado final sin animación |
