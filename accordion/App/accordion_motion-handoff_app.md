# Motion Handoff — Accordion (App)

| | |
|---|---|
| **Componente** | Accordion |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token expand/collapse** | `motion-spring-sm` |
| **Token header pressed** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-30 |
| **Figma** | [Core — App Components](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=9840-1771) |

## Referencia visual

| Recurso | Uso |
|---------|-----|
| [Preview App (HTML)](./accordion_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. |

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token expand/collapse | `motion-curve-sm` · 150ms | `motion-spring-sm` |
| Propiedades slot | grid 0fr/1fr + inner opacity | height + inner opacity |
| Chevron | rotate 180deg | rotate 180deg (0.5 turns) |
| Header estados | hover + pressed + focus | pressed + focus (sin hover) |
| Type Default/Container | sin animación | sin animación |
| Viewport enter/exit | no | no |
| motion-exit | no | no |
| Haptic toggle | no aplica | `haptic-selection-change` (propio del accordion) |

Regla: no traducir ms de Web a duración fija en Flutter. El spring define el asentamiento.

**Tokens alineados con Dropdown (Web):** `motion-curve-sm` en Web ↔ `motion-spring-sm` en App para microinteracciones de ítem (hover/pressed en dropdown; expand/header en accordion).

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **Composición:** chevron → [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html).
- **Figma:** referencia visual del componente; motion se implementa en código.
- **QA final:** validar en dispositivo con `SpringDescription` / `flutter_animate` del design system.

---

## Motion Specification

El Accordion en App es un **ítem único**: header de 48dp (título + chevron) y slot colapsable. Tap en header alterna expanded/collapsed.

**Expand/collapse** anima en paralelo `height` del body (0 ↔ intrinsic), `opacity` del contenido interno y `rotation` del chevron (0 ↔ 180°) con `motion-spring-sm` (mass 1, stiffness 400, damping 35). Mismo token al colapsar.

**Header pressed** usa `motion-spring-sm` sobre escala/opacidad. Sin estado hover en App.

**Type Default vs Container:** cambio visual instantáneo (surface + radius 8dp; padding 16dp en slot expandido en Container).

Sin entrada/salida del viewport. Sin stagger.

**Haptics:** `haptic-selection-change` en toggle expand/collapse. Chevron → handoff [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html).

---

## Composición

| Pieza | Handoff |
|---|---|
| Expand/collapse + header pressed | este documento |
| Chevron | [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html) |

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tap header (expandir) | — | — | — | — | — | — | * | — |
| 2 | Haptic | Toggle expand | — | `haptic-selection-change` | — | — | selectionClick | 0 | 0 | — |
| 3 | Response | Slot expande | `AccordionBody` | `height` | 0 | intrinsic | `motion-spring-sm` | m 1 · s 400 · d 35 | * | settle |
| 4 | Response | Contenido visible | `body inner` | `opacity` | 0 | 1 | `motion-spring-sm` | m 1 · s 400 · d 35 | * | settle |
| 5 | Response | Chevron rota | `chevron` | `rotation` | 0 | 180deg | `motion-spring-sm` | m 1 · s 400 · d 35 | * | settle |
| 6 | Trigger | Tap header (colapsar) | — | — | — | — | — | — | * | — |
| 7 | Haptic | Toggle collapse | — | `haptic-selection-change` | — | — | selectionClick | 0 | * | — |
| 8 | Response | Reverse filas 3–5 | body, inner, chevron | — | expandido | colapsado | `motion-spring-sm` | m 1 · s 400 · d 35 | * | settle |
| 9 | Trigger | Press header | — | — | — | — | — | — | * | — |
| 10 | Response | Header pressed | `AccordionHeader` | scale, opacity | 1, 1 | ~0.985, ~0.92 | `motion-spring-sm` | m 1 · s 400 · d 35 | * | settle |
| 11 | Trigger | Cambio Type | — | — | — | — | — | — | * | — |
| 12 | Response | Variante visual | root | — | — | — | **sin animación** | — | — | — |

---

## Token Mapping

| Momento | Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---|---:|---:|---:|
| Expand / collapse | `motion-spring-sm` | `spring-standard-sm` | 1.0 | 400 | 35 |
| Header pressed | `motion-spring-sm` | `spring-standard-sm` | 1.0 | 400 | 35 |

---

## Implementación Dart (Flutter)

```dart
const motionSpringSm = SpringDescription(
  mass: 1.0,
  stiffness: 400,
  damping: 35,
);

final disableAnimations = MediaQuery.of(context).disableAnimations;

Future<void> toggleAccordion({required bool expanded}) async {
  if (disableAnimations) {
    accordionController.setExpanded(expanded);
    return;
  }
  await accordionController.animateExpanded(
    spring: motionSpringSm,
    expanded: expanded,
    bodyHeight: expanded ? const Tuple2(0.0, 1.0) : const Tuple2(1.0, 0.0),
    bodyOpacity: expanded ? const Tuple2(0.0, 1.0) : const Tuple2(1.0, 0.0),
    chevronTurns: expanded ? const Tuple2(0.0, 0.5) : const Tuple2(0.5, 0.0),
  );
}
```

---

## Haptics

| Acción | Token |
|---|---|
| Tap header (expand/collapse) | `haptic-selection-change` |
| Chevron press | ver handoff [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html) |
| Cambio Type | none |

```dart
void onHeaderTap() {
  context.haptics.trigger(GgdsHapticSemantic.selectionChange);
  toggleAccordion(expanded: !isExpanded);
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| vs Inline Message Expand | Mismas propiedades animadas; Accordion sin dismiss |
| Ítem único | No documentar accordion group en este handoff |
| Type | Default / Container: swap instantáneo |
| `disableAnimations` | Estado final sin spring |
| Haptics | `haptic-selection-change` en toggle; chevron → Icon Button |
| Figma | Solo referencia visual; motion en código |
