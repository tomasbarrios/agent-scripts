# Adoptar docs-kit en otro proyecto / otra máquina

## En la otra máquina

### Opción A — Clonar agent-scripts (preferida si hay remote)

```bash
git clone <URL-de-agent-scripts> ~/Projects/agent-scripts
# o actualizar:
cd ~/Projects/agent-scripts && git pull
```

Si aún **no** hay remote: en esta máquina, `cd ~/Projects/agent-scripts && git remote add origin … && git push -u origin main`, luego clona en la otra.

### Opción B — Copiar solo el kit (USB / rsync / zip)

```bash
# Desde la máquina origen
rsync -a ~/Projects/agent-scripts/docs-kit/ user@otra:~/Projects/docs-kit/

# O zip
cd ~/Projects/agent-scripts && zip -r docs-kit-v0.1.0.zip docs-kit/
```

## En el proyecto destino

1. Copia las plantillas (ajusta la ruta raíz de docs; puede ser `docs/` o `mvp/docs/`):

```bash
KIT=~/Projects/agent-scripts/docs-kit   # o donde hayas puesto el kit
PROJ=~/path/al/proyecto
mkdir -p "$PROJ/docs"
cp -R "$KIT/templates/docs/." "$PROJ/docs/"
```

2. En el `docs/README.md` del proyecto, deja o añade:

```markdown
> **docs-kit:** v0.1.0 — método en `agent-scripts/docs-kit` (o ruta local del kit).
```

3. En el `AGENTS.md` del proyecto (opcional pero útil):

```markdown
Antes de crear un `.md` en docs/: clasificar con la tabla de `docs/README.md`
(pregunta → carpeta). Hechos del mundo externo → domain/; approach de app → rfcs/.
```

4. No copies ejemplos de doterra tal cual; usa [`examples/domain-vs-rfc.md`](examples/domain-vs-rfc.md) como patrón y escribe dominio/RFC del dominio **de ese** producto.

## Qué no hace el kit

- No impone MIW, git flow ni stack UI (eso es por proyecto / ADR).
- No sustituye el contenido: solo la **forma** de organizar.
- No auto-sincroniza: al subir `VERSION`, actualiza a mano el proyecto.

## Verificar adopción

- [ ] Existe `docs/README.md` con tabla pregunta → carpeta
- [ ] Existe `docs/rfcs/README.md` con estados
- [ ] `domain/` no menciona alcance MVP / implementación
- [ ] Hay al menos un par domain+RFC o el ejemplo adaptado
- [ ] El README del proyecto declara `docs-kit: v…`
