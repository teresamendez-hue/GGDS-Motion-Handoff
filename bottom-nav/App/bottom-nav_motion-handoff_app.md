# Motion Handoff — Bottom Nav (App)

| | |
|---|---|
| **Componente** | Bottom Nav |
| **Plataforma** | App (Flutter) |
| **Owner** | Juan Carlos De La Via Levy |
| **Design system** | GGDS |
| **Variantes layout** | `fixed` · `floating` |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-09-01 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./bottom-nav_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |

> El preview App y el Token Mapping muestran cómo implementarlo en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Companion Web | no documentado | — |
| Token item / FAB | — | `motion-spring-sm` |
| Física | — | mass 1.0 · stiffness 400 · damping 35 |
| Selección | — | transición de color icono + label |
| Indicador deslizante | — | no aplica |
| Hover | — | no aplica |
| Disabled | — | instantáneo |

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS.
- **QA final:** validar pressed, cambio de selección, FAB y variantes Fixed/Floating en dispositivo.
- **Figma:** [Bottom Nav](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/branch/ahhZnEMvRu7ZvVQ8GlSFbh/Core---App-Components?node-id=13132-1272) · [Handoff](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/branch/ahhZnEMvRu7ZvVQ8GlSFbh/Core---App-Components?node-id=13211-6646)

---

## Motion Specification

Bottom Nav en App es navegación persistente entre secciones. No tiene entrada ni salida de viewport. Al tocar un **item**, la app navega a otra sección y el item pasa a **Selected**: icono y label transicionan a `color/icon/accent` y `color/text/accent` (#797985), con `motion-spring-sm`. El contenedor del icono permanece ghost (sin fill). El estado **Pressed** anima el background del contenedor (`color/bg/action-3-pressed` · #B4B4BF) con el mismo spring; icono y label no cambian. **Re-tap** en el item ya seleccionado no cambia sección ni dispara haptic; solo muestra pressed mientras el dedo está abajo.

El **FAB** central (opcional) es acción independiente: Enabled (`color/bg/action-accent-1-enabled` · #8F8F9C), Pressed (`color/bg/action-accent-1-pressed` · #404047), Focused (`color/bg/action-accent-1-enabled` · #8F8F9C; solo fill, sin ring). Ícono siempre `color/icon/on-action-accent-1` (#FFFFFF). Pressed con `motion-spring-sm`. Fixed y Floating son layout estático (sin animación al cambiar variante). De 3 a 5 items; el FAB puede removerse. Sin indicador deslizante. Sin `hover`. **Disabled** es instantáneo. `MediaQuery.disableAnimations` aplica estado final sin transición.

### Colores de estado (Figma)

| Subcomponente | Estado | Propiedad | Token | Valor |
|---------------|--------|-----------|-------|-------|
| Item | Enabled | icono, label | `color/icon/on-action-3`, `color/text/action-enabled` | #252529 |
| Item | Pressed | background contenedor | `color/bg/action-3-pressed` | #B4B4BF |
| Item | Selected | icono, label | `color/icon/accent`, `color/text/accent` | #797985 |
| FAB | Enabled | background | `color/bg/action-accent-1-enabled` | #8F8F9C |
| FAB | Pressed | background | `color/bg/action-accent-1-pressed` | #404047 |
| FAB | Focused | background | `color/bg/action-accent-1-enabled` | #8F8F9C |
| FAB | * | icono | `color/icon/on-action-accent-1` | #FFFFFF |

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tap item (nueva sección) | Nav item | — | — | — | — |
| 2 | Response | Selección | Nav item — icono | color | action-enabled | icon/accent | motion-spring-sm |
| 3 | Response | Selección | Nav item — label | color | action-enabled | text/accent | motion-spring-sm |
| 4 | Response | Deselección | Nav item previo — icono | color | icon/accent | action-enabled | motion-spring-sm |
| 5 | Response | Deselección | Nav item previo — label | color | text/accent | action-enabled | motion-spring-sm |
| 6 | Trigger | Tap down | Nav item | — | — | — | — |
| 7 | Response | Pressed | Nav item | backgroundColor contenedor | transparent | action-3-pressed | motion-spring-sm |
| 8 | Trigger | Tap up / cancel | Nav item | — | — | — | — |
| 9 | Response | Release pressed | Nav item | backgroundColor contenedor | pressed | transparent | motion-spring-sm |
| 10 | Trigger | Re-tap item seleccionado | Nav item | — | — | — | — |
| 11 | Response | Solo pressed | Nav item | backgroundColor | — | pressed → transparent | motion-spring-sm |
| 12 | Trigger | Focus visible (FAB) | FAB | — | — | — | — |
| 13 | Response | Focused | FAB | backgroundColor | — | action-accent-1-enabled | sin ring |
| 14 | Trigger | Tap down | FAB | — | — | — | — |
| 15 | Response | Pressed | FAB | backgroundColor | action-accent-1-enabled | action-accent-1-pressed | motion-spring-sm |
| 16 | Trigger | Item / FAB disabled | Nav item, FAB | — | — | — | — |
| 17 | Response | Disabled | Nav item, FAB | todas | — | final | instantáneo |
| 18 | Trigger | `disableAnimations` | items, FAB | — | — | — | — |
| 19 | Response | Sin animación | todas | — | — | final | instantáneo |

---

## Token Mapping

| Token semántico | mass | stiffness | damping | Uso |
|---|---|---|---|---|
| `motion-spring-sm` | 1.0 | 400 | 35 | Item: selección, pressed · FAB: pressed |

---

## Implementación Dart

```dart
final motion = context.motionTokens;
final spring = motion.springSm; // motion-spring-sm
final disableAnimations = MediaQuery.of(context).disableAnimations;

void onNavItemSelected(int index) {
  if (index == _currentIndex) return;

  context.haptics.trigger(GgdsHapticSemantic.selectionChange);

  if (disableAnimations) {
    _currentIndex = index;
    return;
  }

  _selectionController.animateWith(
    SpringSimulation(spring, _selectionController.value, index.toDouble(), 0),
  );
  _navigateToSection(index);
}

void onNavItemPressedChanged(bool pressed) {
  if (disableAnimations) {
    _pressedProgress = pressed ? 1.0 : 0.0;
    return;
  }
  _pressedController.animateWith(
    SpringSimulation(spring, _pressedController.value, pressed ? 1.0 : 0.0, 0),
  );
}

void onFabPressed() {
  context.haptics.trigger(
    widget.fabHaptic ?? GgdsHapticSemantic.actionPress,
  );
  widget.onFabAction?.call();
}

// FAB pressed — mismo spring en backgroundColor
void onFabPressedChanged(bool pressed) {
  if (disableAnimations) {
    _fabPressedProgress = pressed ? 1.0 : 0.0;
    return;
  }
  _fabPressedController.animateWith(
    SpringSimulation(spring, _fabPressedController.value, pressed ? 1.0 : 0.0, 0),
  );
}
```

---

## Haptics Specification

| Elemento | Token default | Trigger | Override |
|---|---|---|---|
| **Nav Item** | `haptic-selection-change` | `onTap` cuando `index != currentIndex` | no recomendado |
| **FAB** | `haptic-action-press` | `onPressed` | `none` \| `default` |
| **Re-tap mismo item** | — | — | sin haptic |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |
| `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |

---

## Implementación Haptics

```dart
void onNavItemTap(int index) {
  if (index != _currentIndex) {
    context.haptics.trigger(GgdsHapticSemantic.selectionChange);
    _navigateToSection(index);
  }
}

void onFabPressed() {
  final haptic = widget.fabHaptic ?? GgdsHapticSemantic.actionPress;
  if (haptic != GgdsHapticSemantic.none) {
    context.haptics.trigger(haptic);
  }
  widget.onFabAction?.call();
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Selección | `motion-spring-sm` — icono y label → `color/icon/accent` / `color/text/accent` (#797985) |
| Pressed | `motion-spring-sm` — background contenedor icono |
| FAB | `motion-spring-sm` + `haptic-action-press` |
| Navegación | tap en item cambia sección; re-tap sin haptic |
| Fixed / Floating | layout estático; sin animar reposicionamiento |
| Coherencia | mismo token que Tab item e Icon Button en GGDS |
| A11y | `disableAnimations` → salto instantáneo |
