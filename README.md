# .github — Configuración compartida Indunnova16

Este repo contiene configuración que se aplica a TODOS los repos de la organización Indunnova16 que no tengan su propia versión.

## Contenido

### `.github/ISSUE_TEMPLATE/`
Plantillas estructuradas para abrir issues. Cuando alguien hace click en "New issue" en cualquier repo de la org, GitHub ofrece estos formularios automáticamente (a menos que el repo tenga su propio `.github/ISSUE_TEMPLATE/`).

- `bug_report.yml` — formulario "🐛 Reportar un bug"
- `feature_request.yml` — formulario "✨ Solicitar mejora"
- `config.yml` — desactiva la opción de "blank issue"

### Para modificar las plantillas
Edita los archivos en este repo y el cambio se propaga automáticamente a todos los 75+ repos. No hace falta tocar nada en los repos individuales.

### Para que un repo tenga su propia plantilla
Crear `.github/ISSUE_TEMPLATE/bug_report.yml` (o el archivo correspondiente) en ese repo específico. Tiene prioridad sobre el central.

## Guía completa para escribir issues
Ver [GUIA_ISSUES_INDUNNOVA.md](https://github.com/Indunnova16/.github/blob/main/GUIA_ISSUES_INDUNNOVA.md) en este repo.
