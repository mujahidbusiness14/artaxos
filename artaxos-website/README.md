# Artaxos — sitio web

Sitio estático de una sola página. No requiere build: los archivos se sirven tal cual.

Preparado por **Kometa Creatividad + Estrategia** — Agosto 2026.

---

## Contenido del paquete

```
index.html                  página completa
assets/
  index.BWnQOJ_3.css        hoja de estilos
  img-op/                   76 imágenes (pares AVIF + WebP)
entries/                    JavaScript del sitio
favicon.ico                 16/32/48 px
favicon-16.png
favicon-32.png
apple-touch-icon.png        180 px, pantalla de inicio iOS
icon-192.png / icon-512.png Android / PWA
site.webmanifest
sitemap.xml
robots.txt
vercel.json                 configuración para Vercel
.htaccess                   configuración para Apache / cPanel
nginx.conf.example          ejemplo de server block para Nginx
```

No hay `package.json` ni paso de compilación. Es un sitio estático.

---

## Opción A — Vercel (recomendado)

### Con GitHub

1. Sube el contenido de esta carpeta a un repositorio.
2. En Vercel: **Add New → Project → Import** el repositorio.
3. Framework Preset: **Other**. Deja Build Command y Output Directory vacíos.
4. **Deploy**.

Cada push genera una URL de preview, útil para que el cliente revise antes de publicar.

### Sin GitHub

Instala la CLI y publica desde la carpeta:

```bash
npm i -g vercel
cd artaxos-dist
vercel          # preview
vercel --prod   # producción
```

### Dominio

En **Settings → Domains**, agrega `artaxos.com`. El dominio está en Cloudflare,
así que el registro DNS se cambia allí, no en el registrador.

> Importante: si Cloudflare queda en modo proxy (nube naranja) frente a Vercel,
> configura SSL/TLS en **Full (strict)**. En modo Flexible se producen bucles de
> redirección.

---

## Opción B — VPS o hosting compartido

1. Sube todo el contenido de esta carpeta a la raíz web
   (`/var/www/artaxos`, `public_html`, `htdocs`, según el proveedor).
2. **Apache / cPanel**: `.htaccess` ya está incluido y funciona sin cambios.
3. **Nginx**: copia `nginx.conf.example` a `/etc/nginx/sites-available/artaxos`,
   crea el symlink en `sites-enabled`, luego `nginx -t && systemctl reload nginx`.
4. Instala el certificado SSL (Let's Encrypt / certbot).
5. Descomenta el bloque HTTPS en `.htaccess` si usas Apache.

### Detalle que suele fallar

Las imágenes son **AVIF y WebP**. Servidores antiguos no conocen esos tipos MIME
y los entregan como `application/octet-stream`, con lo que el navegador no los
muestra y el sitio se ve sin fotos. Los tipos ya están declarados en `.htaccess`
y en `nginx.conf.example`. Si las imágenes no aparecen, ese es el motivo.

---

## Después de publicar

1. **Google Search Console** → agrega la propiedad → envía `https://artaxos.com/sitemap.xml`.
2. Verifica los enlaces críticos:
   - Botón "Solicitar Cotización Personalizada" → WhatsApp `+52 55 3196 9083`
   - Íconos de Facebook, Instagram y TikTok en el footer
   - Botón "Reseñas en Google" en la sección Contacto
3. Comparte el enlace en WhatsApp y revisa cómo se ve la vista previa
   (ver pendientes abajo).

---

## Checklist del brief — 13 de 13 completados

| # | Cambio | Prioridad | Estado |
|---|---|---|---|
| 1 | Corregir href del botón "Solicitar Cotización Personalizada" | Crítica | Hecho |
| 2 | URLs reales en íconos FB, IG, TikTok del footer | Crítica | Hecho |
| 3 | Enlace de Google Business Profile en Contacto | Crítica | Hecho |
| 4 | Reemplazar testimonios por sección de eventos reales | Alta | Hecho |
| 5 | Sustituir imágenes de galería con fotos reales | Alta | Hecho |
| 6 | Actualizar H1 y subtítulo del hero (Crowd Management) | Alta | Hecho |
| 7 | Actualizar meta description | Media | Hecho |
| 8 | Actualizar copy del footer | Media | Hecho |
| 9 | Corregir texto de copyright | Media | Hecho |
| 10 | Estadísticas solo en la sección Nosotros | Media | Hecho |
| 11 | Agregar sección FAQ | Recomendada | Hecho |
| 12 | Agregar sección de tipos de evento | Recomendada | Hecho |
| 13 | Agregar sección educativa de Crowd Management | Recomendada | Hecho |

### Adicionales (fuera del checklist, informados)

- `<link rel="canonical">` y `<meta property="og:url">` estaban **vacíos**; ahora apuntan a `https://artaxos.com/`
- `<title>` estaba **truncado a media palabra** (`...para la seguridad fí...`); reemplazado por
  `Artaxos | Crowd Management Professional en México`
- Favicons: el HTML los referenciaba pero **los archivos no existían** (404); ya están generados
- `sitemap.xml` creado y declarado en `robots.txt`
- Marcadores de posición (blur) de la galería aún mostraban las fotos de stock; regenerados
- `og:description`, `twitter:description` y la descripción del JSON-LD decían *"renta de vallas
  metálicas y arcos detectores"*; actualizados al nuevo posicionamiento
- Se eliminó un script de Cloudflare (`/cdn-cgi/challenge-platform/...`) que quedó incrustado al
  copiar el sitio y provocaba un error 404 en cada carga fuera de Cloudflare

---

## Correcciones posteriores (revisión del cliente, Agosto 2026)

| # | Corrección | Prioridad | Estado |
|---|---|---|---|
| 1a | Logo flotante del hero quedaba fijo sobre el contenido en móvil | Crítica | Hecho |
| 1b | Badge "24/7 Disponible" encimado con el logo en móvil | Crítica | Hecho |
| 1c | Sección "¿Por Qué Artaxos?" cortada del lado derecho en móvil | Crítica | Hecho |
| 2 | Encabezado a forma de pregunta: "¿Dónde Operamos?" | Media | Hecho |
| 3 | Centrar las 2 tarjetas de la fila inferior | Media | Hecho |
| 4 | Justificar el párrafo de Crowd Management | Baja | Hecho |

Detalle técnico de las tres correcciones móviles:

- **1a** — `#top-logo-area` estaba en `position:fixed`, por lo que flotaba encima del
  contenido durante todo el scroll. Se cambió a `position:absolute` solo por debajo de
  768 px: ahora queda arriba de la página y se va con el scroll. En escritorio no cambia nada.
- **1b** — El badge del hero estaba anclado arriba a la derecha (`-top-4 -right-4`), justo
  donde aparece el logo flotante en móvil. Se movió abajo a la derecha solo en móvil.
- **1c** — Causa real: los elementos de un CSS grid tienen `min-width:auto` por defecto, así
  que la imagen de 800 px forzaba una columna de 804 px dentro de una rejilla de 358 px. El
  texto sobrante quedaba recortado por `overflow-x:hidden`. Se fijó la columna a
  `minmax(0,1fr)` y `min-width:0` en los hijos, por debajo de 1024 px.

---

## Pendientes de decisión del cliente

Ninguno impide publicar, pero conviene resolverlos.

1. **Logo del hero / header.** El archivo nuevo es un lockup vertical (proporción 1.88:1);
   el actual es horizontal (4.18:1). Con la misma altura, el nuevo se ve a menos de la mitad
   de ancho y en móvil la palabra "Artaxos" quedaría en unos 7 px. Se necesita una versión
   horizontal, o autorización para aumentar la altura del header y el footer.

2. **Etiquetas de la galería.** CORPORATIVO, EVENTO PRIVADO, CONFERENCIA, EXPOSICIÓN,
   EVENTO PÚBLICO y EVENTO MASIVO fueron deducidas de las fotos. Conviene confirmarlas.

3. **Permisos de sede.** Algunas fotos muestran Park Hyatt, JW Marriott Polanco y Hyatt Regency
   de forma identificable. Verificar que no exista una cláusula de confidencialidad.

4. **Reclamo "primera empresa en México".** Aparece en la meta description. Es una afirmación
   de superioridad no verificable; conviene que Artaxos confirme por escrito que la sostiene.

---

## Verificación técnica realizada

- **Responsive:** 320, 360, 390, 414, 768 y 834 px — sin desbordamiento horizontal,
  las 8 secciones presentes, sin texto cortado, sin imágenes rotas.
- **Animaciones:** probadas con scroll normal, scroll rápido y llegada por enlace directo
  (`#contacto`) con retroceso — ningún elemento queda invisible en ningún caso.
- **Estructura HTML:** todas las etiquetas balanceadas.
- **Rutas:** todas las referencias a imágenes y archivos raíz resuelven correctamente.
- **Accesibilidad:** el acordeón de FAQ usa `<details>`/`<summary>` nativos (funciona sin
  JavaScript); `prefers-reduced-motion` desactiva todas las animaciones.
