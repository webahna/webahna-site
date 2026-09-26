# AGENTS.md

## Alcance

Estas instrucciones aplican a todo el repositorio. Este proyecto es el sitio público de Webahna, construido como sitio estático con Astro. Mantén los cambios pequeños, coherentes con la página afectada y orientados a rendimiento, accesibilidad, SEO y conversión.

## Stack y comandos

- Node.js `>=22.12.0` y npm (el lockfile canónico es `package-lock.json`).
- Astro 6 con salida estática.
- Tailwind CSS 4 mediante `@tailwindcss/vite`.
- Sitemap generado por `@astrojs/sitemap`; el dominio canónico está configurado como `https://webahna.com`.

Ejecuta los comandos desde la raíz:

```sh
npm ci          # instalación limpia
npm run dev     # desarrollo local, normalmente en http://localhost:4321
npm run build   # compilación de producción en dist/
npm run preview # revisión local de la compilación
```

No hay scripts de lint, tests ni type-check dedicados en `package.json`. No afirmes que pasaron. La validación mínima obligatoria para cambios de código es `npm run build`.

## Mapa del proyecto

- `src/pages/`: rutas basadas en archivos.
  - `index.astro`: portada principal.
  - `diseno-web-los-mochis.astro`: landing de SEO local.
- `src/layouts/Layout.astro`: documento HTML común, metadatos SEO, fuentes y Google Analytics.
- `src/components/index/`: secciones de la portada.
- `src/components/mochis/`: secciones de la landing de Los Mochis.
- `src/styles/global.css`: Tailwind, fuentes locales, tema y estilos base.
- `src/styles/index/` y `src/styles/mochis/`: CSS tradicional asociado a las secciones.
- `src/assets/`: imágenes y SVG procesados por Astro; impórtalos desde los componentes.
- `public/`: archivos servidos sin transformación, como favicons y fuentes; referencia estos archivos con rutas desde `/`.
- `dist/`, `.astro/` y `node_modules/`: contenido generado. Nunca lo edites ni lo incluyas en cambios manuales.

`src/components/LosMochis.astro` es una versión monolítica heredada y actualmente no forma parte de una ruta. La implementación activa de esa landing está dividida en `src/components/mochis/`. No actualices ambas versiones salvo que la tarea pida expresamente consolidarlas.

## Convenciones de implementación

- Usa componentes `.astro` renderizados en servidor por defecto. Agrega JavaScript de cliente solo cuando una interacción lo requiera.
- Conserva el estilo existente: indentación de dos espacios, comillas dobles en JavaScript/TypeScript y punto y coma en el frontmatter.
- Mantén las páginas como composición de secciones; coloca el contenido y marcado de una sección en su componente, no en la página de ruta.
- Reutiliza `Layout.astro` para páginas nuevas y tipa sus props en el frontmatter.
- Conserva el contenido visible en español y sus acentos. No cambies precios, testimonios, datos de contacto, afirmaciones comerciales ni identificadores de analítica sin una petición explícita.
- No agregues dependencias para algo que Astro, CSS o JavaScript del navegador puedan resolver de forma clara.

## Estilos

La portada está en una migración parcial a Tailwind 4: `Nav.astro` y `Hero.astro` ya contienen utilidades, mientras otras secciones siguen importando CSS desde `src/styles/index/`. La landing de Los Mochis continúa usando CSS tradicional por sección.

- Sigue el patrón del componente que estés modificando; no conviertas otras secciones de CSS a Tailwind como efecto secundario.
- Coloca tokens globales y `@theme` en `src/styles/global.css`.
- Si agregas CSS tradicional, impórtalo desde el componente dueño y usa selectores específicos de la sección. Los archivos CSS importados por componentes pueden afectar globalmente a toda la página; evita selectores genéricos nuevos como `section`, `nav` o `h2` sin un contenedor de ámbito.
- Antes de usar una variable CSS, confirma que esté definida para la página donde se renderiza. No dependas de los tokens comentados del tema anterior.
- Comprueba al menos vistas móvil y escritorio. Evita anchos fijos, desbordamiento horizontal y texto ilegible sobre fondos decorativos.

## SEO, enlaces y accesibilidad

- Para nuevas rutas, proporciona `title`, `description`, canonical y metadatos Open Graph mediante `Layout.astro`; usa el dominio configurado en Astro para URLs absolutas.
- Mantén un solo `h1` por página y una jerarquía semántica de encabezados.
- Los identificadores de sección (`servicios`, `proceso`, `proyectos`, `precios`, `contacto`) son destinos de navegación; si cambian, actualiza todos sus enlaces.
- Usa texto alternativo descriptivo en imágenes de contenido. Un `alt=""` solo es apropiado para imágenes puramente decorativas.
- Los controles interactivos deben funcionar con teclado y mostrar foco visible. Si se modifica el FAQ, conserva la apertura/cierre y sincroniza atributos como `aria-expanded` y `aria-controls`.
- En enlaces externos que abran con `target="_blank"`, agrega `rel="noopener noreferrer"`.
- No cambies el ID de Google Analytics, correos ni enlaces de WhatsApp sin autorización explícita.

## Validación antes de entregar

1. Revisa el diff y confirma que no incluye archivos generados, secretos ni cambios ajenos a la tarea.
2. Ejecuta `npm run build`.
3. Para cambios visuales o interactivos, revisa `/` y `/diseno-web-los-mochis/` según corresponda, en móvil y escritorio.
4. Verifica navegación por anclas, enlaces externos, imágenes, FAQ y ausencia de errores en la consola del navegador.
5. Si cambias metadatos o rutas, inspecciona el HTML generado y confirma que el sitemap siga construyéndose.

## Higiene de cambios

- Respeta los cambios locales existentes: no restaures, sobrescribas ni reformatees archivos fuera del alcance.
- No edites `.env` ni expongas secretos. Si se requieren variables nuevas, documenta solo sus nombres y propósito.
- Evita refactorizaciones amplias dentro de una corrección puntual. Si detectas deuda técnica relacionada, descríbela por separado.
- Mantén `package-lock.json` sincronizado únicamente cuando cambien dependencias.
