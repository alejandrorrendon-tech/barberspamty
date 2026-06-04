# The Barber's Spa — Roadmap, Ideas y Setup

Documento vivo del proyecto. Sirve para alinear a todos los que trabajen en la página
(humanos y sus Claude). Editar libremente, pero los cambios de **código** siempre pasan
por `git pull` → cambio → `commit` → `push` a `master`.

- **Repo:** https://github.com/alejandrorrendon-tech/barberspamty
- **Rama de trabajo:** `master`
- **Sitio (cuenta del socio):** https://barberspamty.vercel.app
- **Preview activo (cuenta de Alejandro):** https://barberspamty-seven.vercel.app
- **Stack:** HTML + CSS + JS vanilla en un solo `index.html`. Sin frameworks salvo necesidad real.
- **Identidad:** barbería premium. Paleta oscura + dorado. Tipografía condensada (Barlow Condensed / Barlow).
  **No cambiar colores ni fuentes sin pedirlo: ya están bien.**

---

## 1. Estado actual (hecho)

- [x] Rediseño base (hero, servicios, nosotros, galería, contacto, footer, WhatsApp flotante).
- [x] **Intro con scroll-scrub**: video (navaja, Firefly) controlado por scroll; al terminar
      aparece la home. Con fallbacks para `prefers-reduced-motion` y no-JS.
- [x] **Sección de productos**: carrusel con 6 productos *placeholder* + botón "Pedir" que abre
      WhatsApp con mensaje precargado.
- [x] **Carrusel de reseñas**: autoplay 5s + dots de navegación.
- [x] Link "Productos" en el nav.

---

## 2. Pendientes inmediatos (próxima sesión)

- [ ] Cargar **productos reales** (ver plantilla en sección 6).
- [ ] Afinar los carruseles (velocidad, snap, comportamiento en móvil).
- [ ] Ajustar suavidad/velocidad de la animación de intro según se vea en celular.
- [ ] (Opcional) Video de fondo en el hero + scroll suave con Lenis.

---

## 3. Flujo de trabajo (para no pisarnos)

1. **Siempre** `git pull origin master` antes de editar.
2. Cambio → `commit` con mensaje claro → `push` a `master`.
3. Nada de editar el código directo en Vercel ni en producción.
4. Vercel debe tener **auto-deploy** desde `master` (ver sección 5).
5. Para "ver cómo queda" → deploy a Vercel (URL pública), **nunca localhost**.

---

## 4. Lista para el socio (Vercel)

1. Aceptar la invitación de colaborador del repo.
2. Vercel → proyecto `barberspamty` → **Deployments** → **Redeploy** del último commit.
3. Settings → Git → activar **auto-deploy** en cada push a `master`.
4. (Ideal) Dar acceso al proyecto de Vercel a Alejandro para desplegar directo.
5. No tocar el código fuera de GitHub `master`.

---

## 5. Prompt para que el socio pegue en SU Claude Code

```
Hola, voy a colaborar con un socio en una página web (barbería en Monterrey).
El repo de GitHub es: https://github.com/alejandrorrendon-tech/barberspamty
La rama de trabajo es: master

Configura mi entorno así:

1. Clona el repo en una carpeta de trabajo y déjalo listo para hacer pull/push.
2. Regla de oro: NUNCA edites el código directo sin antes hacer `git pull origin master`.
   Después de cada cambio significativo: commit + push a master. Mi socio trabaja en
   el mismo repo, así evitamos pisarnos.
3. Es un sitio estático (HTML + CSS + JS vanilla en un solo index.html). No migrar a
   frameworks salvo que lo pidamos.
4. Hosting: Vercel, conectado a este repo con auto-deploy en cada push a master.
5. Cuando quiera "ver cómo queda", haz deploy a Vercel (URL pública), nunca localhost.
6. Tono del proyecto: barbería premium, paleta oscura + dorado, tipografía condensada.
   NO cambiar colores ni fuentes sin pedirlo: ya están bien.
7. Antes de codear un proyecto nuevo o un cambio grande: estructúrame un brief formal
   y espera mi "va" antes de ejecutar.
8. Lee el archivo ROADMAP.md del repo: ahí está todo el contexto, ideas y pendientes.

Confírmame cuando el repo esté clonado y me digas el estado actual del index.html.
```

---

## 6. Plantilla de productos (vaciar aquí los reales)

Por cada producto: nombre, categoría, precio, descripción corta (1 línea), y foto
(URL o archivo en `assets/products/`).

| # | Nombre | Categoría | Precio | Descripción corta | Foto |
|---|--------|-----------|--------|-------------------|------|
| 1 | | Cabello / Barba / Sets | $ | | |
| 2 | | | $ | | |
| 3 | | | $ | | |
| 4 | | | $ | | |
| 5 | | | $ | | |
| 6 | | | $ | | |

Placeholders actuales (a reemplazar): Pomada Mate $280, Cera Moldeadora $260,
Aceite para Barba $320, Tónico Sal de Mar $240, Pomada Clásica $300, Kit Esencial $890.

---

## 7. Ideas para subir el sitio a "nivel Awwwards"

Hacerlas por capas: primero lo funcional, luego el wow visual. Filosofía: **80% del impacto
con 20% del esfuerzo.**

| Idea | Herramienta | Impacto / Esfuerzo |
|------|-------------|--------------------|
| **Video de fondo en el hero** (humo / luz dorada lenta en loop) | Firefly (genera) + ffmpeg (loop perfecto + WebM ligero) | Alto / Bajo |
| **Scroll "el cliente entra a la barbería"** (cámara avanza con el scroll) | Firefly video + técnica de scrub ya implementada | Muy alto / Medio |
| **Transiciones suaves entre secciones** (fade, cortina, parallax) | GSAP + Lenis (scroll suave premium) | Alto / Bajo |
| **3D de navaja/silla girando** | Spline (diseño visual sin código) → exportar | Medio-alto / Medio |
| **Hover magnético** en botones y cards | GSAP (~15 líneas) | Detalle premium / Muy bajo |
| **Galería antes/después** (slider arrastrable) | JS vanilla | Útil + vendedor / Bajo |
| **Texto que se revela al hacer scroll** (letra por letra) | GSAP SplitText | Elegante / Bajo |

**Recomendación:** después de meter productos reales y afinar carruseles, agregar
**1 video de fondo en el hero** + **Lenis (scroll suave)**. Eso solo ya sube muchísimo el nivel.

---

## 8. Técnica: extender una animación de video (idea de Alejandro)

Se puede **encadenar** videos cortos para hacer animaciones largas y continuas:

1. Generas un video de 8s (ej. en Firefly).
2. Extraes su **último frame**:
   `ffmpeg -sseof -0.1 -i video.mp4 -frames:v 1 ultimo-frame.jpg`
3. En Firefly (image-to-video) subes ese frame como **imagen inicial** + prompt de qué sigue.
4. Pegas los clips: `ffmpeg` concat → video largo y fluido.

Encadenando así: 8s → 16s → 24s… sin cortes visibles.
(Ya se usó para la intro: `intro-poster.jpg` es el último frame y sirve de poster/punto de continuación.)

---

## 9. Herramientas disponibles para crear cosas increíbles

- **Firefly (Adobe)** — generar imágenes y videos premium (hero, productos, animaciones).
- **ffmpeg** — cortar, comprimir, loop perfecto, chroma key, concatenar, sacar frames.
- **Firecrawl** — buscar/scrapear imágenes y datos de la web.
- **Unsplash** — fotos reales gratis para uso comercial (placeholders y galería).
- **GSAP + Lenis** — animaciones y scroll suave nivel premium.
- **Spline / Three.js** — 3D para el web.
- **Playwright** — abrir el sitio y tomar screenshots para verificar antes de publicar.
- **magick (ImageMagick)** — resize/formato de imágenes sin IA.

> Esta lista se actualiza conforme sumemos herramientas nuevas que ayuden a crear mejor.
