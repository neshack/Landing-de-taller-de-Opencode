# Landing del Taller de OpenCode 2

Sitio estático (una sola página, sin build) para la landing del **Curso de OpenCode 2 — Taller práctico para consultores**. Publicado con GitHub Pages.

- **Demo en vivo:** https://neshack.github.io/Landing-de-taller-de-Opencode/
- **Autor:** Néstor Peña ([@neshack](https://x.com/neshack) · [LinkedIn](https://www.linkedin.com/in/neshack))
- **Complemento:** [infomanagement](https://infomg.mx)

---

## Qué es este repositorio

Es el sitio público que presenta el taller: temario, datos de la sesión, forma de preparar el entorno y quién lo imparte. No es la herramienta del taller ni el kit de materiales: es la página que el participante abre (normalmente desde el QR del proyector) y desde la que se entra a todo lo demás.

La página es un único `index.html` autónomo. No hay framework, ni bundler, ni `package.json`, ni dependencia de terceros en tiempo de ejecución.

## Estructura

```
.
├── index.html                      # página completa: HTML + CSS + JS en línea
├── assets/
│   ├── fonts/                      # Geist Sans + Geist Mono (woff2, self-hosted)
│   │   └── OFL.txt                 # SIL Open Font License 1.1
│   └── img/                        # hero, OG image, wordmarks y logo de infomanagement
├── README.md                       # (vacío por ahora)
└── README2.md                      # documentación del repositorio
```

No hay un `CNAME`: el sitio se sirve en la URL de proyecto de GitHub Pages.

## Desarrollo local

No hay nada que compilar. Cualquiera de estas dos opciones funciona:

```bash
# Abrir el archivo directamente
open index.html          # macOS
start index.html         # Windows

# O servirlo por HTTP (más fiel a producción)
python -m http.server 8000
npx serve .
```

Luego abrir <http://localhost:8000>.

## Despliegue

El repositorio se publica desde `main` usando **GitHub Pages**:

1. **Settings → Pages**
2. **Source:** *Deploy from a branch*
3. **Branch:** `main` · **Folder:** `/ (root)`
4. Guardar

Cada `push` a `main` reconstruye y publica el sitio. También se puede publicar manualmente con la CLI, si ya estás autenticado:

```bash
gh api -X POST repos/:owner/:repo/pages \
  -f source[branch]=main \
  -f source[path]=/ \
  --jq '.html_url'
```

## Decisiones técnicas

### Una sola página, cero dependencias

El contenido cabe en una sesión de 2 horas y el público entra desde un móvil con datos móviles. El presupuesto de carga manda: HTML, CSS y JS en el mismo archivo, fuentes servidas desde el propio repositorio y ninguna petición a terceros en runtime. No hay CDNs, ni Google Fonts, ni scripts de terceros.

### Tema oscuro únicamente

El deck se proyecta en una sala con luz baja, así que la página solo existe en oscuro. Los tokens viven en `:root` dentro del `<style>` de `index.html`:

| Token | Valor | Uso |
| --- | --- | --- |
| `--bg` / `--bg-2` | `#0A0E14` / `#0C1219` | Fondo base y franjas alternas |
| `--plate` / `--plate-2` | `#10171F` / `#151D27` | Superficies elevadas (código, kit) |
| `--line` / `--line-2` | `#1C2634` / `#27323F` | Separadores y bordes |
| `--ink` / `--ink-2` / `--ink-3` | `#E9EFF6` / `#A6B3C2` / `#7E8B9C` | Texto en tres niveles |
| `--accent` / `--accent-2` | `#FF7A2A` / `#FF9A3C` | Naranja terminal del deck |
| `--r-plate` / `--r-control` | `14px` / `10px` | Radios: superficies y controles |
| `--wrap` | `1200px` | Ancho máximo de contenido |
| `--ease` | `cubic-bezier(.16,1,.3,1)` | Curva única para toda transición |

Se usan dos familias: **Geist Sans** para el texto y **Geist Mono** para todo lo factual (fechas, comandos, rutas, números). El monoespaciado no es decorativo: es la convención visual de una terminal.

### JavaScript sin dependencias

Dos comportamientos, ambos escritos a mano en el `<script>` final:

- **Tabs del setup** (`[data-tabs]`): click, flechas ←/→, `Home`/`End` y *roving tabindex* (`aria-selected`, un único tabulador dentro del grupo).
- **Revelado al hacer scroll**: `IntersectionObserver` con `rootMargin: "0px 0px -8% 0px"` y `threshold: 0.12`; cada elemento se observa una sola vez y deja de observarse al entrar.

Sin JS, la página sigue siendo usable: `document.documentElement.classList` intercambia `no-js` por `js` al cargar, las pestañas se ocultan y **se muestran ambos paneles**, y todos los bloques aparecen sin animación.

### Accesibilidad

- Skip link al contenido, foco visible con `:focus-visible` y outline en el acento.
- Pestañas implementadas con el patrón ARIA completo (`tablist` / `tab` / `tabpanel`).
- Textos alternativos en todas las imágenes, iconos SVG marcados `aria-hidden`.
- `prefers-reduced-motion: reduce` desactiva animaciones, scroll suave y transiciones.
- Estilos de impresión: la página se imprime legible en blanco y negro.

### SEO y social

`lang="es"`, `theme-color`, y una tarjeta Open Graph completa (`og:image` 1200×630 en `assets/img/og-image.jpg`) replicada para Twitter (`summary_large_image`). El favicon es un SVG en línea con el glifo de prompt sobre fondo casi negro: cero peticiones extra.

### Rendimiento

- Fuentes Geist precargadas con `rel="preload"`, `font-display: swap` y `crossorigin`.
- La imagen principal usa `fetchpriority="high"` y `decoding="async"`, con `width`/`height` declarados para evitar CLS.
- Todas las imágenes están en WebP o PNG optimizado.

## Contenido de la página

| Sección | Contenido |
| --- | --- |
| Hero | Título, propuesta de valor y acceso al temario |
| Datos | Una sesión · 2 h máximo · 2 demos aplicadas · OpenCode 2 |
| Temario | Seis bloques (0–5), con las diapositivas correspondientes de un deck de 20 |
| Archivos del taller | Setup del entorno (Windows / macOS / Linux), cheat sheet y material de referencia |
| Instructor | Néstor Peña y sus enlaces |
| Footer | Créditos, marca y fecha de verificación de materiales |

## Requisitos del entorno del taller

Los comandos que aparecen en la página (para preparar el entorno antes de la sesión) son:

```powershell
# Windows — solo verifica, sin instalar nada
powershell -ExecutionPolicy Bypass -File .\setup\setup-windows.ps1 -Check
# Windows — aplica
powershell -ExecutionPolicy Bypass -File .\setup\setup-windows.ps1
```

```bash
# macOS / Linux — solo verifica
bash setup/setup-macos-linux.sh --check
# macOS / Linux — aplica
bash setup/setup-macos-linux.sh
```

Verificación posterior:

```bash
opencode --version        # responde "opencode v2.x.y"
opencode mcp list        # Context7 aparece conectado
```

> **Nota sobre Windows:** el script del kit usa la ruta de instalación con npm, probada localmente con OpenCode 2.0.27. La documentación oficial de V2 indica que los gestores de paquetes no están soportados en Windows y ofrece el [binario independiente](https://opencode.ai/v2/docs/).

## Cómo trabajar en el repositorio

1. Ramifica desde `main`.
2. Mantén el cambio en `index.html` coherente con el sistema de diseño: si tocas color, radio o espaciado, usa las variables de `:root`.
3. Revisa el resultado en móvil: la página se consulta desde un teléfono, y varios bloques cambian de layout por debajo de `700px`.
4. Commit con mensajes convencionales (`feat:`, `fix:`, `docs:`, `chore:`). Sin atribución de IA ni `Co-Authored-By`.
5. Abre un PR hacia `main`. El sitio se actualiza solo al hacer merge.

## Créditos y licencias

- **Geist Sans / Geist Mono** — [*Vercel Geist*](https://vercel.com/font), bajo SIL Open Font License 1.1. Texto completo en [`assets/fonts/OFL.txt`](assets/fonts/OFL.txt).
- **OpenCode** — nombre, wordmark e ilustración de portada son marcas de sus titulares; su uso aquí es referencial.
- **infomanagement** — logotipo y marca © de sus titulares.
- Las fotografías e ilustraciones de `assets/img/` son material original del taller.

El repositorio no incluye un archivo `LICENSE`. Sin licencia explícita, se aplica la reserva de derechos por defecto: si necesitas reutilizar el código o los materiales, pide autorización primero.

## Contacto

Néstor Peña · [@neshack](https://x.com/neshack) · [LinkedIn](https://www.linkedin.com/in/neshack)