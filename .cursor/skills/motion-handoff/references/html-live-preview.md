# HTML Live Preview — Motion Handoff GGDS

Guía para armar el preview interactivo en `*_motion-handoff_[plataforma].html`.

## Estructura obligatoria

1. **Stage** — mock visual del componente en contexto (no un rectángulo genérico)
2. **Controles** — según tipo de componente
3. **Animación con los tokens reales** del handoff
4. **Readout** — actualizado al interactuar o al disparar la animación

Siempre incluir bajo el título del preview:

> Motion lab — no es el componente de la librería. Validá motion y tokens; la UI final está en Figma.

## Dos modos de preview

| Modo | Componentes | Controles en el HTML |
|---|---|---|
| **Viewport** | Modal, Sidesheet, Toast, Drawer | Entrada · Salida · Reset · reduced-motion |
| **Estado interno** | Button, Icon Button, Checkbox, Radio, Switch, Toggle | **Interactuar con el componente** (click/tap y Tab). Solo **checkboxes** auxiliares: Loading, Disabled, reduced-motion. **Prohibido** botonera Default/Hover/Pressed/Focus y **prohibido** control manual `Selected` en checkbox/radio |

## Viewport (entrada/salida)

Referencia canónica: `references/history/sidesheet/Web/sidesheet_motion-handoff_web.html`

- Stage con `position: relative; overflow: hidden`
- Elemento principal + scrim si aplica
- Botones: Entrada, Salida, Reset
- Checkbox `prefers-reduced-motion`
- Animar con curvas y duraciones del Token Mapping (Web) o spring simulado (App)
- Readout: estado (`hidden`, `entering`, `exiting`) + token de entrada/salida

## Estado interno (Web)

Referencia canónica: `references/history/button/Web/button_motion-handoff_web.html`

- Usar `:hover`, `:active`, `:focus-visible` con `transition` y tokens del handoff
- Checkboxes auxiliares: Loading, Disabled, Skeleton (según componente)
- Copy del hint: *"Los checkboxes de abajo solo simulan Loading y Disabled."* — usar **checkboxes**, nunca *toggles*
- Readout: estado actual (`default`, `pressed`, `focus`, `loading`, `disabled`)
- `prefers-reduced-motion`: desactivar todas las transiciones

## Estado interno (App)

Referencia canónica: `references/history/button/App/button_motion-handoff_app.html`

- **No modelar `hover` en App**
- Estados: `pressed`, `focus`, `loading`, `disabled`, `skeleton`
- Selección en checkbox/radio: por interacción directa (click/tap o teclado), no por checkbox auxiliar `Selected`
- Animar con `springTo()` y parámetros del token (`mass`, `stiffness`, `damping`)
- Callout obligatorio:

> Este preview es una representación visual del motion, no una simulación física real. El navegador no soporta springs de Flutter. Los valores que el dev implementa son los parámetros de spring del Token Mapping.

- Checkbox auxiliar: `disableAnimations (sin animación)` en lugar de `prefers-reduced-motion`

## springTo() — simulación App en JS

```javascript
function springTo({ from, to, mass, stiffness, damping, onUpdate, onComplete }) {
  const w0 = Math.sqrt(stiffness / mass);
  const zeta = damping / (2 * Math.sqrt(stiffness * mass));
  let x = from, v = 0, t = 0;
  const dt = 1 / 60;
  function step() {
    const a = -stiffness * (x - to) - damping * v;
    v += a * dt;
    x += v * dt;
    t += dt;
    onUpdate(x);
    if (Math.abs(x - to) < 0.001 && Math.abs(v) < 0.001) {
      onUpdate(to);
      onComplete?.();
      return;
    }
    requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}
```

## Readout

| Modo | Qué mostrar |
|---|---|
| Viewport | Token de entrada/salida según animación activa |
| Estado interno (mismo token en todos los estados) | Token **una vez** (estático) + estado dinámico al interactuar |
| Estado interno Web | `hover / pressed / focus / loading / disabled` |
| Estado interno App | `pressed / focus / loading / disabled` (sin hover) |

## Token Mapping en HTML

- **MD:** Timeline + Token Mapping completos
- **HTML — sección Token Mapping:** card compacta con cadena semántica → primitivo → valor
- **HTML — Timeline completa:** opcional si el preview es interactivo y el token es único

## Accesibilidad en preview

- Web: checkbox `prefers-reduced-motion` + clase `.reduced-motion` que desactiva transiciones
- App: checkbox `disableAnimations` con el mismo efecto
