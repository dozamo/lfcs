# Despliegue nativo de Jekyll en GitHub Pages

---

### Paso 1: Crear el Archivo de Configuración (`_config.yml`)

Este archivo es el cerebro de tu sitio. Le dice a GitHub Pages qué tema usar.

1.  Ve a tu nuevo repositorio en GitHub: `https://github.com/dozamo/lfcs`
2.  Haz clic en el botón **"Add file"** y luego en **"Create new file"**.
3.  En el campo del nombre del archivo, escribe exactamente:
    ```
    _config.yml
    ```
4.  En el área grande para editar el contenido, pega lo siguiente:
    ```yaml
    # El "cerebro" de tu sitio de documentación
    title: Mi Guía de Estudio LFCS
    description: Apuntes y guías para la certificación LFCS.
    
    # --- La línea mágica ---
    # Esto le dice a GitHub Pages que use el tema "Just the Docs"
    remote_theme: just-the-docs/just-the-docs
    
    # Opcional: añade un esquema de color. Prueba 'dark' o déjalo comentado.
    # color_scheme: dark
    ```
5.  Baja hasta el final de la página. Verás una sección para "Commit new file". Simplemente haz clic en el botón verde **"Commit new file"**. No necesitas cambiar nada.

**¡Felicidades!** Acabas de darle a tu sitio su instrucción más importante.

### Paso 2: Crear tu Primera Página de Contenido (`index.md`)

Ahora vamos a crear la página principal.

1.  De vuelta en la página principal de tu repositorio, haz clic de nuevo en **"Add file"** -> **"Create new file"**.
2.  Como nombre del archivo, escribe:
    ```
    index.md
    ```
3.  En el área de contenido, pega esto. Es Markdown con un pequeño encabezado (el `front matter` que hablamos antes).
    ```markdown
    ---
    layout: home
    title: Página Principal
    ---
    
    # Bienvenido a mi Guía de Estudio LFCS
    
    Este sitio contiene mis apuntes y guías personales para prepararme para la certificación **Linux Foundation Certified Sysadmin (LFCS)**.
    
    ## ¿Por qué este sitio?
    
    * Para organizar mi estudio.
    * Para practicar con GitHub Pages y `just-the-docs`.
    * Para compartir conocimiento con la comunidad.
    
    ¡Navega por las secciones usando el menú de la izquierda! (Pronto añadiremos más).
    ```
4.  Igual que antes, baja y haz clic en el botón verde **"Commit new file"**.

### Paso 3: Activar GitHub Pages (El interruptor maestro)

Ahora que tenemos los archivos, solo falta decirle a GitHub que los publique.

1.  En tu repositorio, haz clic en la pestaña **"Settings"**.
2.  En el menú de la izquierda, haz clic en **"Pages"**.
3.  Verás una sección llamada **"Build and deployment"**. Asegúrate de que las opciones estén así:
    *   **Source:** `Deploy from a branch`
    *   **Branch:**
        *   Branch: `main` (o `master` si tu repositorio usa ese nombre)
        *   Folder: `/(root)`
4.  Haz clic en **"Save"**.



**¡Y eso es todo!** La magia ocurre aquí. GitHub detectará tus archivos, verá la instrucción `remote_theme`, descargará "Just the Docs", construirá tu sitio y lo publicará.

### Paso 4: Ver tu Sitio en Vivo

Este proceso puede tardar uno o dos minutos la primera vez.

En la misma página de configuración de "Pages", verás un mensaje en la parte superior que dice: "Your site is live at...". Al principio puede que esté construyéndose (puedes ver el progreso en la pestaña "Actions").

Una vez listo, la URL de tu sitio será:

**`https://dozamo.github.io/lfcs/`**

¡Visítala y sorpréndete! Verás una página de documentación con un diseño profesional, creada con solo dos archivos y unos pocos clics.

---

### ¿Y ahora qué? El Flujo de Desarrollo

Ahora que el sitio está configurado, puedes empezar el flujo de desarrollo normal:

1.  **Clona el repositorio en tu máquina:**
    ```bash
    git clone git@github.com:dozamo/lfcs.git
    cd lfcs
    ```
2.  **Crea una nueva página:** Por ejemplo, crea un archivo llamado `01-comandos-basicos.md`.
    ```markdown
    ---
    layout: default
    title: Comandos Básicos
    nav_order: 1
    ---
    
    # Comandos Básicos de Linux
    
    Aquí veremos los comandos `ls`, `cd`, `pwd`...
    ```
3.  **Sube los cambios:**
    ```bash
    git add .
    git commit -m "Añadida página de comandos básicos"
    git push
    ```

Espera un minuto y refresca tu sitio web. Verás que la nueva página ha aparecido automáticamente en la navegación de la izquierda, ordenada correctamente gracias a `nav_order: 1`.

**Has logrado el flujo que te prometí:** has creado un sitio de documentación completo y funcional sin instalar nada localmente y sin escribir una sola línea de pipeline. A partir de ahora, tu trabajo es simplemente escribir Markdown y hacer `git push`.
