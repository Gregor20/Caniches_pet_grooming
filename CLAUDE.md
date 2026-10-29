# ROL
Actúa como Creative Developer + Art Director ganador de premios Awwwards. Construye la web de **Caniches Pet Grooming**, peluquería canina de Madrid especializada en caniches.

# OBJETIVO
Landing de una página, ultra moderna, dinámica y futurista de alta gama, con UN objetivo de negocio: que el visitante **reserve cita por WhatsApp**. Cada sección empuja hacia eso. Español de España (tuteo), `lang="es"`, precios en €.

# 0. PASO CERO: ANALIZA LA TARJETA ANTES DE ESCRIBIR CÓDIGO
Abre `img/Caniches pet grooming.pdf`: son 2 páginas y cada una es una foto JPEG rasterizada (sin texto ni vectores). Frente 1718×1153 px, dorso 1793×1157 px; ambas traen ~14 px de franja oscura abajo que hay que recortar.
1. Extrae las páginas (PyMuPDF, pdfimages, pdftoppm o Pillow) a `img/tarjeta-frente.webp` y `img/tarjeta-dorso.webp` (≤ 250 KB cada una).
2. Recorta el logo (arco con corona + siluetas de caniche + "Caniches Pet Grooming") a `img/logo.png` y el QR del dorso a `img/qr-instagram.png`. No existe el logo original con transparencia: úsalo dentro de una forma (círculo o arco con borde de neón) o recréalo en SVG, y avisa en el resumen final de que hace falta el original. Comprueba adónde apunta el QR antes de rotularlo.
3. Muestrea los colores con un script (no a ojo) y contrasta los tokens de la sección 2.
Si no puedes abrir el PDF, trabaja con el brief de la sección 1.

# 1. MARCA: DATOS REALES DE LA TARJETA (no añadas otros)
- Negocio: Caniches Pet Grooming. Titular: Jonatan Rodríguez.
- Teléfonos: 674 001 918 y 691 913 010 (enlaces `tel:+34…` para ambos). WhatsApp por defecto: `https://wa.me/34674001918`; confirma cuál de los dos lo tiene y déjalo en `CONFIG`.
- Instagram: @caniches_petgrooming → https://www.instagram.com/caniches_petgrooming/
- Dirección: Calle Gerardo Diego, 13 posterior, 28018 Madrid.
- Horario: NO figura en la tarjeta. No lo inventes: muestra "Con cita previa · reserva por WhatsApp" (campo opcional en `CONFIG`).
- Frente (ilustración estilo anime): dos estilistas con traje fucsia monocromo, un caniche negro grande y un perro peludo dorado (tipo pequinés) en un salón con boiserie y suelo de damero rosa brillante; neón de corona dorada y de corazón con huella; destellos de 4 puntas; cuadro de siluetas de caniche; planta en maceta rosa. Logo: arco con corona rosa y dos siluetas negras de caniche (una con lazo), "Caniches" en script caligráfico con degradado carmín y contorno blanco, "Pet Grooming" en serif negra bold.
- Dorso: rosa chicle con huellas de pata y corazones de línea fucsia, nombre en cursiva serif de alto contraste, iconos de teléfono/Instagram/ubicación y un QR con flecha dibujada a mano.
- Tono: cálido, divertido y glamuroso, pero impecable. El perro dorado sugiere que atienden otras razas: posiciónate como "especialistas en caniches" sin afirmar exclusividad.

# 2. CONCEPTO Y SISTEMA VISUAL: "EL SALÓN DE LA TARJETA, DE NOCHE"
El mismo salón rosa de la tarjeta con las luces apagadas y el neón encendido: lujo oscuro + glamour fucsia. Lo futurista va en el comportamiento (spotlight, magnetismo, holo-foil, micro-etiquetas HUD tipo "01 — SERVICIOS", líneas de 1 px), NO en cambiar la marca a azul, cian o morado.

Tokens (variables CSS + `tailwind.config`; los colores de marca están medidos en la tarjeta):
- Fondo obsidiana cálida #09050B · superficies ciruela #150B14 / #1E101C · texto marfil #FFF4F8
- Fucsia de marca #FE086C (neón, glows, bordes, iconos, texto grande; nunca en superficies grandes planas) · fucsia profundo #E6005F para botones rellenos con texto blanco (4,6:1; el blanco sobre #FE086C solo da 3,8:1)
- Rosa cuarzo #FF7EB3 · rosa chicle #FD98BC · pétalo #FAD8E3 · carmín del logo: degradado #AC011E → #E93357
- Oro champán #F5D08A y oro neón #FED371 (la corona) para detalles de lujo: precios, filetes, destellos
- Lavanda eléctrico #8A64FF: solo como luz ambiental de la aurora (≤ 2 % de la superficie), nunca en botones ni texto (no aparece en la tarjeta)
- Reparto: 70 % obsidiana · 20 % rosas · 8 % oro · 2 % lavanda

Tipografía (máx. 3 familias): Syne 700/800 en titulares (fluidos con `clamp()`, tracking apretado) con la palabra clave en Playfair Display Italic (eco de la cursiva del nombre en la tarjeta) · Plus Jakarta Sans 400/500/600 para texto · micro-etiquetas en mayúsculas con tracking amplio. El script "Caniches" no se reescribe con otra fuente: usa el recorte del logo; solo si no queda limpio, "Caniches" en Playball + "Pet Grooming" en Playfair Display 800.

Elementos firma de la tarjeta (úsalos: hacen única la web):
a) suelo de damero rosa/negro en perspectiva 3D, con brillo y reflejo, en el hero y en el CTA final;
b) boiserie: marcos de línea fina con luz de neón rosa como paneles de pared tras las secciones;
c) neón SVG con glow en capas y parpadeo muy sutil: corona champán, corazón con huella y destellos de 4 puntas (que también son la partícula del canvas y la viñeta de las listas);
d) huellas de pata y corazones de línea como patrón tenue;
e) siluetas de caniche en SVG (cachorro con lazo, continental, teddy).
Atmósfera: aurora de 3 `radial-gradient` grandes con translate/scale lento (40–60 s), grano SVG al 4–6 %, bordes de cristal de 1 px.

# 3. REGLAS TÉCNICAS (obligatorias)
- Un único `index.html` en la raíz, con CSS y JS inline y sin build. Las imágenes viven en `img/` con rutas relativas.
- Librerías solo por CDN y con versión fija (nada de `@latest`; comprueba que cada URL responde 200): Tailwind CSS (Play CDN: aceptable aquí por la restricción de archivo único; en producción real se compilaría), Lucide, GSAP 3.13+ con ScrollTrigger (SplitText ya es gratuito), Google Fonts (`display=swap`, solo los pesos usados, `preconnect`). Canvas 2D propio.
- Los efectos firma (damero, neón, spotlight, holo) van en un `<style>` propio. Llama a `lucide.createIcons()` tras cada render dinámico.
- Código completo y funcional: sin "...", sin "resto del código", sin lorem ipsum.
- Todo lo editable vive en UN objeto `CONFIG` al inicio del `<script>` y el HTML se pinta desde él: marca y contacto, servicios (con `claims`), tamaños, factores de precio, extras, estilos, reseñas y `flags`.
- Resiliencia: si una CDN falla, la web sigue legible (comprueba `window.gsap` y `window.lucide`). Anti-FOUC: `<html class="no-js">` pasa a `js` con un script inline en `<head>`; el CSS oculta los `[data-reveal]` solo bajo `.js`; si GSAP no carga en 3 s, se quita `.js`.
- Rendimiento: un único bucle `requestAnimationFrame` compartido (cursor, canvas, tilt), pausado con `document.hidden`; DPR ≤ 2; `backdrop-filter` solo en navbar, modal y chips; imagen del hero con `fetchpriority="high"` y `width/height`, el resto `loading="lazy"`; `gsap.matchMedia()` para separar escritorio, móvil y reduced-motion; `ScrollTrigger.refresh()` tras `document.fonts.ready` y `load`. Objetivo Lighthouse móvil: Rendimiento ≥ 85, Accesibilidad ≥ 95, SEO ≥ 95, CLS < 0,05.
- Accesibilidad: `prefers-reduced-motion` (sin cursor custom, parallax ni tilt; canvas estático; marquee desplazable a mano), foco visible, contraste ≥ 4,5:1, objetivos táctiles ≥ 44 px, texto ≥ 14 px, roles y `aria-*` en pestañas, acordeón y modal (usa `<dialog>` nativo).
- Táctil: todo `:hover` y el cursor custom solo bajo `@media (hover:hover) and (pointer:fine)`. En móvil el tilt se sustituye por flotación suave (giroscopio opcional con botón; iOS pide permiso) y el spotlight por un brillo fijo suave.
- Responsive real en 360, 390, 768, 1024, 1440 y 1920 px; `100svh`; sin scroll horizontal; scroll nativo (sin scroll-jacking).
- SEO local: title, meta description, `theme-color` #09050B, Open Graph, favicon SVG inline y JSON-LD `LocalBusiness` (nombre, teléfono +34674001918, dirección, `sameAs` Instagram).

# 4. COMPORTAMIENTOS GLOBALES
- Canvas de fondo (`position:fixed`, `pointer-events:none`, escucha `pointermove` en `window`, `resize` con debounce). Tres capas: micro-burbujas iridiscentes (borde que rota entre fucsia, cuarzo, champán y lavanda), polvo dorado con destellos de 4 puntas que parpadean y bokeh difuso. Reaccionan al puntero con repulsión suave (radio ≈ 140 px, muelle con amortiguación, sin saltos), ascienden despacio con wrap y la velocidad del scroll las estira un poco. Clic/tap = ráfaga de 8–12 destellos. Cantidad = `min(90, área/22000)`, ≤ 45 en móvil, donde derivan solas y reaccionan al toque.
- Cursor: anillo luminoso de 36 px + punto de 6 px. El anillo sigue con lerp ≈ 0,18 (`gsap.quickTo`) y glow fucsia. Sobre `a, button, [data-magnetic], .card` crece a ≈ 72 px y el elemento es atraído hasta 10 px (efecto magnético). El cursor nativo solo se oculta cuando el custom ya se inicializó y se restaura sobre input, textarea y select.
- GSAP: `power3.out` / `expo.out`, 0,6–1,2 s, stagger 0,08. Titulares revelados por línea con máscara, `ScrollTrigger.batch` en las tarjetas, contadores animados y parallax suave en el hero.

# 5. SECCIONES (en este orden)
## 5.1 Navbar flotante (glassmorphism)
Píldora centrada (`top-4`, ancho máx. ≈ 960 px) con `bg-white/5 backdrop-blur-xl border border-white/10`: mini-logo + "Caniches Pet Grooming", enlaces (Servicios · Estilos · Presupuesto · Opiniones · Visítanos) y CTA "Reservar" con icono de WhatsApp y borde luminoso de gradiente cónico animado (`@property --a`, con fallback estático). Se oculta al bajar y reaparece al subir; enlace activo según ScrollTrigger. En móvil: hamburguesa → menú a pantalla completa con enlaces en stagger, foco atrapado, ESC cierra, `aria-expanded`.

## 5.2 Hero
- Izquierda: eyebrow "Peluquería canina · Madrid"; H1 gigante con gradiente animado fucsia → cuarzo → champán: «Alta costura & spa de lujo para tu caniche» (la palabra "caniche" en Playfair Display Italic); subtítulo de 2 líneas; CTAs "Reserva tu cita" (WhatsApp) y "Descubre los estilos".
- Derecha: la **tarjeta de presentación como pieza coleccionable 3D** con `tarjeta-frente`, esquinas de 28 px. Tilt con el ratón (±12°, `perspective` 1000 px, suavizado con `quickTo`), brillo especular que sigue al puntero, lámina holográfica sutil (`mix-blend-mode`), capas de neón (corona, corazón con huella, destellos) a distinto `translateZ`, y **giro frente/dorso** con clic/tap o botón "Girar". El dorso es HTML real (fondo rosa chicle con huellas, nombre en Playfair Italic, teléfonos `tel:`, Instagram, dirección, QR) para que sea seleccionable y accesible. Detrás, 2 tarjetas pequeñas con fotos de caniches en parallax.
- Chips flotantes de cristal con icono Lucide y flotación suave: "Cortes Teddy & Asian Style", "Hidroterapia de microburbujas", "Primera visita sin estrés".
- Suelo de damero en perspectiva en la base e indicador de scroll.

## 5.3 Servicios: bento grid con spotlight
Cuadrícula asimétrica (12 columnas en escritorio, 1 en móvil, al menos una tarjeta de 2 filas). Cada tarjeta lleva un `radial-gradient` que sigue al cursor (`--mx/--my` actualizados en rAF) en el relleno y en el borde de 1 px (borde iluminado con `mask-composite`). Tarjetas:
1. Corte Teddy Bear & Asian Style: esculpido a tijera de precisión.
2. Hidroterapia con microburbujas: spa relajante y nutrición del rizo.
3. Estilismo de exposición & Continental: acabados de pasarela.
4. Cuidado hipoalergénico & cachorros: primera experiencia sin estrés.
Más 2 teselas: "Jonatan & equipo" (ilustración de la tarjeta) e "Instagram @caniches_petgrooming" (con el QR en escritorio). Cada tarjeta: icono Lucide, título en Syne, 1–2 líneas, 2–3 etiquetas y un detalle visual (foto con tinte o SVG animado).

## 5.4 Selector de estilo: Puppy Clip · Teddy · León · Moderno
Pestañas accesibles (`role="tablist"`, flechas del teclado). Al elegir un estilo, GSAP hace crossfade del visual (foto en `img/estilos/` si existe; si no, silueta SVG de marca) y actualiza la ficha: descripción, **tiempo de sesión**, **nivel de mantenimiento** (medidor 1–5 + "retoque cada X semanas") y **precio estimado "desde"** (= base Toy × `factor` del estilo), con contadores animados. Por defecto: factores Puppy ×0,8 · Teddy ×1 · León ×1,5 · Moderno ×1,25; sesiones y mantenimiento coherentes entre sí. Todo sale de `CONFIG`, nunca del HTML. Botón "Calcular para mi caniche": preselecciona el servicio en la calculadora y hace scroll hasta ella.
Slider Antes/Después (`clip-path` + `input[type=range]`): SOLO si existen fotos reales en `img/antes-despues/`; si no, omítelo.

## 5.5 Calculadora y reserva rápida
Tres pasos visibles a la vez: (a) tamaño: Toy (≤ 28 cm), Miniatura (28–35), Mediano (35–45), Gigante/Estándar (45–60) [alturas FCI]; (b) servicio; (c) extras (chips). Resultado en vivo (`aria-live="polite"`) con contador animado: precio estimado en rango, tiempo de spa, desglose y la nota "Precio orientativo; se confirma tras valorar el pelaje".
- "Reservar por WhatsApp" = `<a href>` real, recalculado en cada cambio, con mensaje precargado vía `encodeURIComponent` ("Hola Jonatan 👋 Quiero reservar cita…" + tamaño, servicio, extras, estimado y tiempo).
- "Reserva rápida" abre un `<dialog>` con formulario moderno (nombre, nombre del peque, día preferido con `min` = hoy, franja mañana/tarde, notas). No envía nada a un servidor: valida, compone el mensaje y abre WhatsApp.
- Valores por defecto (PLACEHOLDER, a validar): base Toy 35 € / 60 min · Miniatura 45 € / 75 · Mediano 55 € / 100 · Gigante 75 € / 140. Factores precio/tiempo: baño spa ×0,6/×0,6 · primera visita cachorro ×0,7/×0,8 · Teddy & Asian ×1/×1 · exposición & Continental ×1,6/×1,5. Extras: mascarilla nutritiva +8 € / 10 min · desenredado profundo +10 € / 20 min · ozono +12 € / 10 min (claim, ver §7). Rango mostrado: de precio a precio × 1,15, redondeado a 5 €.

## 5.6 Testimonios (marquee infinito)
Dos filas en sentido opuesto con bucle sin saltos (pista duplicada con `aria-hidden` en la copia, `translateX(-50%)` en CSS), pausa con hover/foco, bordes con fundido (`mask-image`) y velocidad por variable CSS. Tarjeta: inicial en círculo (sin caras de stock), nombre, "con [perro], caniche [tamaño]", estrellas (`aria-label`), cita y etiqueta (ver §7). 8 reseñas de ejemplo en español natural, variadas en tono y longitud.

## 5.7 Visítanos
Dirección, teléfonos, WhatsApp e Instagram; mapa embebido (`https://www.google.com/maps?q=Calle+Gerardo+Diego+13,+28018+Madrid&output=embed`) en tono oscuro con filtro CSS, `loading="lazy"` y botón "Cómo llegar". Horario según §1.

## 5.8 CTA final y footer
CTA sobre el suelo de damero con neón: «Tu caniche merece pasarela» + botón de WhatsApp magnético. Footer: logo, enlaces, redes, aviso legal (titular Jonatan Rodríguez + dirección; NIF pendiente) y créditos. Botón flotante de WhatsApp en móvil (respeta `env(safe-area-inset-bottom)` y no tapa contenido).

# 6. IMÁGENES
Prioridad: (1) arte de la tarjeta extraído del PDF → (2) fotos reales del cliente si hay en `img/` → (3) Unsplash.
- Unsplash: fotos reales de caniches (poodle, toy poodle, poodle grooming, dog groomer). NO inventes IDs: usa URLs directas (`https://images.unsplash.com/photo-…?auto=format&fit=crop&w=1200&q=80`) que hayas comprobado (responden 200 y la foto es de un caniche). `alt` descriptivo en español, `width/height`, `loading="lazy"` y `onerror` que cae a una silueta SVG o a un degradado de marca. Crédito "Fotos: Unsplash" en el footer.
- Tratamiento: esquinas de 24–32 px, borde de cristal de 1 px, tinte sutil fucsia→obsidiana (`mix-blend-mode: soft-light`) y grano. Nunca filtros fuertes que desfiguren al perro.
- No presentes fotos de stock como trabajo del salón ni como antes/después.

# 7. CONTENIDO: QUÉ NO INVENTAR
- Cifras ("+2.000 clientes"), años de experiencia, premios, horarios, NIF, nombres ni políticas.
- Reseñas: ejemplos con `placeholder:true` y etiqueta discreta "Ejemplo" mientras `CONFIG.flags.showPlaceholderBadges` sea true. La insignia "Verificada" solo se pinta con `verified:true` (reseñas reales de Google/Instagram, con permiso). Publicar reseñas falsas como reales está prohibido por la normativa de consumo de la UE.
- Claims sensibles ("ozono", "orgánica", "hipoalergénico", "biotecnológico"): en `CONFIG.services[].claims`, desactivados por defecto, para activarlos o quitarlos sin tocar el HTML. Copy por defecto: "microburbujas", "cosmética suave".
- Todos los precios, siempre "orientativos".

# 8. PROCESO, VERIFICACIÓN Y ENTREGA
1. Paso 0 y tokens. 2. `CONFIG`. 3. `index.html` completo.
4. Verifica de verdad: sirve la carpeta (`python -m http.server`) y ábrela en un navegador (o headless) a 390×844 y 1440×900. Comprueba: 0 errores en consola, sin scroll horizontal, imágenes cargadas, navegación por teclado (nav, pestañas, calculadora, modal), `prefers-reduced-motion`, y que el enlace de WhatsApp lleve el resumen correcto. Corrige lo que falle.
5. Cierra con un resumen de ≤ 10 líneas: qué hiciste, cómo probarlo, qué es placeholder y qué falta del cliente (logo original, fotos reales, precios, horario, reseñas reales, WhatsApp, NIF). Si no pudiste ejecutar un navegador, dilo; no afirmes que está probado.
Si no puedes escribir archivos, devuelve `index.html` completo en un solo bloque de código.
Si dos reglas chocan, prioriza: veracidad > conversión (WhatsApp) > accesibilidad y rendimiento > espectáculo visual.
Evita: emojis como iconos (usa Lucide), degradados morado→azul genéricos, tarjetas idénticas con sombra gris, todo centrado, más de 3 familias tipográficas, `filter: blur` gigante animado, imágenes rotas.
