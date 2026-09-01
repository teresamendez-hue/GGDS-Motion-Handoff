# Motion Handoff — Footer (App)

| | |
|---|---|
| **Componente** | Footer |
| **Plataforma** | App (Flutter) |
| **Owner** | Juan Carlos De La Via Levy |
| **Design system** | GGDS |
| **Tokens semánticos** | `motion-spring-md` (reposicionamiento) · overlay instantáneo |
| **Categoría** | Guía / Default |
| **Fecha** | 2026-09-01 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./footer_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |

> Handoff **solo App**. No existe variante Web del Footer en GGDS.

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Teclado:** la propiedad booleana `keyboard` de Figma es solo visual. En desarrollo usar el teclado nativo del dispositivo vía `MediaQuery.viewInsets.bottom`.
- **Overlay:** si el Footer debe permanecer visible con el teclado abierto, usar `overlay: true`. Si arranca en `overlay: false`, cambiar a `overlay: true` en cuanto aparezca el teclado.
- **Contenido interno** (Button, Icon Button, Checkbox): motion y haptics documentados en sus handoffs respectivos.
- **Haptics:** el Footer como contenedor no dispara haptics. Los hijos delegan a Button, Icon Button o Checkbox según corresponda.
- **QA final:** validar en dispositivo o simulador con `SpringDescription` del design system y teclado nativo.

---

## Motion Specification

Footer en App es un contenedor de acciones fijo al fondo de la pantalla. Su motion se activa cuando el teclado nativo aparece o desaparece: el Footer se reposiciona para permanecer visible por encima del teclado.

El reposicionamiento vertical sigue `motion-spring-md`, el mismo token que Bottom Sheet para paneles que se desplazan desde abajo. En implementación idiomática de Flutter, **priorizar** `AnimatedPadding` o `Padding` con `MediaQuery.viewInsets.bottom`, que sincroniza con la curva del teclado del sistema (Apple HIG / Material). El spring documenta el contrato cuando se requiere control explícito.

Cuando el Footer pasa de Overlay/False a Overlay/True al abrir el teclado, la superficie — `background-color`, `box-shadow` y `border-radius` superior — se activa **de forma inmediata** al mismo tiempo que aparece el teclado (sin esperar el asentamiento del reposicionamiento).

Al cerrar el teclado, el overlay desaparece **de forma inmediata** al mismo tiempo que inicia el reposicionamiento hacia abajo. Solo el desplazamiento vertical usa `motion-spring-md`.

No se anima el teclado desde el Footer. Sin fade en el reposicionamiento: el componente no entra ni sale del viewport, solo se desplaza.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Teclado nativo aparece | `Footer` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Reposicionamiento | contenedor | `padding-bottom` / `translateY` | `0` | `viewInsets.bottom` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Response | Overlay activado (si `overlay` era false) | superficie | `backgroundColor`, `boxShadow`, `borderRadius` top | transparente, sin sombra, `0` | blanco, shadow-m, `20px` | none | instantáneo | 0 | 0 |
| 4 | Trigger | Teclado nativo desaparece | `Footer` | — | — | — | — | — | 0 | 0 |
| 5 | Response | Reposicionamiento inverso | contenedor | `padding-bottom` / `translateY` | `viewInsets.bottom` | `0` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 6 | Response | Overlay revertido (si volvió a false) | superficie | `backgroundColor`, `boxShadow`, `borderRadius` top | blanco, shadow-m, `20px` | transparente, sin sombra, `0` | none | instantáneo | 0 | 0 |

*Asentamiento perceptual definido por el spring.

---

## Token Mapping (App)

| Comportamiento | Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---|---:|---:|---:|
| Reposicionamiento con teclado | `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |
| Overlay (entrada y salida) | none | — | — | — | — |

---

## Implementación Flutter (Dart)

```dart
const _footerRepositionSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

// Preferido: delegar al sistema vía viewInsets
Widget buildFooter(BuildContext context) {
  final bottomInset = MediaQuery.viewInsetsOf(context).bottom;
  final overlay = _resolveOverlay(context, bottomInset > 0);

  return AnimatedPadding(
    duration: const Duration(milliseconds: 300), // curva del teclado nativo
    curve: Curves.easeOut,
    padding: EdgeInsets.only(bottom: bottomInset),
    child: _FooterSurface(
      overlay: overlay, // cambio instantáneo; sin spring en superficie
      child: _FooterActions(),
    ),
  );
}

// Fallback explícito con spring del token (cuando no se usa AnimatedPadding)
void animateFooterOffset({
  required AnimationController controller,
  required double target,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) {
    controller.value = target;
    return;
  }
  controller.animateWith(
    SpringSimulation(_footerRepositionSpring, controller.value, target, 0.0),
  );
}
```

---

## Haptics — delegación a hijos

| Componente hijo | Token | Trigger |
|---|---|---|
| Button / Icon Button | `haptic-action-press` | `onPressed` |
| Checkbox (variante data) | `haptic-selection-change` | `onChanged` |
| Footer (contenedor) | — | no aplica |

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | reposicionamiento del contenedor + transición overlay |
| Teclado | nativo del OS; no renderizar teclado propio |
| Overlay/True con teclado | obligatorio si el Footer debe permanecer visible |
| Fade | no usar en reposicionamiento |
| Haptics | solo en hijos; no en apertura/cierre de teclado |
| A11y | `disableAnimations` aplica estado final sin transición |
| Coherencia | `motion-spring-md` alineado con Bottom Sheet; overlay sin animación |
