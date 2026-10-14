# Landing del taller — Curso de OpenCode

Página estática de una sola pieza (`index.html`) pensada para resolver el QR de cierre del deck
actualizado: diapositiva 20 (D14 en la numeración original), temario, preparación del entorno,
guía de uso de los archivos del kit y contacto del instructor.

- **Sin build, sin framework, sin peticiones externas.** HTML + CSS + un script inline mínimo.
- Funciona desde `file://` (doble clic en `index.html`) y desde cualquier host estático.
- Tema oscuro único (fijado por el propio deck: el QR se escanea en una sala con poca luz).
- Todo el contenido visible está en español; el código y los comentarios, en inglés.

## Deploy

1. Sube el contenido completo de `landing/` (`index.html` + `assets/`) al host estático que uses
   (GitHub Pages, Netlify, Cloudflare Pages, S3, IIS, nginx...). No hay rutas absolutas:
   la página funciona igual en la raíz del dominio o en un subdirectorio.
2. Abre la URL publicada y comprueba que la portada y la fuente mono se ven bien. No hace falta
   configurar nada más: no hay cookies, ni analítica, ni llamadas a CDN.

Prueba local sin publicar:

```bash
python -m http.server 8787 --directory landing   # luego http://127.0.0.1:8787/
```

## El QR de cierre (diapositiva 20 / D14)

El destino público todavía no está decidido (las tres opciones están en
`../instructor/checklist-pre-taller.md`). Pasos cuando se decida:

1. Publica `landing/` y **copia la URL final**.
2. Genera el QR con esa URL (el repo no incluye generador de QR).
3. Pega el QR en la diapositiva 20 (`bonus`) del deck nuevo; en la numeración original corresponde a D14 (`../presentacion/mejorada/Curso de OpenCode - mejorada.pptx`).

La página **no promete ninguna descarga**: describe cómo usar los archivos del kit. Si algún día
existe un enlace público, se añade como CTA adicional y se actualiza también este README.

## Dónde editar el contenido

Todo el contenido vive en `index.html`. Cada bloque está delimitado por un comentario en el CSS y
por su etiqueta en el HTML:

| Sección | Qué contiene | Ancla |
| --- | --- | --- |
| Barra superior | Logotipo de OpenCode, «Ver el temario», `@neshack` | — |
| Portada | Franja de estado de OpenCode 2, titular, resumen del taller, CTAs e ilustración | `#inicio` |
| Datos del taller | Una sesión, máximo de 2 horas, demos y entorno de OpenCode 2 | — |
| Temario | Los 6 bloques y sus números de diapositiva del deck actual (1–20); la diapositiva 20 corresponde a D14 | `#temario` |
| Cómo usar los archivos | 3 pasos, comandos del setup con pestañas y árbol del kit | `#archivos` |
| Instructor | Nombre, rol y enlace a X | — |
| Pie | Wordmark de OpenCode, chip de infomanagement, créditos | — |

**Datos: no inventar.** Las cifras y los textos salen de estas fuentes del kit y hay que mantenerlas
sincronizadas si el material cambia:

- `../instructor/guia-instructor.md` — nombres y secuencia de los bloques, además de la correspondencia D1–D14 con las diapositivas nuevas.
- `../setup/README.md` — comandos exactos del setup y la tabla de verificación.
- `../docs/verificacion-v2.md` — datos técnicos de OpenCode 2 citados (`opencode v2.x.y`); `../docs/verificacion-tecnica.md` y `../docs/transcripcion-deck.md` son históricas (V1), no fuentes de la página actual.
- `../presentacion/mejorada/Curso de OpenCode - mejorada.pptx` — títulos, orden y numeración del deck V2 (20 diapositivas).
- `../README.md` — estructura del kit que reproduce el árbol de `#archivos` (incluye `presentacion/`).

El límite de una sesión de hasta 2 horas es la instrucción vigente del instructor para esta landing;
no se muestran asignaciones de tiempo por bloque.

Si cambias el árbol del kit, cambia también `landing/index.html` (bloque `.tree-wrap`) y el README
del kit: el árbol de la página es una copia del que se distribuye.

## Assets

| Archivo | Origen |
| --- | --- |
| `assets/img/opencode-wordmark-dark.png` | `docs/opencode-wordmark-dark.png` (variante para fondo oscuro) |
| `assets/img/opencode-wordmark-light.png` | `docs/opencode-wordmark-light.png` (variante para fondo claro; hoy no se usa en la página) |
| `assets/img/logo_infomg.png` | `docs/logo_infomg.png`; en el pie va sobre un chip claro porque el azul de la marca necesita fondo claro |
| `assets/img/hero-opencode-v2.webp` | Versión WebP optimizada de `../presentacion/mejorada/imagenes/portada.png`, ilustración de portada generada para el deck V2 |
| `assets/img/og-image.jpg` | Recorte 1200×630 de la misma ilustración para Open Graph y la vista previa grande de X |

Notas:

- El logotipo de OpenCode es monocromo (no se recolorea) y el azul de infomanagement aparece solo
  dentro de su propio chip.
- Las imágenes sociales se declaran con ruta relativa. Antes de publicar, confirma que el
  previsualizador las resuelve; si requiere una URL absoluta, actualiza `og:image` y `twitter:image`
  con la URL pública elegida.
- El icono de X y la flecha de los enlaces externos son SVG en línea (el de X procede de Simple
  Icons, licencia MIT). El favicon es un SVG en `data:` — cero peticiones de red.

## Fuentes y licencia

Geist Sans (400/500/600/700) y Geist Mono (400/500) se sirven autoalojadas desde
`assets/fonts/*.woff2`, con `font-display: swap`. Los archivos vienen del paquete Fontsource
(jsDelivr) y se muestrean en `latin`.

**Licencia:** SIL Open Font License 1.1 — `Copyright 2024 The Geist Project Authors
(https://github.com/vercel/geist-font)`. El texto completo está en `assets/fonts/OFL.txt` y debe
acompañar a los `.woff2` si se redistribuyen (condición 2 de la OFL).

No se carga ninguna fuente desde Google Fonts ni desde un CDN en tiempo de ejecución.

## Comportamiento y accesibilidad

- **Movimiento:** entrada del héroe (transform + `clip-path`), revelados por `IntersectionObserver`
  y física de hover. Nada escucha `scroll`. Con `prefers-reduced-motion: reduce` se apaga todo y el
  contenido queda visible sin animaciones.
- **Pestañas del setup:** `role="tablist"` con teclado completo (flechas, Inicio/Fin). Sin
  JavaScript se muestran los dos paneles a la vez con su propio rótulo (degradación explícita).
- **Contraste medido en el navegador** (no estimado) sobre el fondo real de cada texto: el par más
  bajo de la página es 5.2:1 y el cuerpo va de 7.4:1 a 16.7:1 — WCAG AA en todos los casos.
- Foco visible con anillo naranja, `color-scheme: dark`, selección y barras de desplazamiento
  tematizadas, enlace «Saltar al contenido» y texto alternativo en todas las imágenes.
- Solo tema oscuro: no hay variante clara. El deck y la sala mandan, y mezclar temas en la misma
  página rompe la lectura.

## Verificación de esta actualización

- La landing refleja OpenCode 2 y el deck actual de 20 diapositivas; la diapositiva 20 corresponde a D14.
- El temario ya no distribuye minutos por bloque. Indica una sesión única con un máximo de 2 horas.
- Las rutas del hero y de la imagen social apuntan a assets locales optimizados; las rutas siguen siendo relativas.
- La vista previa de navegador en escritorio y móvil queda pendiente: el servidor HTTP local no fue accesible en esta sesión.
