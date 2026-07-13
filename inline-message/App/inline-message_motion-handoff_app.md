# Motion Handoff — Inline Message (App)

| | |
|---|---|
| **Componente** | Inline Message |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token entrada/salida** | `motion-spring-md` |
| **Token expand** | `motion-spring-sm` |
| **Categoría** | Guía |
| **Fecha** | 2026-06-29 |
| **Figma** | [Core — App Components](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=3573-1186) |

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token entrada bloque | `motion-curve-md` · 300ms | `motion-spring-md` |
| Token salida bloque | `motion-exit` · 200ms | `motion-spring-md` (invertido) |
| Expand / collapse | `motion-curve-sm` · 150ms | `motion-spring-sm` |
| Propiedades bloque | fade + expand vertical (slot) | fade + expand vertical (height) |
| Variante Expand | sin X · sin dismiss | sin X · sin dismiss |
| Semánticas | sin cambio de token | sin cambio de token |
| Auto-dismiss | no | no |
| Scrim | no | no |

Regla: no traducir ms de Web a duración fija en Flutter. El spring define el asentamiento.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **Figma:** referencia visual del componente; **no se documenta ni implementa motion en Figma**.
- **QA final:** validar en dispositivo con `SpringDescription` / `flutter_animate` del design system.

---

## Motion Specification

El Inline Message en App transmite información contextual en el flujo de la pantalla. Panel 320px con badge semántico, título, descripción, Action Link opcional y variante Expand con chevron.

**Default:** incluye X de cierre. **Expand:** sin X, no es cerrable — solo expand/collapse.

Entrada y salida del bloque usan `motion-spring-md`: **fade + expand/collapse vertical** del slot (`opacity` + `height`), patrón equivalente a Compose `fadeIn + expandVertically`. **Sin translate**. La salida animada aplica **solo a Default**.

Expand/collapse usa `motion-spring-sm` en la región de descripción y rotación del chevron.

Variantes semánticas no cambian tokens.

**Haptics:** sin haptic en aparición. X y chevron son componentes compuestos — ver [Composición](#composición).

---

## Composición

| Pieza | Handoff |
|---|---|
| Bloque + motion del mensaje | este documento |
| Cerrar (X, solo Default) | [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html) |
| Chevron expand/collapse | [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html) |
| Action Link | [Link](../../link/App/link_motion-handoff_app.html) |

**Haptics:** el inline message no dispara haptic al aparecer. Press en X y chevron según handoff Icon Button. Action Link sin haptic.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Mensaje se muestra (programmatic) | — | — | — | — | — | — | 0 | — |
| 2 | Response | Bloque entra | `InlineMessage` | `opacity` | 0 | 1 | `motion-spring-md` | m 1 · s 300 · d 28 | 0 | settle |
| 3 | Response | Bloque entra | `InlineMessage` (slot) | `height` | 0 | intrinsic | `motion-spring-md` | m 1 · s 300 · d 28 | 0 | settle |
| 4 | Trigger | Tap en X (solo Default) | — | — | — | — | — | — | * | — |
| 5 | Response | Bloque sale (solo Default) | `InlineMessage` | `opacity` | 1 | 0 | `motion-spring-md` | m 1 · s 300 · d 28 | * | settle |
| 6 | Response | Bloque sale (solo Default) | `InlineMessage` (slot) | `height` | intrinsic | 0 | `motion-spring-md` | m 1 · s 300 · d 28 | * | settle |
| 7 | Trigger | Tap chevron (Expand) | — | — | — | — | — | — | * | — |
| 8 | Response | Details expande | `details` | `opacity`, `height` | colapsado | expandido | `motion-spring-sm` | m 1 · s 400 · d 35 | * | settle |
| 9 | Response | Chevron rota | `chevron` | `rotation` | 0 | 180deg | `motion-spring-sm` | m 1 · s 400 · d 35 | * | settle |
| 10 | Trigger | Tap chevron (colapsar) | — | — | — | — | — | — | * | — |
| 11 | Response | Reverse filas 8–9 | `details`, `chevron` | — | expandido | colapsado | `motion-spring-sm` | m 1 · s 400 · d 35 | * | settle |

---

## Token Mapping

| Momento | Token semántico | mass | stiffness | damping |
|---|---|---|---|---|
| Entrada / salida bloque | `motion-spring-md` | 1.0 | 300 | 28 |
| Expand / collapse | `motion-spring-sm` | 1.0 | 400 | 35 |

---

## Implementación Dart (Flutter)

```dart
const motionSpringMd = SpringDescription(
  mass: 1.0,
  stiffness: 300,
  damping: 28,
);

const motionSpringSm = SpringDescription(
  mass: 1.0,
  stiffness: 400,
  damping: 35,
);

final disableAnimations = MediaQuery.of(context).disableAnimations;

Future<void> showInlineMessage(InlineMessageData data) async {
  if (disableAnimations) {
    inlineMessageController.setVisible();
    return;
  }
  await inlineMessageController.animateIn(
    spring: motionSpringMd,
    opacity: const Tuple2(0.0, 1.0),
    height: const Tuple2(0.0, 1.0),
  );
}

Future<void> dismissInlineMessage() async {
  if (disableAnimations) {
    inlineMessageController.setHidden();
    return;
  }
  await inlineMessageController.animateOut(
    spring: motionSpringMd,
    opacity: const Tuple2(1.0, 0.0),
    height: const Tuple2(1.0, 0.0),
  );
}

Future<void> toggleExpand({required bool expanded}) async {
  if (disableAnimations) {
    inlineMessageController.setExpanded(expanded);
    return;
  }
  await inlineMessageController.animateDetails(
    spring: motionSpringSm,
    expanded: expanded,
    detailsOpacity: expanded ? const Tuple2(0.0, 1.0) : const Tuple2(1.0, 0.0),
    chevronTurns: expanded ? const Tuple2(0.0, 0.5) : const Tuple2(0.5, 0.0),
  );
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| vs Toast | Toast flotante: slide + `motion-spring-lg`. Inline en flujo: fade + expand + `motion-spring-md` |
| vs Coachmark | Mismos tokens de bloque; Coachmark ancla a target, Inline Message ocupa slot en flujo |
| `disableAnimations` | Estado final sin spring |
| Figma | Solo referencia visual; motion se implementa en código |
