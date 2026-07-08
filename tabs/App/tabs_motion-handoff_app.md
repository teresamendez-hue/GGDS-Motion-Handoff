# Motion Handoff — Tab (App)

| | |
|---|---|
| **Componente** | Tab |
| **Plataforma** | App (Flutter) |
| **Owner** | Ezequiel Arguello |
| **Design system** | GGDS |
| **Variantes indicador** | `line` · `segmented` |
| **Token semántico (indicador + panel)** | `motion-spring-md` |
| **Token semántico (tab item)** | `motion-spring-sm` |
| **Categoría** | Guía (indicador/panel) · Default (item) |
| **Fecha** | 2026-07-08 |

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
| Variantes indicador | **Line** · **Segmented** | **Line** · **Segmented** |
| Token indicador + panel | `motion-curve-md` | `motion-spring-md` |
| Física indicador/panel | easing-decelerate · 300ms | mass 1.0 · stiffness 300 · damping 28 |
| Token tab item | `motion-curve-sm` | `motion-spring-sm` |
| Física item | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Panel | slide horizontal | slide horizontal |
| Scroll manual | nativo | nativo (`ScrollController`) |
| Reveal tab activo | `scrollTo` smooth | `ScrollController.animateTo` |
| Hover | sí | no aplica |
| Disabled | instantáneo | instantáneo |

Regla: no traducir ms de Web a duración fija en Flutter.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS.
- **Preview Web companion:** solo referencia de intención; no copiar CSS.
- **QA final:** validar indicador, slide de panel, scroll horizontal y reveal en dispositivo.

---

## Motion Specification

Tab en App cambia la selección con indicador **Line** o **Segmented**. El indicador y el **panel de contenido** se animan con `motion-spring-md` (slide horizontal coherente con el índice). Los **tab items** usan `motion-spring-sm` en pressed y focus. Sin `hover`. **Selected** es estado visual estático; el movimiento principal lo hace el indicador. **Disabled** es instantáneo.

La lista admite **scroll horizontal** nativo en overflow. El usuario desplaza con gesto nativo (sin token). Al **seleccionar un tab fuera del viewport**, `ScrollController.animateTo` revela el tab activo; con `disableAnimations` el salto es instantáneo (`jumpTo`). El indicador recalcula posición en cada tick de scroll.

Sin enter/exit de viewport. `MediaQuery.disableAnimations` aplica estado final sin transición en indicador, panel e items.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tab selected | Tab bar | — | — | — | — |
| 2 | Response | Indicador | Indicator | offset, width | tab N | tab N±1 | motion-spring-md |
| 3 | Response | Panel | Panel track | offset | panel N | panel N±1 | motion-spring-md |
| 4 | Trigger | Scroll horizontal (usuario) | Tab scroll | — | — | — | — |
| 5 | Response | Indicador sync | Indicator | offset | — | recalculado | instantáneo |
| 6 | Trigger | Tab selected off-screen | Tab scroll | — | — | — | — |
| 7 | Response | Reveal tab activo | Tab scroll | scroll offset | actual | tab visible | nativo / instantáneo |
| 8 | Trigger | Tap down | Tab item | — | — | — | — |
| 9 | Response | Pressed | Tab item | opacity | 1 | 0.85 | motion-spring-sm |
| 10 | Trigger | Focus visible | Tab item | — | — | — | — |
| 11 | Response | Focus ring | Ring | opacity | 0 | 1 | motion-spring-sm |
| 12 | Trigger | Tab disabled | Tab item | — | — | — | — |
| 13 | Response | Disabled | Tab item | todas | — | final | instantáneo |
| 14 | Trigger | `disableAnimations` | indicador, panel, items | — | — | — | — |
| 15 | Response | Sin animación | todas | — | — | final | instantáneo |

---

## Token Mapping

| Token semántico | mass | stiffness | damping | Uso |
|---|---|---|---|---|
| `motion-spring-md` | 1.0 | 300 | 28 | Indicador + slide de panel (entrada y salida) |
| `motion-spring-sm` | 1.0 | 400 | 35 | Pressed y focus del tab item |

---

## Implementación Dart

```dart
final motion = context.motionTokens;
final springNav = motion.springMd;   // motion-spring-md
final springItem = motion.springSm;  // motion-spring-sm
final disableAnimations = MediaQuery.of(context).disableAnimations;

void onTabSelected(int index) {
  if (disableAnimations) {
    _jumpToTab(index);
    _tabScrollController.jumpTo(_offsetForTab(index));
    return;
  }
  _indicatorController.animateWith(
    SpringSimulation(springNav, _indicatorController.value, index.toDouble(), 0),
  );
  _panelController.animateWith(
    SpringSimulation(springNav, _panelController.value, index.toDouble(), 0),
  );
  _revealActiveTab(index);
}

void _revealActiveTab(int index) {
  final target = _offsetForTab(index);
  if (disableAnimations) {
    _tabScrollController.jumpTo(target);
    return;
  }
  _tabScrollController.animateTo(
    target,
    duration: const Duration(milliseconds: 300),
    curve: Curves.easeOutCubic,
  );
}

_tabScrollController.addListener(_syncIndicatorOnScroll);

// Tab item pressed / focus — motion-spring-sm
void onTabPressedChanged(bool pressed) {
  if (disableAnimations) return;
  _itemController.animateWith(
    SpringSimulation(springItem, _itemController.value, pressed ? 0.85 : 1.0, 0),
  );
}
```

---

## Haptics Specification

| Campo | Valor |
|---|---|
| **Token default** | `haptic-selection-change` |
| **Trigger** | `onTabSelected` (solo si el índice cambia) |
| **Override** | no recomendado |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |

---

## Implementación Haptics

```dart
void onTabSelected(int index) {
  if (index != _currentIndex) {
    context.haptics.trigger(GgdsHapticSemantic.selectionChange);
  }
  animateToTab(index);
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Indicador + panel | `motion-spring-md` — mismo token, sincronizados |
| Tab item | `motion-spring-sm` — pressed y focus |
| Variantes | **Line** y **Segmented** — mismo spring de indicador |
| Scroll manual | nativo; sync indicador en scroll listener |
| Reveal tab activo | `animateTo`; `jumpTo` con `disableAnimations` |
| Haptics | `haptic-selection-change` al cambiar tab |
| A11y | `disableAnimations` → salto instantáneo |
