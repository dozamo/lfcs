# 📚 Sitio de documentación con MkDocs

Este repositorio contiene un sitio estático generado con **[MkDocs](https://www.mkdocs.org/)**, pensado para organizar y publicar documentación escrita en Markdown.

## 🚀 Instalación

> Requisitos previos:  
> - Python 3.8 o superior
> - `pip` instalado  

```bash
# 1. Crear un entorno virtual (opcional, recomendado)
python3 -m venv .venv
source .venv/bin/activate  # En Linux / macOS
# .venv\Scripts\activate   # En Windows

# 2. Instalar MkDocs
pip install mkdocs

# (opcional) Instalar el tema Material
pip install mkdocs-material
```

## 🛠️ Comandos principales

```bash
# Crear un nuevo proyecto
mkdocs new my-project
cd my-project

# Servir en local
mkdocs serve

# Construir para producción
mkdocs build
```

_Despliegues en Git{lab,hub,...} Page:_ Por defecto, MkDocs genera los archivos HTML en la carpeta `site/`. No se debe incluir esta carpeta en Git (debería ser listada en `.gitignore`).

## 📂 Estructura básica

```bash
.
├── docs/            # Archivos Markdown de la documentación
│   └── index.md     # Página principal
├── mkdocs.yml       # Configuración del sitio
└── .gitignore       # Ignora site/, entornos y temporales
```

## 📦 Despliegue

Con **GitHub Pages** y **MkDocs Material**:

```bash
pip install mkdocs-material
mkdocs gh-deploy
```

Esto compilará el sitio y lo publicará automáticamente en tu rama `gh-pages`.
