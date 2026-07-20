# Ejemplo: domain vs RFC

Patrón (anonimizado). Caso real completo: `doterra` → `mvp/docs/domain/semana-de-duos.md` + `mvp/docs/rfcs/semana-de-duos-cotizacion.md`.

## Domain (hechos externos)

> Una promoción estacional vende una **caja** (SKU propio) y **pares** diarios (SKU propio).  
> Precio = suma de precios de los ítems que se pagan.  
> Lo gratis no aporta PV.  
> La caja añade un regalo extra que no viene en los pares sueltos.

No habla de carrito, UI, ni MVP.

## RFC (approach nuestro)

> **Estado:** `aceptado`  
> Contraparte: `domain/….md`

Decisión: la caja/par es un **producto** en catálogo; al cotizar entra **solo ese SKU** al carrito; el desglose (pagado vs regalo) es una **capa que no suma**.

Trade-off: armar a mano los ítems sin el SKU de oferta es otro flujo.

## Test rápido

Si borras el nombre de tu app del párrafo y sigue siendo verdad en el mundo real → **domain**.  
Si solo tiene sentido dentro de tu código/UX → **RFC** (o `architecture/current`).
