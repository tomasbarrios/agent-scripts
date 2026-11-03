# Vendor: steveruizok / product-description

Una sola skill (gist plano) que arma un **repo de spec de comportamiento**
outside-in: describe qué hace un producto feature a feature, event a event,
leyendo código y tests, lo verifica contra el producto vivo y junta lo que se
ve mal en un triage.

## Procedencia

| Campo | Valor |
|---|---|
| Gist | <https://gist.github.com/steveruizok/83ae5c53f2784ebf8f5fe0a3fb94480f> |
| Acceso | Submódulo `vendor/steveruizok-product-description` (gist, dentro de este repo) |
| Fecha de alta | previa (ver historia de `.gitmodules`) |

## Estructura del repo

Es un gist **plano**: `SKILL.md` en la raíz, no `skills/<name>/`. Por eso no se
copia a `skills/` ni se corre `install.sh` sin aprobación.

## Skill que ofrece (1)

| Skill | Qué hace |
|---|---|
| `product-description` | Spec de comportamiento outside-in a partir de código y tests, verificada contra el producto corriendo. El output es otro directorio/repo, no una skill. |

## Qué ya se instaló desde acá

- `product-description` — copiada/adaptada a `skills/` de este repo (ver su
  fila en [`../skills-catalogo.md`](../skills-catalogo.md); se invoca a mano,
  `disable-model-invocation`).

## Actualizar el submódulo

```bash
git submodule update --remote vendor/steveruizok-product-description
```

Deja el bump de commit en un commit propio.
