# dinas-wms-contracts

Fuente única de verdad del contrato OpenAPI del proyecto Dinas WMS.

Este repo se consume como **submódulo git** desde los cinco repos del
proyecto, montado en `contracts/`. La ruta final en cada repo es
`contracts/openapi.yaml`, la misma de siempre.

## Regla de gobierno — quién puede escribir acá

**Solo el Arquitecto redacta el contrato, y solo Camilo commitea acá.**

Ningún agente de ningún repo modifica `openapi.yaml`, ni en este repo
ni en su copia local del submódulo. Ni para aplicar un cambio ya
acordado, ni para "solo confirmar" algo. Si un agente encuentra un
vacío, una contradicción o un error en el contrato, **lo señala** — no
lo resuelve editando el archivo.

El flujo es siempre:

1. El Arquitecto redacta el archivo **completo** (nunca un diff
   narrado — un diff pegado encima de un archivo real ya destruyó el
   contrato una vez, 7249 líneas → 123).
2. Camilo lo commitea acá y le pone el tag de versión.
3. Cada repo actualiza su puntero de submódulo a ese tag.

## Por qué existe este repo

Antes, cada repo tenía su propia copia sincronizada a mano. Eso
produjo tres incidentes reales:

- Un diff narrado pegado **encima** del contrato completo, dejándolo
  en 123 líneas. Se recuperó desde HEAD.
- Una copia intermedia distribuida antes de que el Arquitecto cerrara
  su última corrección — un repo quedó con texto inconsistente.
- Un repo con la copia commiteada vieja mientras su working tree tenía
  la correcta, sin que nadie lo notara.

El patrón común no es descuido: **pegar contenido no es verificable,
traer un commit sí**. Un submódulo anclado a un tag hace que "¿tengo
el contrato correcto?" sea una pregunta con respuesta exacta.

## Versionado

Cada versión del contrato lleva un tag: `v0.29.0`, `v0.30.0`, etc. El
tag coincide con el campo `info.version` dentro del propio
`openapi.yaml`.

Los repos consumidores anclan a un tag, no a una rama. Actualizar el
contrato en un repo es un acto deliberado (mover el puntero del
submódulo y commitearlo), nunca algo que pase solo.

## Verificación

Cada entrega del Arquitecto viene con su sha256 normalizado a LF.
Para comprobar:

    tr -d '\r' < openapi.yaml | sha256sum

Si tu checkout dejó el archivo con CRLF, el sha crudo va a diferir —
el que vale es el normalizado. Esta trampa ya costó tiempo antes.

## Estado actual

- Versión: **v0.29.0**
- sha256 (LF): `7aacd66cfbd5cdcad1e641d5a0867348a5180b0715d3248eeeebe35953343b54`
- 7467 líneas, 95 paths, 105 schemas

## Repos que lo consumen

- `dinas-wms-middleware`
- `dinas-wms-dashboard`
- `dinas-wms-app-sales`
- `dinas-wms-app-bodega`
- `dinas-wms-app-driver`

`dinas-wms-sap-sync` no consume el contrato del WMS: habla con SAP
Service Layer, no con el middleware.
