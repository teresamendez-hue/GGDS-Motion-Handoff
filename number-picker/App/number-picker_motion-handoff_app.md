# Motion Handoff — Number Picker (App)

| | |
|---|---|
| **Componente** | Number Picker |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuen David Benitez |
| **Design system** | GGDS |
| **Tokens** | `motion-spring-md` (campo) · `motion-spring-sm` (acciones/valor) |
| **Categoría** | Default |
| **Fecha** | 2026-08-26 |
| **Figma** | [Core — App Components · node 13041:1553](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/branch/8Dk0SpxqvWWQZuq9tko3mU/Core---App-Components?node-id=13041-1553) |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./number-picker_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |

---

## Paridad Web ↔ App

Handoff **solo App**. No existe companion Web para este componente.

| Aspecto | App |
|---------|-----|
| Tipo de motion | Estado interno (sin viewport) |
| Campo (focus/error) | `motion-spring-md` · m:1.0 · k:300 · d:28 |
| Acciones +/- y slide de valor | `motion-spring-sm` · m:1.0 · k:400 · d:35 |
| Tipeo directo | sin motion |
| Disabled | instantáneo (`disableAnimations`) |

Regla: no traducir duraciones fijas en ms. El spring define el asentamiento.

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **QA final:** validar en dispositivo o simulador con `flutter_animate` / `SpringDescription` del design system.
- **Widgetbook:** comportamiento funcional del componente vive en la librería; este handoff documenta solo motion.

---

## Motion Specification

Number Picker en App anima estados internos del campo y de los botones +/-. No tiene animación de entrada/salida de pantalla.

Estados cubiertos en Figma: `Enabled`, `Focused`, `Disabled`, `Error` (+ variantes focused/error). Skeleton documentado como fuera de contrato de motion.

**Geometría del control (preview alineado a Figma):** tira de input **110×44px**; action icons **44×44px** (− / +); focus ring externo **2px**, radio **10px**, inset **−2px** respecto al borde del input; `overflow: hidden` en el contenedor para que el pressed de los botones respete las esquinas del borde (**8px**).

El focus ring y la transición a error usan `motion-spring-md`, alineado a Field Text y Field Prefix.

**Pressed en +/-:** solo `backgroundColor` (`action-3-enabled` → `action-3-pressed` / `rgba(37,37,41,0.06)`). **Sin `scale`** — el feedback debe llenar el área del botón y encajar con el borde del input, no encogerse.

**Slide de valor (+/−):** crossfade sincronizado outgoing/incoming con un solo progreso spring (`opacity` + `translateY` **±6px**). Incremento: saliente sube, entrante desde abajo. Decremento: invertido. **Sin motion** en tipeo directo.

En `Disabled`, estado final instantáneo en el contenedor del input. En min/max, el botón opuesto queda deshabilitado **sin fondo** — solo cambia el color del ícono (`on-disabled`) — sin animación adicional ni haptic.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | `NumberPicker` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Focus ring | focus ring | `opacity`, `borderColor` | `0`, — | `1`, focus/error-focus | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Trigger | Tap down (+/− habilitado) | `ActionIcon` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Pressed | `ActionIcon` (44×44) | `backgroundColor` | enabled / transparent | pressed | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle* |
| 5 | Trigger | Value changed (+/−) | `NumberPicker` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Value slide | value text | `opacity`, `translateY` | outgoing visible · `0` | outgoing fade · ±6px; incoming fade in · `0` | `motion-spring-sm` | m:1.0, k:400, d:35 · progreso único | 0 | settle* |
| 7 | Trigger | Direct typing | value input | — | — | — | — | — | 0 | 0 |
| 8 | Response | Typing | value text | — | — | — | none | instantáneo | 0 | 0 |
| 9 | Trigger | Validation failed | `NumberPicker` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Error feedback | input border, helper, ring | `borderColor`, `color` | default | error | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 11 | Trigger | Disabled | `NumberPicker` | — | — | — | — | — | 0 | 0 |
| 12 | Response | Disabled applied | root / input wrap | `opacity`, `backgroundColor`, `borderColor` | enabled | disabled del campo | none | instantáneo | 0 | 0 |
| 13 | Response | ActionIcon disabled (min/max) | `ActionIcon` | `color` | on-input | on-disabled | none | instantáneo · sin `backgroundColor` | 0 | 0 |

\*Asentamiento perceptual definido por el spring.

---

## Token Mapping (App)

| Uso | Token semántico | mass | stiffness | damping |
|---|---|---:|---:|---:|
| Focus ring · borde · error | `motion-spring-md` | 1.0 | 300 | 28 |
| Action icon pressed · slide de valor | `motion-spring-sm` | 1.0 | 400 | 35 |
| Disabled · tipeo directo | none | — | — | — |

---

## Implementación Flutter (Dart)

```dart
const _fieldSpring = SpringDescription(mass: 1.0, stiffness: 300.0, damping: 28.0);
const _actionSpring = SpringDescription(mass: 1.0, stiffness: 400.0, damping: 35.0);

final disableAnimations = MediaQuery.of(context).disableAnimations;

void onFocusChanged(AnimationController ringController, bool focused) {
  if (disableAnimations) {
    ringController.value = focused ? 1.0 : 0.0;
    return;
  }
  ringController.animateWith(
    SpringSimulation(_fieldSpring, ringController.value, focused ? 1.0 : 0.0, 0.0),
  );
}

void onActionPressed({required bool pressed, required ValueNotifier<Color?> bg}) {
  if (disableAnimations) {
    bg.value = pressed ? kAction3Pressed : kAction3Enabled;
    return;
  }
  // backgroundColor only — sin scale; el hit target permanece 44×44
  bg.value = pressed ? kAction3Pressed : kAction3Enabled;
}

void onValueChangedByButton({
  required AnimationController slideController,
  required bool increment,
}) {
  if (disableAnimations) {
    slideController.value = 1.0;
    return;
  }
  // Un solo SpringSimulation: t 0→1
  // outgoing: opacity 1→0, translateY 0→±6
  // incoming: opacity 0→1, translateY ∓6→0
  slideController.animateWith(
    SpringSimulation(_actionSpring, 0.0, 1.0, 0.0),
  );
}

// Error: migrar borderColor + helper con _fieldSpring.
// Disabled: opacity final instantánea, sin spring.
// Direct typing: actualizar TextEditingController sin AnimationController.
```

---

## Haptics Specification

| Campo | Valor |
|---|---|
| **Requiere haptics** | Sí (solo botones +/-) |
| **Token default** | `haptic-selection-change` |
| **Trigger** | `onChanged` exitoso al incrementar/decrementar |
| **Override** | no recomendado |
| **Sin haptic** | tipeo directo; tap en botón deshabilitado (min/max) |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |

---

## Implementación Haptics

```dart
void onValueChangedByButton(int newValue) {
  context.haptics.trigger(GgdsHapticSemantic.selectionChange); // haptic-selection-change
  // actualizar valor + motion slide
}

// onChanged por TextField: NO disparar haptic
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo estados internos |
| Geometría | input 110×44px · action icons 44×44px · focus ring inset −2px |
| Pressed +/- | solo backgroundColor; sin scale; esquinas alineadas al borde del input |
| ActionIcon disabled | solo color del ícono; sin fondo en el botón |
| Hover | no aplica en App |
| Slide de valor | solo vía +/-; crossfade sincronizado; ±6px; no en tipeo |
| Overshoot | no usar en campo informativo |
| Disabled / min-max | sin transición |
| Skeleton | fuera del contrato de motion |
| A11y | `disableAnimations` aplica estado final |
| Preview HTML | debe reflejar este contrato; cambios visuales → actualizar MD |
| Coherencia | Field Text (campo) + Switch (haptic) |
