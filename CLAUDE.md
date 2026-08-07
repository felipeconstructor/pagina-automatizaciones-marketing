# CLAUDE.md

Guía para Claude Code al trabajar en este repositorio.

## Proyecto

**Khrono** — landing page de la nueva empresa de Felipe Campos enfocada en
automatizaciones y soluciones integrales de IA y marketing (chatbots 24/7,
copiloto empresarial, marketing con IA, landing pages a medida). Distinta de
Nova (chatbot SaaS inmobiliario) y de Broker.

- Público (GitHub Pages, URL provisoria): https://felipeconstructor.github.io/pagina-automatizaciones-marketing/
- Dominio propio en trámite: **khrono.cl** (registro en NIC Chile, ver
  "Estado — dominio propio" más abajo)
- Repo: https://github.com/felipeconstructor/pagina-automatizaciones-marketing
  (nombre del repo quedó del branding anterior, no se ha renombrado)
- Local: `~/Desktop/AUTOMATIZACIONES Y MARKETING/index.html` (código vive
  directo en la raíz del repo, sin subcarpetas)

**Nota de historial:** el proyecto se llamó primero "Automatizaciones y
Marketing", después "Automatiza.IA", y se rebautizó a **Khrono** (commit
`77d1f3f`). El logo divide el nombre "KHRONO" mitad y mitad visualmente
(commit `9ed6cf5`).

## Stack

- HTML standalone (un solo `index.html`, sin build ni dependencias) +
  `assets/hero-bg.jpg` (foto real de fondo del hero, reemplazó un gráfico
  abstracto)
- Tipografía: Fraunces (títulos) + Public Sans (cuerpo)
- Paleta: fondo oscuro (`#0A0A0A`/`#0D0D0C`/`#141413`), texto crema
  (`#F7F5F2`/`#D9D5CC`), acento naranja (`#FF6B35`, con `#FFB020` de apoyo),
  verde de éxito (`#2ECC71`), rojo de error (`#E5484D`)
- Funnel: anuncio → landing → cuestionario que califica al lead → agenda
  reunión (sin precios/checkout en la página)
- Mobile: menú hamburguesa, tarjetas boxeadas, hint de swipe en carrusel de
  servicios

**Importante:** el archivo debe conservar `<!DOCTYPE html>`, `<head>`,
`<body>` y `<meta name="viewport" content="width=device-width,
initial-scale=1, viewport-fit=cover">`. Ya hubo un bug real (jul 2026) donde
al regenerar el archivo pegando solo `<title>`+`<style>`+body sin esas
etiquetas, los navegadores móviles reales renderizaban la página como
escritorio (980px) y la achicaban — texto diminuto y layout ancho, aunque el
CSS responsive estuviera perfecto. **Probar en Chrome de escritorio
(resize_window) no detecta este bug** — solo se ve en un dispositivo real o
con emulación de dispositivo (CDP device metrics).

## Contenido / secciones

- Hero con foto de fondo real + widget de pasos (reoriented a WhatsApp como
  primer paso, no Instagram Ads)
- "Quiénes somos" con bios de los fundadores y estadísticas reales (antes
  eran solo iniciales)
- Tarjetas de servicio con foto real de fondo (Copiloto empresarial, Chatbot
  24/7, Marketing con IA, Landing pages — en `assets/card-*.jpg`) + badge
  superpuesto con degradado oscuro para legibilidad; las tarjetas RAG de
  datos y "Cotiza con nosotros" siguen con el mockup CSS original (4 ago
  2026, commit `55b6c48`)
- Cuestionario de calificación de leads antes de poder agendar reunión
  (agregado para filtrar quién agenda)
- Mensaje central: automatizaciones **hechas a medida**, no plantillas
  genéricas (reforzado en hero, meta description y "Quiénes somos")

## Integraciones

- **Agendamiento**: los botones "Agendar reunión" apuntan a un evento de
  Google Calendar (`target="_blank"`):
  https://calendar.app.google/vx7JnYVW34hhfntu6
- **Meta Pixel**: instalado (7 ago 2026), ver sección "Estado — Meta Pixel /
  campaña de Ads" más abajo.
- **Notificación WhatsApp al equipo al agendar**: opcional, no implementado.
  Patrón sugerido: webhook de Google Calendar → Make.com → Kapso (mismo
  patrón que el reporte semanal de leads de Nova).
- **Guardar leads en Supabase**: opcional, no implementado.

## Estado — dominio propio (3 ago 2026)

Felipe está registrando **khrono.cl** en NIC Chile (clientes.nic.cl). En el
formulario se seleccionó **"Servidores DNS"** (no "Redireccionamiento Web"),
porque GitHub Pages necesita registros DNS reales (A/CNAME) y no solo un
reenvío de URL.

**Plan de DNS acordado:**
1. Cuenta gratis en Cloudflare → agregar `khrono.cl` → Cloudflare asigna 2
   nameservers propios.
2. Esos 2 nameservers se cargan en el campo "Nombre de Servidor" de NIC.cl
   (botón "Agregar Servidor de Nombre" por cada uno) antes de crear el
   dominio.
3. En Cloudflare, sección DNS del dominio, agregar:
   - 4 registros **A** en `@` → `185.199.108.153`, `185.199.109.153`,
     `185.199.110.153`, `185.199.111.153` (IPs de GitHub Pages)
   - 1 registro **CNAME** en `www` → `felipeconstructor.github.io`
4. En GitHub: repo → Settings → Pages → Custom domain → escribir
   `khrono.cl` (GitHub crea el archivo `CNAME` en el repo automáticamente) →
   activar "Enforce HTTPS" una vez disponible.

**Pendiente:** confirmar que Felipe completó el registro en NIC.cl y
configurar Cloudflare + GitHub Pages una vez el dominio esté activo.

### Avance (4 ago 2026)

- DNS ya está bien propagado: los 4 registros A y el CNAME de `www` resuelven
  correctamente a GitHub Pages, y los nameservers de Cloudflare están activos
  en el dominio. El repo ya tiene el archivo `CNAME` con `khrono.cl` (commit
  `d14834c`).
- `http://khrono.cl` responde 200. `https://khrono.cl` **todavía falla**: el
  certificado servido es el genérico `*.github.io`, no uno emitido para
  `khrono.cl` — GitHub aún no termina de provisionar el cert Let's Encrypt
  para el dominio custom.
- GitHub ahora pide un paso adicional no contemplado en el plan original:
  **verificar la propiedad del dominio** vía un registro **TXT** (Settings →
  Pages te da un "Host" y un "TXT value" para agregar en el DNS) antes de
  dejar activar/reconfirmar el custom domain. Pendiente completar: agregar
  ese TXT en Cloudflare (sin proxy, como cualquier TXT) y volver a GitHub a
  hacer clic en "Verify".
- Intentamos subir a Rodolfo (`rmenadrop-blip`) a rol **Admin** como
  colaborador del repo para que pudiera revisar Settings → Pages sin
  depender de Felipe, pero la UI de colaboradores de GitHub no mostró un
  selector de rol claro (ni al añadir por primera vez ni al re-invitar tras
  eliminarlo). Se abandonó ese camino por ahora — **se sigue trabajando
  directo en el PC de Felipe**, logueado como `felipeconstructor`, dueño del
  repo.
- Verificar de nuevo cuando el TXT esté agregado y confirmado: que el
  banner de error de DNS desaparezca en Settings → Pages y que se habilite
  el checkbox "Enforce HTTPS".

### Avance (4 ago 2026, más tarde) — checklist de lanzamiento

- **El certificado ya se emitió**: `https://khrono.cl` ahora responde 200
  con un cert de Let's Encrypt válido para `khrono.cl` y `www.khrono.cl`
  (expira 2 nov 2026). `www.khrono.cl` redirige (301) a `https://khrono.cl`
  correctamente. Confirmado vía `repos/.../pages` API:
  `https_certificate.state: "approved"`.
- **Pendiente de seguridad real, sigue bloqueado**: `protected_domain_state`
  sigue en `"unverified"` (el TXT del punto anterior nunca se agregó) y
  `https_enforced: false`. Esto significa que `http://khrono.cl` sirve la
  página **en texto plano** en vez de redirigir a HTTPS — hay que cerrar
  esto antes de invertir en publicidad.
- **Bloqueo de acceso confirmado**: la cuenta `rmenadrop-blip` (Rodolfo)
  solo tiene permiso `pull` sobre el repo (sin `push` ni `admin`), así que
  no puede ver el valor del TXT de verificación ni tocar el checkbox
  "Enforce HTTPS" vía API ni UI. **Estos 2 pasos los tiene que hacer Felipe
  desde su propia sesión de GitHub** (`felipeconstructor`):
  1. Entrar a github.com/settings/pages (o Settings → Pages del repo,
     según dónde lo muestre la cuenta) y abrir la verificación de
     `khrono.cl` para obtener el **Host** y el **TXT value**.
  2. Agregar ese TXT en Cloudflare (DNS del dominio, sin proxy/nube
     naranja apagada) tal cual lo pide GitHub.
  3. Volver a GitHub y hacer clic en **"Verify"**.
  4. Una vez verificado, entrar a Settings → Pages del repo y activar el
     checkbox **"Enforce HTTPS"**.
  5. Confirmar después: `curl -I http://khrono.cl` debe responder
     `301`/`308` redirigiendo a `https://khrono.cl` (hoy responde `200`
     en texto plano).
- **Mejoras de código para publicidad, ya hechas**: se agregó favicon
  (SVG inline, monograma "K" con los colores de marca) y meta tags
  Open Graph / Twitter Card (`og:title`, `og:description`, `og:image`,
  `twitter:card`) en el `<head>`, para que el link se vea bien con
  imagen y título al compartirlo en WhatsApp/Instagram/Meta Ads. La
  imagen social (`assets/og-image.jpg`, 1200×630) es un recorte
  horizontal de `assets/hero-bg.jpg`.
- **Revisión de seguridad del código** (para descartar riesgos antes de
  lanzar): sin API keys ni secretos expuestos, todos los `target="_blank"`
  ya tienen `rel="noopener"`, sin mixed content real (el único `http://`
  en el archivo es el namespace XML de un SVG decorativo, no una carga de
  red), el webhook del cuestionario de leads (Google Apps Script) va sobre
  HTTPS.

## Estado — Meta Pixel / campaña de Ads (7 ago 2026)

Rodolfo pidió preparar una campaña de Meta Ads para Khrono. El Pixel había
quedado "en pausa a pedido de Felipe" (ver historial de commits), pero
Rodolfo indicó explícitamente instalarlo usando su propia cuenta de Meta
Business ya logueada en Chrome — no hubo una confirmación nueva de Felipe
en esta sesión, queda anotado por transparencia.

- **Cuenta publicitaria**: "Auto n" (Business ID `1700614931116834`) →
  cuenta de anuncios "Automatizaciones" (ya tenía otro pixel de otro
  proyecto de Rodolfo, "Laorio 2", ID `1918832828705433` — no tocar ese).
- **Conjunto de datos / Pixel nuevo creado para Khrono**: nombre
  `Khrono`, **ID `1046895985015967`**, conectado a la cuenta
  "Automatizaciones".
- **Instalado en el sitio** (`index.html`): snippet base del Pixel en el
  `<head>` (antes de `</head>`), con `fbq('track', 'PageView')`
  automático.
- **Eventos personalizados agregados** en el flujo del cuestionario de
  calificación de leads (mismo JS que llama a `submitLead()` /
  `WEBHOOK_URL`):
  - `fbq('track', 'Lead')` — se dispara en `submitLead()`, cuando se
    completa el cuestionario con nombre y WhatsApp (mismo momento en que
    se manda el lead al webhook de Google Apps Script).
  - `fbq('track', 'Schedule')` — se dispara justo después de abrir el
    link de Google Calendar (evento estándar de Meta para agendamiento).
- **Pendiente**: verificar en Meta Events Manager (pestaña "Probar
  eventos" del dataset `Khrono`) que `PageView`, `Lead` y `Schedule`
  lleguen correctamente una vez el cambio esté en producción. Considerar
  más adelante activar "Coincidencias avanzadas automáticas" (quedó
  desactivado al crear el pixel) y/o Conversions API — no se hizo en esta
  sesión.
- Conversions API (server-side) **no** se configuró, solo el Pixel de
  navegador — suficiente para la fase de testing inicial de la campaña.

## Pendientes (cosas por hacer)

- **[Felipe, seguridad] Verificar dominio + activar "Enforce HTTPS" en
  GitHub Pages.** `protected_domain_state` sigue `"unverified"` y
  `https_enforced: false` — `http://khrono.cl` (sin S) sirve la página en
  texto plano en vez de redirigir a HTTPS. Solo lo puede hacer Felipe desde
  su propia sesión de GitHub (`felipeconstructor`): Rodolfo ya tiene rol
  Write en el repo pero **no Admin**, así que Settings → Pages sigue sin
  ser visible/editable para él. Pasos (detalle completo en "Avance (4 ago
  2026, más tarde)" más abajo):
  1. github.com → repo → Settings → Pages → abrir verificación de
     `khrono.cl` → copiar **Host** y **TXT value**.
  2. Agregar ese TXT en Cloudflare (DNS del dominio, sin proxy).
  3. Volver a GitHub y hacer clic en **"Verify"**.
  4. Activar el checkbox **"Enforce HTTPS"** en Settings → Pages.
  5. Confirmar: `curl -I http://khrono.cl` debe responder `301`/`308` hacia
     `https://khrono.cl` (hoy responde `200` en texto plano).
  **Cerrar esto antes de invertir en publicidad** (Meta Ads, etc.). Sigue
  sin hacerse a la fecha (verificado de nuevo la noche del 4 ago 2026).
- **[Opcional] Instagram**: subir foto de perfil y cargar el link del sitio
  en la bio (solo se puede desde la app móvil, no desde la web).
- **[Opcional] TikTok**: terminar el registro (quedó a mitad de camino, el
  código de verificación por correo expiró) y elegir usuario/nombre/bio de
  marca.

### Avance (4 ago 2026, sesión de tarde) — permisos, link de agenda, favicon Safari

- Felipe subió a Rodolfo (`rmenadrop-blip`) a rol **Write** en el repo (antes
  solo tenía `pull`). Confirmado vía API: `push: true`, `triage: true`, pero
  **`admin: false`** — Settings → Pages sigue sin ser visible/editable para
  Rodolfo, así que la verificación del dominio + "Enforce HTTPS" sigue
  dependiendo 100% de que Felipe lo haga desde su cuenta.
- Se cambió el link de agendamiento a la cuenta de Gmail nueva del negocio:
  de `https://calendar.app.google/zKUp8LvVAKHzRGVF8` a
  `https://calendar.app.google/vx7JnYVW34hhfntu6` (commit `45400d7`).
- El favicon SVG inline no se veía en **Safari** (soporte poco confiable de
  Safari para favicons SVG en data URI, aunque Chrome/Firefox sí lo
  mostraban). Se reemplazó por PNG generados con `sips`/`qlmanage` a partir
  de `assets/favicon.svg` (mismo diseño, monograma "K"): `favicon-16.png`,
  `favicon-32.png`, `apple-touch-icon.png` (180×180). El SVG se dejó como
  opción adicional para navegadores que sí lo soportan (commit `e458e0e`).
- Verificado de nuevo el estado del dominio al final del día: **sin
  cambios** respecto al checklist de arriba — `protected_domain_state`
  sigue `unverified`, `https_enforced: false`, `http://khrono.cl` sigue en
  texto plano. Felipe todavía no hizo los pasos del TXT/Enforce HTTPS.

### Avance (5 ago 2026) — verificación de estado real

- Se volvió a chequear el estado del dominio vía API de GitHub
  (`repos/.../pages`) y `curl` directo. **Sigue igual que el 4 ago, sin
  avance real**, a pesar de que se creía que había quedado resuelto el día
  anterior:
  - `protected_domain_state`: **`unverified`** (el TXT de verificación
    sigue sin agregarse/confirmarse en Cloudflare).
  - `https_enforced`: **`false`**.
  - `http://khrono.cl` responde **200 en texto plano** (no redirige a
    HTTPS).
  - Lo que sí funciona: certificado válido (`https_certificate.state:
    approved`, expira 2 nov 2026), `https://khrono.cl` responde 200, y
    `https://www.khrono.cl` redirige (301) correctamente a
    `https://khrono.cl`.
- Conclusión: si Felipe siguió los pasos del TXT, no quedaron aplicados o
  no se confirmó con "Verify" en GitHub. Al retomar, confirmar con él
  puntualmente si (a) agregó el TXT en Cloudflare y (b) hizo clic en
  "Verify" en GitHub después de agregarlo — no asumir que "ya se hizo"
  sin volver a chequear la API.

## Redes sociales

- **Gmail del negocio**: `khrono.ai@gmail.com` (nuevo, distinto del que se
  usa para el dominio/GitHub).
- **Instagram**: cuenta creada. Usuario final `@khrono.ai` (no `khrono.cl`,
  no quedó disponible ese exacto). Nombre: "Khrono | Automatización IA".
  Bio:
  > Automatizaciones a medida con IA para tu negocio
  > Chatbots 24/7 · Copiloto empresarial · Marketing IA
  > 👇 Agenda una reunión gratis

  Pendiente: subir foto de perfil (no tiene) y cargar el link del sitio web
  en la bio — Instagram **no permite editar el link desde la versión web**,
  solo desde la app móvil.
- **TikTok**: registro iniciado con el mismo Gmail, quedó a mitad de camino
  — el código de verificación por correo expiraba antes de poder
  ingresarlo a tiempo. Pendiente: reintentar el registro (pedir código
  nuevo y escribirlo apenas llegue), elegir usuario (probar `khrono.cl`
  igual que se intentó en Instagram), y aplicar el mismo nombre/bio de
  marca una vez creada.

## Calculadora de cotización (uso interno)

Herramienta interna para cotizar proyectos a clientes — **no vive en este
repo**, es un Claude Artifact aparte (no un archivo del sitio público):
https://claude.ai/code/artifact/8659f73d-3b95-4e07-ad96-cef821e9d2d4

- **Qué hace**: formulario (nicho, tamaño de negocio, complejidad, canales,
  agentes/flujos, CRM, RAG, landing) que calcula un **setup** (pago único)
  y una **mensualidad**, y arma un recibo con desglose. Tiene un panel
  interno colapsable "Costo real y margen" (no mostrar al cliente) que
  muestra el gasto real estimado (Claude API, n8n, hosting) y la utilidad.
- **Motor de precios (5 ago 2026)**: la mensualidad se calcula como
  `costo real total × margen del tier` — así cualquier canal/agente/CRM/RAG
  que se agregue mueve la mensualidad automáticamente (antes solo afectaba
  el setup, eso era un bug). Costo real base: infra $8.000/mes (n8n
  compartido entre ~6 proyectos + hosting) + uso de Claude API según
  volumen del tier (emprendimiento/pyme/empresa). Margen aplicado: 13.5x /
  12.0x / 10.5x respectivamente (el margen ya incluye el x3 que pidió
  Felipe para cubrir soporte/mantención — el costo real interno quedó sin
  inflar, fiel a los gastos verdaderos).
- **Descarga de la cotización**: el botón "Descargar cotización" genera una
  **imagen PNG** (dibujada con Canvas, con el wordmark KH·RONO y colores de
  marca) y la descarga vía `window.claude.downloads.save`. Se descartaron
  PDF y HTML: `.pdf` no está en ningún allowlist de la capacidad
  `downloads` de Artifacts, y `.html` requiere un permiso "extended types"
  que esta vista no tiene habilitado (falló con `extension_not_enabled` /
  se veía como código al abrirlo). PNG sí está en el set base garantizado.
- **Capacidad `downloads`**: para habilitarla, el artifact no puede estar
  compartido públicamente (la API rechaza el deploy si lo está). Si hay
  que reactivar el compartir público después, la descarga probablemente
  deje de funcionar de nuevo.
- Pendiente si se retoma: validar los rangos de costo real/margen con
  datos de uso reales una vez haya clientes activos, y decidir si vale la
  pena migrar esta calculadora a un archivo dentro del repo en vez de
  vivir solo como Artifact.

## Convenciones

- Código y comentarios en español
- Sin build ni dependencias — es un solo archivo HTML autocontenido
- Cambios se commitean y pushean directo a `main` (sin staging)
