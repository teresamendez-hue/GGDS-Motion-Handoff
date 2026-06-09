# Motion Handoff GGDS

Repositorio de trabajo para generar y mantener handoffs de motion de componentes del GGDS en Web y App.

## Objetivo

- Documentar de forma clara como se comporta el motion de cada componente.
- Alinear diseño y desarrollo con tokens semanticos y primitivos.
- Entregar previews interactivos para validar estados, ritmo y accesibilidad.

## Estructura del proyecto

Cada componente vive en su carpeta y se separa por plataforma:

```text
motion/
├── [componente]/
│   ├── Web/
│   │   ├── [componente]_motion-handoff_web.html
│   │   ├── [componente]_motion-handoff_web.md
│   │   └── [componente]_motion-spec_web.md
│   └── App/
│       ├── [componente]_motion-handoff_app.html
│       ├── [componente]_motion-handoff_app.md
│       └── [componente]_motion-spec_app.md
├── tokens.md
├── motion-tokens-gds-2.0-tabla.csv
└── guia-ops-motion-handoff.md
```

## Componentes trabajados

- `button`
- `sidesheet`
- `checkbox`
- `radio-button`
- `box-selector`

## Entregables por componente

- `motion-handoff_*.html`: preview interactivo para validacion visual.
- `motion-handoff_*.md`: resumen funcional para handoff/ticket.
- `motion-spec_*.md`: especificacion tecnica de tokens y reglas.

## Accesibilidad

- Web: usar `prefers-reduced-motion` para validar estado final sin transiciones.
- App: usar `disableAnimations` para simular comportamiento accesible equivalente.

## Convenciones

- Mantener token semantico visible en la pill.
- Describir composicion completa del token en `Token Mapping`.
- No sobrescribir decisiones historicas sin registrar el cambio.
- Priorizar estados reales del componente sobre decoraciones visuales.

## Historial y trazabilidad

El historial interno del skill se guarda en:

- `~/.cursor/skills/motion-handoff/references/history/`

Cada snapshot se versiona por componente y fecha (`YYYY-MM-DD`).

