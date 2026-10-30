# gflow — Google Flow (Veo/Imagen) desde la terminal

Entorno aislado de [`gflow-cli`](https://github.com/ffroliva/gflow-cli) (MIT, v0.92.0 fijada). Sirve para generar imágenes y vídeos con Google Flow sin abrir la web: iconos, capturas promocionales, vídeos cortos para la app. No toca `docs/`, `worker/` ni `mobile/`: vive solo en esta carpeta, con su propio `.venv`.

> No es de Google. Es un proyecto comunitario, en fase alpha, que automatiza un Chrome real con tu sesión de Flow. Usarlo queda sujeto a los Términos de Google y el riesgo de cuenta es tuyo; lee [ACCOUNT_SAFETY](https://github.com/ffroliva/gflow-cli/blob/main/docs/ACCOUNT_SAFETY.md) antes de empezar.

## Qué necesitas

- Un PC (Windows, macOS o Linux) con **Google Chrome** instalado. No funciona en el móvil ni en una sesión en la nube: el login pide una ventana de Chrome visible.
- [uv](https://docs.astral.sh/uv/) y Python 3.11+ (uv lo descarga si falta).
- Una cuenta de Google con acceso a Flow.

## Instalación

```bash
cd gflow
uv sync                                   # crea gflow/.venv con las versiones de uv.lock
uv run playwright install chromium        # una vez, ~150 MB
cp .env.example .env                      # salidas a gflow/output/, MCP sin gasto
uv run gflow auth login --browser chrome  # abre Chrome: inicia sesión con Google
uv run gflow auth status                  # debe terminar en verde
```

Todos los comandos se lanzan desde `gflow/` con `uv run gflow …`.

## Uso rápido

```bash
uv run gflow models                                   # modelos y alias disponibles
uv run gflow credits user                             # saldo de créditos (no gasta)
uv run gflow image t2i "icono de radar azul marino, fondo claro, plano" --aspect 1:1
uv run gflow video t2v "..."                          # ⚠ Veo gasta créditos
uv run gflow --help                                   # el resto: character, scene, movie, project…
```

**Coste:** solo el vídeo (Veo) consume créditos. Las imágenes y los `upscale` usan cuota diaria y no gastan créditos, aunque el `upscale` de vídeo solo se ha medido como gratis una vez.

## Con Claude Code (MCP)

`.mcp.json` registra el servidor en modo `--no-spend`: Claude puede listar modelos, proyectos, personajes, descargar y escalar medios, pero **las herramientas de generar imagen o vídeo no existen** para él. Se activa al abrir Claude Code dentro de `gflow/` (te pedirá aprobarlo la primera vez). Si algún día quieres que genere, quita `--no-spend` a conciencia.

## Dónde quedan las cosas

| Qué | Dónde | ¿En git? |
|---|---|---|
| Sesión de Google (cookies) | `~/.local/share/gflow-cli` · `%LOCALAPPDATA%\gflow-cli` · `~/Library/Application Support/gflow-cli` | No, fuera del repo |
| Imágenes y vídeos | `gflow/output/` (con el `.env` de ejemplo) | No |
| Entorno Python | `gflow/.venv/` | No |
| Versión fijada | `pyproject.toml` + `uv.lock` | Sí |

Nunca subas una carpeta `profile_*` ni pongas `GFLOW_CLI_HOME` dentro del repo: contienen tu sesión de Google.

## Actualizar

No uses `gflow update` aquí; cambia la versión en `pyproject.toml` y ejecuta:

```bash
uv lock --upgrade-package gflow-cli && uv sync
```

Revisa antes el [CHANGELOG](https://github.com/ffroliva/gflow-cli/blob/main/CHANGELOG.md): Flow cambia a menudo y el proyecto saca versiones casi a diario.
