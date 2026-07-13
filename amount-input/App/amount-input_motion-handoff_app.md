# Motion Handoff — Amount Input (App)

| | |
|---|---|
| **Componente** | Amount Input |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Tokens** | `motion-spring-md` · `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-30 |
| **Figma** | [Core — App Components](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=7020-1468) |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./amount-input_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **Figma:** referencia visual del componente; skeleton no documentado (ver Recomendaciones).
- **Composición:** el icono de visibilidad (eye) es un [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html) — press, focus y estados del botón están en su handoff; este documento cubre el field (tipografía, crossfade del valor enmascarado, badge y helper).
- **QA final:** validar en dispositivo o simulador con `flutter_animate` / `SpringDescription` del design system.

---

## Motion Specification

Amount Input en App es un campo de monto monetario con prefijo de moneda estático, icono de visibilidad y slots opcionales de badge y helper text. No aplica animaciones de entrada/salida de viewport — solo estados internos.

Estados cubiertos: `Enabled`, `Focused`, `Typing`, `Filled`, `Muted` (valor enmascarado con bullets), `Disabled`.

El foco y el tipeo animan el **color de tipografía** del monto (`neutral-alt` → `on-input`) con `motion-spring-md`, en paridad con Field Text pero **sin borders ni ring** — el componente en Figma no usa contorno. El prefijo de moneda (`Gs.`, `US$`, `R$` o ninguno) es estático: cambios de variante de currency o size no animan.

El valor enmascarado (visible ↔ bullets `••••`) usa crossfade con `motion-spring-sm`. Badge y helper text entran/salen con `motion-spring-sm` (opacity + height). Disabled aplica opacity final de forma instantánea.

El toggle de visibilidad se implementa con [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html); el swap de ícono (eye open/closed) es instantáneo en este handoff.

---

## Composición

| Pieza | Handoff |
|---|---|
| Field de monto (tipografía, máscara, badge, helper) | este documento |
| Icono de visibilidad (eye) | [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html) |

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | `AmountInput` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Typography color | `Amount` text | `color` | `#6C6C78` (neutral-alt) | `#252529` (on-input) | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Trigger | Input changed / typing | `AmountInput` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Typography color | `Amount` text | `color` | neutral-alt | on-input | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 5 | Trigger | Blur (empty) | `AmountInput` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Typography color | `Amount` text | `color` | on-input | neutral-alt | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 7 | Trigger | Visibility toggle (eye) | `IconButton` | — | — | — | — | ver handoff Icon Button | 0 | 0 |
| 8 | Response | Value mask crossfade | amount text layer | `opacity` | visible / bullets | bullets / visible | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle† |
| 9 | Response | Icon asset swap | eye icon | `asset` | open / closed | closed / open | none | instantáneo; estados del botón en handoff Icon Button | 0 | 0 |
| 10 | Trigger | Badge show | `TextBadge` slot | — | — | — | — | — | 0 | 0 |
| 11 | Response | Badge enter | `TextBadge` | `opacity`, `height` | `0`, `0` | `1`, intrinsic | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle† |
| 12 | Trigger | Badge hide | `TextBadge` slot | — | — | — | — | — | 0 | 0 |
| 13 | Response | Badge exit | `TextBadge` | `opacity`, `height` | `1`, intrinsic | `0`, `0` | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle† |
| 14 | Trigger | Helper show | `HelperText` slot | — | — | — | — | — | 0 | 0 |
| 15 | Response | Helper enter | `HelperText` | `opacity`, `height` | `0`, `0` | `1`, intrinsic | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle† |
| 16 | Trigger | Helper hide | `HelperText` slot | — | — | — | — | — | 0 | 0 |
| 17 | Response | Helper exit | `HelperText` | `opacity`, `height` | `1`, intrinsic | `0`, `0` | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle† |
| 18 | Trigger | Disabled | `AmountInput` | — | — | — | — | — | 0 | 0 |
| 19 | Response | Disabled applied | root | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |
| 20 | Trigger | Currency / Size change | `AmountInput` | — | — | — | — | — | 0 | 0 |
| 21 | Response | Variant swap | prefix / typography / row height | — | — | — | none | sin transición | 0 | 0 |

\*Asentamiento perceptual definido por el spring (`motion-spring-md`).

†Asentamiento perceptual definido por el spring (`motion-spring-sm`).

Filas 7 y 9: motion del [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html) enlazado. Este documento cubre el crossfade del valor enmascarado (fila 8).

---

## Token Mapping (App)

| Momento | Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---|---:|---:|---:|
| Focus · typing · filled | `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |
| Value mask crossfade · badge · helper | `motion-spring-sm` | `spring-standard-sm` | 1.0 | 400 | 35 |
| Disabled · currency · size | none | — | — | — | — |

> El icono de visibilidad es un [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html) compuesto — no incluir su motion en este mapping.

---

## Implementación Flutter (Dart)

```dart
// Componer GgdsIconButton para el eye — ver handoff Icon Button.
// Este snippet cubre solo el field: tipografía activa, crossfade de máscara y slots.

const _amountInputSpringMd = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

const _amountInputSpringSm = SpringDescription(
  mass: 1.0,
  stiffness: 400.0,
  damping: 35.0,
);

void animateAmountActive({
  required AnimationController controller,
  required bool active,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) return;
  controller.animateWith(
    SpringSimulation(
      _amountInputSpringMd,
      active ? 0.0 : 1.0,
      active ? 1.0 : 0.0,
      0.0,
    ),
  );
}

void animateAmountMaskCrossfade({
  required AnimationController controller,
  required bool masked,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) {
    controller.value = masked ? 1.0 : 0.0;
    return;
  }
  controller.animateWith(
    SpringSimulation(
      _amountInputSpringSm,
      masked ? 0.0 : 1.0,
      masked ? 1.0 : 0.0,
      0.0,
    ),
  );
}

void animateSlotVisibility({
  required AnimationController controller,
  required bool show,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) {
    controller.value = show ? 1.0 : 0.0;
    return;
  }
  controller.animateWith(
    SpringSimulation(
      _amountInputSpringSm,
      show ? 0.0 : 1.0,
      show ? 1.0 : 0.0,
      0.0,
    ),
  );
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo estados internos |
| Hover | no aplica en App |
| Currency / Size | sin transición al cambiar variante |
| Muted (estado Figma) | cada dígito → `•`; separadores (`.`, `,`) visibles; crossfade del valor con `motion-spring-sm` — no confundir con motion del Icon Button |
| Composición | eye → [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html) |
| Skeleton | omitido del handoff — el shimmer de Figma no se replica fielmente en el preview HTML |
| Disabled | sin transición |
| A11y | `disableAnimations` aplica estado final |
| Coherencia | color de tipografía alineado con intención de Field Text (`motion-spring-md`); sin borders |
