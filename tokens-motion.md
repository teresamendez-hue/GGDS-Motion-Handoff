# Motion System GGDS — Tokens (Motion)

Fuente de verdad para handoffs de motion. **Nunca usar valores de memoria** — leer este archivo antes de generar cualquier `.md`.

> Si un token no aparece aquí, no existe en el sistema. Notificar al diseñador antes de generar el handoff.

---

## Tokens primitivos — Curvas Bezier (Web)

| Token | Valor CSS |
|---|---|
| `easing-linear` | `cubic-bezier(0, 0, 1, 1)` |
| `easing-standard` | `cubic-bezier(0.4, 0, 0.2, 1)` |
| `easing-accelerate` | `cubic-bezier(0.4, 0, 1, 1)` |
| `easing-decelerate` | `cubic-bezier(0.0, 0, 0.2, 1)` |
| `easing-emphasis` | `cubic-bezier(0.2, 0, 0, 1)` |
| `easing-back` | `cubic-bezier(0.34, 1.56, 0.64, 1)` |

---

## Tokens primitivos — Duración (Web)

| Token | Valor |
|---|---|
| `duration-50` | 50ms |
| `duration-100` | 100ms |
| `duration-150` | 150ms |
| `duration-200` | 200ms |
| `duration-300` | 300ms |
| `duration-400` | 400ms |
| `duration-500` | 500ms |
| `duration-600` | 600ms |
| `duration-700` | 700ms |
| `duration-800` | 800ms |

---

## Tokens semánticos — Curvas Bezier (Web)

| Categoría | Token semántico | Token primitivo curva | Token primitivo duración | Comportamiento |
|---|---|---|---|---|
| Default | `motion-curve-sm` | `easing-standard` | `duration-150` | Sutil y natural |
| Guía | `motion-curve-md` | `easing-decelerate` | `duration-300` | Flujo suave y guiado |
| Comunicación | `motion-curve-lg` | `easing-emphasis` | `duration-500` | Presencia amable |
| Interactivos | `motion-curve-xl` | `easing-standard` | `duration-700` | Ritmo pausado y confiable |
| Carga continua | `motion-curve-spin` | `easing-linear` | `duration-800` | Rotación lineal en loop (spinner) |
| Salida | `motion-exit` | `easing-accelerate` | *(ver tabla de pares)* | Retiro rápido; el usuario ya decidió cerrar |

> `motion-exit` es el **único** token semántico de salida en Web. No crear variantes por categoría (`motion-exit-md`, etc.). La duración se resuelve según el token de entrada del componente.

### Pares entrada → salida (Web)

| Token entrada | Duración entrada | Token salida | Primitivo curva salida | Duración salida |
|---|---|---|---|---|
| `motion-curve-sm` | `duration-150` · 150ms | `motion-exit` | `easing-accelerate` | `duration-100` · 100ms |
| `motion-curve-md` | `duration-300` · 300ms | `motion-exit` | `easing-accelerate` | `duration-200` · 200ms |
| `motion-curve-lg` | `duration-500` · 500ms | `motion-exit` | `easing-accelerate` | `duration-300` · 300ms |
| `motion-curve-xl` | `duration-700` · 700ms | `motion-exit` | `easing-accelerate` | `duration-400` · 400ms |

La duración de salida es ~40% menor que la de entrada del mismo componente. El dev consume **`motion-exit`** en código, no `easing-accelerate` directo.

---

## Tokens primitivos — Spring (App / Flutter)

| Token | Valor |
|---|---|
| `mass-1` | 1.0 |
| `stiffness-100` | 100 |
| `stiffness-200` | 200 |
| `stiffness-300` | 300 |
| `stiffness-400` | 400 |
| `damping-20` | 20 |
| `damping-28` | 28 |
| `damping-30` | 30 |
| `damping-35` | 35 |

---

## Tokens semánticos — Spring (App / Flutter)

| Categoría | Token semántico | mass | stiffness | damping | Comportamiento |
|---|---|---|---|---|---|
| Default | `motion-spring-sm` | `mass-1` → 1.0 | `stiffness-400` → 400 | `damping-35` → 35 | Sutil y natural |
| Guía | `motion-spring-md` | `mass-1` → 1.0 | `stiffness-300` → 300 | `damping-28` → 28 | Flujo suave y guiado |
| Comunicación | `motion-spring-lg` | `mass-1` → 1.0 | `stiffness-200` → 200 | `damping-30` → 30 | Presencia amable |
| Interactivos | `motion-spring-xl` | `mass-1` → 1.0 | `stiffness-100` → 100 | `damping-20` → 20 | Ritmo pausado y confiable |
| Carga continua | `motion-curve-spin` | — | — | — | Rotación lineal en loop · `Curves.linear` · 800ms / vuelta · no usar spring |

> `motion-curve-spin` en App no es un spring: mapea a `Curves.linear` + `Duration(milliseconds: 800)` + `AnimationController.repeat()`.

> Los springs **no** tienen token de duración. El tiempo de asentamiento lo determinan mass, stiffness y damping.

### Salida (App) — criterio

Los springs no diferencian curva de entrada/salida. Para salidas que deban sentirse más rápidas que la entrada, usar `Curves.easeIn` con duración explícita (~40% menor que la entrada estimada) o parámetros de spring documentados en el handoff del componente.

---

## Resolución rápida — Web

| Token semántico | Curva CSS | Duración |
|---|---|---|
| `motion-curve-sm` | `cubic-bezier(0.4, 0, 0.2, 1)` | 150ms (entrada) |
| `motion-curve-md` | `cubic-bezier(0.0, 0, 0.2, 1)` | 300ms (entrada) |
| `motion-curve-lg` | `cubic-bezier(0.2, 0, 0, 1)` | 500ms (entrada) |
| `motion-curve-xl` | `cubic-bezier(0.4, 0, 0.2, 1)` | 700ms (entrada) |
| `motion-curve-spin` | `cubic-bezier(0, 0, 1, 1)` | 800ms / revolución (loop) |
| `motion-exit` | `cubic-bezier(0.4, 0, 1, 1)` | ver tabla de pares |

---

## Resolución rápida — App

| Token semántico | flutter_animate / SpringDescription |
|---|---|
| `motion-spring-sm` | mass: 1.0, stiffness: 400, damping: 35 |
| `motion-spring-md` | mass: 1.0, stiffness: 300, damping: 28 |
| `motion-spring-lg` | mass: 1.0, stiffness: 200, damping: 30 |
| `motion-spring-xl` | mass: 1.0, stiffness: 100, damping: 20 |
| `motion-curve-spin` | `Curves.linear` · `Duration(milliseconds: 800)` · `AnimationController.repeat()` |

---

## Restricciones del sistema

- **Nunca** usar `easing-back` en toasts, modales informativos, drawers, snackbars ni feedback de estado.
- **Nunca** pasar `easing-accelerate` ni valores CSS crudos en salida — usar siempre el semántico `motion-exit`.
- **Nunca** pasar valores CSS o numéricos crudos al componente sin cadena semántica → primitivo → valor.
- Si un componente no encaja en ninguna categoría, crear un nuevo token semántico antes de generar el handoff.
