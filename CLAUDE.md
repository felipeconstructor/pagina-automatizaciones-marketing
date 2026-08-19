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
- Tarjetas de servicio con foto real de fondo, las 6: Copiloto empresarial,
  Chatbot 24/7, Marketing con IA, Landing pages, RAG de datos y "Cotiza con
  nosotros" (`assets/card-*.jpg`) + badge superpuesto con degradado oscuro
  para legibilidad. Patrón `.card-visual.has-photo` +
  `<img class="card-photo">` + `<span class="card-badge">`; el
  `object-position` de cada foto se controla por selector
  `[src*="card-X"]` en el CSS. Últimas 2 tarjetas (RAG y Cotiza) se
  convirtieron de mockup CSS a foto real el 7 ago 2026, commit `69bbbed`.
- Botón flotante de WhatsApp (`.wa-float`, esquina inferior derecha, anillo
  animado + pulso) a `+56996996933`, agregado 7 ago 2026, commit `69bbbed`.
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
- **Tracking de visitas + leads (Google Sheet + panel en vivo)**: instalado
  (10 ago 2026), ver sección "Panel de leads en tiempo real" más abajo.
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

### Avance (7-9 ago 2026) — estructura de campaña armada en Ads Manager

Campaña completa armada como **borrador** (no publicada) en la cuenta
"Automatizaciones" (Ads Manager), pendiente de revisión final de Rodolfo
antes de publicar. Presupuesto de testeo acordado: **$150.000-300.000
CLP/mes**, arrancando con **$3.000 CLP/día por conjunto**.

- **Campaña**: "Khrono - Leads WhatsApp/Automatización - Ago 2026",
  objetivo Clientes potenciales, Advantage+ campaign budget desactivado
  (presupuesto a nivel de conjunto, no de campaña, para poder comparar
  conjuntos entre sí), "compartir 20% del presupuesto entre conjuntos"
  desmarcado a propósito.
- **Público personalizado de retargeting** ya creado (independiente de
  que el conjunto 3 esté activo o no, así empieza a acumular gente desde
  ya): "Khrono - Visitantes sin completar cuestionario (30d)" — sitio web,
  30 días, excluye a quienes completaron el evento `Lead`.
- **3 conjuntos de anuncios**, todos con: conversión = Sitio web (no
  Formularios instantáneos — Meta lo sugiere/preselecciona por defecto,
  hay que cambiarlo a mano cada vez), conjunto de datos = `Khrono`,
  evento de conversión = `Cliente potencial`, destino `https://khrono.cl`,
  complemento del navegador = Ninguno:
  1. **"01 - Audiencia amplia (frío)"** — Chile, sin intereses, deja que
     Advantage+ explore. Fecha de inicio programada: 8 ago 2026, 05:00
     GMT-4. 3 anuncios (imágenes de `ads-khrono/conjunto-1-amplio/`).
  2. **"02 - Audiencia intereses"** — mismo público base + intereses
     (marketing digital, automatización, pyme, WhatsApp Business). 3
     anuncios (`ads-khrono/conjunto-2-intereses/`).
  3. **"03 - Retargeting"** — usa el público personalizado de arriba en
     vez de segmentación por intereses. Fecha de inicio corrida ~10 días
     después de las otras dos (para dar tiempo a que la audiencia
     acumule volumen real desde que se creó) — chequear tamaño de
     audiencia en Públicos antes de esa fecha y correrla más si sigue
     chica. 3 anuncios (`ads-khrono/conjunto-3-retargeting/`).
- **Copys por conjunto** (texto principal / título / descripción / CTA):
  - Conjunto 1: "¿Cuántos clientes se te escapan por no responder a
    tiempo? Automatizamos tu WhatsApp para que respondas en segundos,
    24/7." / "Deja de perder clientes por responder tarde" / "Agenda una
    reunión gratis" / CTA **Más información**.
  - Conjunto 2: "Mientras atiendes tu negocio, un asistente con IA
    responde por ti en WhatsApp y agenda reuniones solo." / "Todo
    respondido, sin mover un dedo" / "Conoce cómo automatizarlo" / CTA
    **Más información**.
  - Conjunto 3: "Todavía puedes agendar tu reunión gratis. Toma 2
    minutos y te mostramos cómo automatizar tu negocio." / "Tu reunión,
    a un clic de distancia" / "Agenda ahora, es gratis" / CTA
    **Reservar**.
  - CTA se decidió distinto por conjunto a propósito: "Reservar" implica
    una acción de compromiso inmediato que no calza con audiencia fría
    (el clic lleva primero al cuestionario de calificación, no a un
    calendario directo) — "Más información" es más honesto para
    conjuntos 1 y 2; "Reservar" sí calza en el 3 porque esa audiencia ya
    conoce Khrono.
- **Imágenes**: las 9 fotos base (generadas con ChatGPT/DALL·E a partir
  de los prompts documentados en la conversación, no en este repo) están
  organizadas en `~/Documents/Automatizaciones/ads-khrono/` por conjunto,
  cada una con su versión `-texto.png` (overlay de texto agregado en
  Canva por Rodolfo: Fraunces, `#F7F5F2` con una palabra de énfasis en
  `#FF6B35`, ubicado en el tercio inferior para no tapar cara/producto).
- **Pendiente crítico antes de publicar** (único punto que sigue sin
  resolver al 9 ago 2026 — los demás se corrigieron, ver avance de abajo):
  1. **No existe una página de Facebook para Khrono.** El anuncio sigue
     con la página personal de Rodolfo ("RodolfoMena") como placeholder
     en los 9 anuncios (confirmado de nuevo el 9 ago) — hay que crear
     una página de Facebook de marca y reasignarla antes de publicar de
     verdad, si no el anuncio se muestra como publicado por Rodolfo, no
     por Khrono.
  2. **Condiciones del servicio de generación de clientes potenciales de
     Meta sin aceptar** en esa página — Meta bloquea publicar hasta
     aceptarlas (botón "Ver Condiciones" en el panel de errores del
     anuncio). Depende del punto 1 (hay que aceptarlas desde la página
     de marca real).
- **Estado al cierre de la sesión del 7 ago**: quedó en medio de la
  revisión final conjunto por conjunto (verificando presupuesto, fechas,
  audiencia, imagen y copy de cada uno de los 9 anuncios) cuando se
  cortó la sesión — no se alcanzó a terminar de revisar los conjuntos 2
  y 3 ni a confirmar que los 9 anuncios individuales tengan la imagen y
  el copy correctos. **Retomar revisando anuncio por anuncio antes de
  publicar**, no asumir que quedó completo.

### Avance (9 ago 2026) — revisión profesional y corrección de bugs de segmentación

Rodolfo pidió una revisión de la campaña "como especialista en marketing
digital de Meta". Se encontraron y corrigieron 3 problemas reales en el
borrador (verificados directo en Ads Manager, no solo por lo documentado
arriba):

- **Bug crítico de segmentación (ya corregido)**: los 3 conjuntos
  (`01-Audiencia amplia`, `02-Audiencia intereses` y `03-Retargeting`)
  tenían cargado el mismo público personalizado "Khrono - Visitantes sin
  completar cuestionario" como filtro de "Incluir" — probablemente quedó
  pegado al duplicar el conjunto de retargeting para crear los otros dos.
  Efecto: los 3 conjuntos marcaban audiencia estimada "menos de 1.000" y
  competían por el mismo puñado de gente, incluido el conjunto que debía
  llegar a todo Chile en frío. Se sacó esa audiencia personalizada de
  `01` y `02` (queda correctamente solo en `03-Retargeting`).
  - `01-Audiencia amplia`: pasó de <1.000 a **16,9M-19,9M** de audiencia
    estimada.
  - `02-Audiencia intereses`: además no tenía ningún interés real cargado
    pese al nombre y a lo documentado arriba (marketing digital,
    automatización, pyme, WhatsApp Business nunca se habían aplicado). Se
    cargaron los intereses reales disponibles en Meta: **Digital
    marketing (marketing)**, **Automatización de marketing** y
    **Pequeñas y medianas empresas (negocios y finanzas)**. "WhatsApp
    Business" no existe como categoría de interés en Meta (solo devuelve
    comportamientos genéricos de dispositivo móvil), así que no se forzó.
- **Segmentación geográfica de `02-Audiencia intereses` acotada**: tenía
  "Chile" (país completo) sin restricción, lo cual diluye el presupuesto
  bajo de testeo en zonas de baja densidad de pymes. Se cambió a
  **Región Metropolitana + Valparaíso (radio 40km desde la ciudad,
  cubre Viña del Mar/Quilpué/Villa Alemana/Concón) + Concepción/Biobío
  (radio 40km)**. Audiencia final: **5,3M-6,2M** (antes 16,9M-19,9M con
  el bug de arriba corregido, o <1.000 con el bug sin corregir).
- **"Compartir 20% del presupuesto entre conjuntos" había quedado
  activado** pese a que se documentó arriba como "desmarcado a
  propósito" — se volvió a desmarcar a nivel de campaña.
- **Fechas de inicio verificadas, sin cambios**: `01` y `02` arrancan
  juntos el **10 ago 2026 05:00 GMT-4** (no quedaron en el pasado como se
  temía) y `03-Retargeting` el **20 ago 2026** — los 10 días de
  diferencia siguen intactos tal como se planeó.
- Sigue en **borrador**, no se publicó nada en esta sesión. Pendiente
  real antes de publicar: el punto 1 de la lista de arriba (página de
  Facebook de marca) — todo lo demás en la campaña ya quedó corregido.
- Recordatorio operativo: toda la campaña quedó en **borrador/pausada**
  a propósito para que Rodolfo la revise — si se deja así, **no** va a
  arrancar sola en la fecha programada (8 ago 05:00 quedó en el pasado
  sin activarse). Para lanzar de verdad hay que resolver los pendientes
  de arriba y luego activar manualmente cada conjunto (o pedirle a
  Claude que lo haga).

### Avance (11 ago 2026) — página de Facebook de marca creada

Se creó la página de Facebook de marca que faltaba (punto 1 del pendiente
de arriba), vía Meta Business Suite (Claude en Chrome, sesión de Rodolfo).

- **Nombre**: Khrono. **Categoría**: Empresa de software. **Presentación**:
  "Automatizaciones con IA: chatbots 24/7, copiloto empresarial y marketing
  IA para tu negocio."
- **Portfolio comercial**: se creó dentro de **"Auto n"** — el portfolio
  que contiene la cuenta publicitaria "Automatizaciones" (ID
  `982447667818471`), la misma donde vive la campaña de Ads de Khrono. Se
  eligió a propósito ese portfolio y no otros que administra Rodolfo
  (Laorio, CRnexo, Prolig propiedades — negocios distintos) para no mezclar
  el acceso administrativo.
- **Foto de perfil: pendiente a propósito.** Rodolfo prefirió subirla él
  mismo más tarde (imagen del robot/mascota de marca Khrono que ya tiene
  preparada) en vez de que se subiera en esta sesión — no se generó ni se
  guardó ningún archivo de imagen para esto en el repo ni en el disco.
- **Reasignación de los 9 anuncios: hecha por Rodolfo mismo (11 ago 2026,
  ~30h después de que la campaña empezó a correr)**. Se decidió hacer el
  cambio de inmediato en vez de esperar: con el presupuesto tan bajo
  (~$3.000 CLP/día por conjunto) la campaña casi no tenía aprendizaje
  acumulado a las 30h, y el cambio de página siempre reinicia la fase de
  aprendizaje del conjunto de anuncios y manda el anuncio a revisión de
  nuevo (edit "significativo" para Meta) — mejor pagar ese costo temprano
  que después de acumular más días de datos. Efecto esperado y aceptado:
  los anuncios entran a revisión otra vez (pausa de minutos a horas) y los
  likes/comentarios/shares de esos posts quedan en cero bajo la página
  nueva. **Pendiente todavía**: confirmar que la revisión de Meta terminó
  y los anuncios volvieron a estar activos, y aceptar las Condiciones del
  servicio de clientes potenciales de Meta en la página **Khrono** (antes
  bloqueado porque la página no existía).

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
- **[Meta Ads] Cerrar la transición a la página de marca.** Ver "Avance
  (11 ago 2026)" arriba — la página **Khrono ya existe** (portfolio "Auto
  n") y los 9 anuncios ya fueron reasignados a ella por Rodolfo. Falta:
  confirmar que pasaron la revisión de Meta post-cambio y volvieron a
  estar activos, aceptar las Condiciones del servicio de clientes
  potenciales de Meta en la página Khrono, y subir la foto de perfil
  (Rodolfo la sube él mismo, imagen de marca ya elegida pero sin guardar
  como archivo).

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

## Subagente "khrono" (Claude Code) — 10 ago 2026

Subagente custom en carpeta dedicada `.claude/agents/khrono/` (no archivo
suelto): la identidad del agente la da el campo `name` del frontmatter, no
la ruta.

- `khrono.md` — definición del agente (`model: opus`). Persona: empresario
  senior (40 años) + programador senior + investigador de mercado. Se
  invoca cuando hay que estructurar cómo entregar a un cliente cualquiera
  de los 8 servicios de la agencia (automatización integral, copiloto
  empresarial, marketing con IA, RAG/chatbot, CRM a medida, landing pages,
  agentes de IA, dashboard en vivo): aplica un framework de diagnóstico de
  3 preguntas, traduce el servicio a lenguaje de valor/negocio (nunca de
  tecnología), arma la ruta técnica ajustada al cliente, define la métrica
  de éxito a 90 días, y entrega el prompt de Claude listo para ese
  servicio.
- `conocimiento-servicios-ia.md` — base de conocimiento destilada del
  informe estratégico (ver más abajo): contexto de mercado, framework de
  venta sin vender IA, ruta técnica de los 8 servicios, stack recomendado,
  cómo priorizar inversión de crecimiento, radar de qué se vende más con
  IA.
- **Regla dura:** el agente nunca inventa, sugiere ni cita precios de
  KHRONO — siempre remite a la calculadora interna (ver "Calculadora de
  cotización" arriba). Tampoco cubre el negocio secundario de e-commerce
  (excluido a propósito de su conocimiento).
- Se invoca vía el tool Agent con `subagent_type: khrono`.

## Informes internos generados (carpeta `informes/`, no versionada al público)

- `KHRONO IA - Informe Estrategico (ago 2026).pdf` (31 páginas) — informe
  de mercado de agencias de IA, framework de venta por valor, los 8
  servicios con ruta técnica y precio sugerido, stack recomendado, plan de
  crecimiento a 12 meses, y apartado separado de e-commerce con IA como
  negocio secundario. Fuente original del contenido que nutrió al
  subagente `khrono` (sin los precios).
- `KHRONO IA - Prospectos La Ligua y Cabildo (ago 2026).pdf` — ~34
  negocios reales (Páginas Amarillas Chile) de La Ligua y Cabildo con
  nombre, dirección y teléfono, filtrados por rubro (inmobiliarias,
  ferreterías, restaurantes, clínicas dentales, supermercados) para
  prospección directa. No incluye Papudo/Zapallar/Petorca todavía.
  Números de directorio público, verificar vigencia antes de contactar en
  volumen.

## Panel de leads en tiempo real (uso interno, 10 ago 2026)

Rodolfo pidió ver en una pantalla cuánta gente entra a `khrono.cl`, cuántos
completan el cuestionario (leads) y cuántos agendan. Se armó en base a lo
que ya existía (el webhook de leads), sin levantar backend nuevo.

- **Los leads ya se guardaban** en la Google Sheet "Leads - Automatizaciones
  y Marketing" (`1hnfYzmISzzOu_aWaxLRYXSzO0EyVMyvu1bZxEUBLt3w`, dueño
  `rodolfomena051@gmail.com`) vía un Apps Script container-bound a esa
  Sheet (proyecto "leads"), desplegado como Web App en el mismo
  `WEBHOOK_URL` que usa `index.html` (`AKfycbxXMYm5Zos1PTH4U2VVcU5MzQGpjc3l_8wRtlOv8TUl6W2nglk5EQbCjoTVlZiBMnB0fA`).
- **Tracking de visitas agregado**: `index.html`, justo después de
  `fbq('track', 'PageView')`, hace un `fetch` fire-and-forget (mismo patrón
  que `submitLead()`) con `{tipo: 'visita', origen: window.location.href}`
  al mismo `WEBHOOK_URL`.
- **Apps Script actualizado** (`doPost`/`doGet` del proyecto "leads",
  editado vía el editor web, no vive en este repo git):
  - `doPost` ahora distingue por `data.tipo`: sin `tipo` (o `'lead'`) sigue
    escribiendo en la pestaña de leads exactamente igual que antes
    (compatibilidad total); `tipo === 'visita'` escribe en una pestaña
    nueva **"Visitas"** (Fecha, Origen), creándola sola si no existe.
  - Se cambió `getActiveSheet()` por `getSheetByName('Untitled')` — con
    `getActiveSheet()` el próximo lead se podía ir a la pestaña
    equivocada si alguien dejaba abierta la pestaña "Visitas" en el
    navegador (bug latente que se vuelve real en cuanto hay 2 pestañas).
  - Se agregó `doGet(e)`, público, que devuelve
    `{ visitas, leads, agendamientos, ultimaActualizacion }` (conteo de
    filas de cada pestaña). No requiere auth, mismo modelo de acceso que
    ya usaba `doPost`.
  - Desplegado como **nueva versión de la misma implementación** (no
    "nueva implementación"), así que `WEBHOOK_URL` no cambió.
  - **"Agendamientos" es un proxy** = mismo número que "Leads": el clic
    que completa el cuestionario es el mismo que abre Google Calendar
    (`index.html`, handler de `nextBtn`), no hay confirmación real de que
    la persona eligió un horario. Conectar la Google Calendar real
    (`khrono.ai@gmail.com`) queda pendiente si se necesita el dato exacto.
- **Dashboard**: Claude Artifact aparte (no vive en el repo, mismo patrón
  que la calculadora de cotización) —
  https://claude.ai/code/artifact/c4ee72fd-354c-4300-b4cd-566ecd8be9c1
  - Usa la capacidad `mcp` (conector de Google Drive del viewer) con
    `watchTool` sobre `read_file_content` leyendo directo la Sheet de
    leads — **no** pega contra el `doGet` del Apps Script (los Artifacts
    corren con CSP estricta y no pueden hacer `fetch` a un host externo
    arbitrario aunque el endpoint sea público; solo `downloads` y `mcp`
    están disponibles como capacidades). El contenido llega como texto
    markdown (tablas), se parsean las filas de cada pestaña para sacar
    los conteos.
  - Por eso el dashboard **requiere que quien lo abra tenga el conector
    de Google Drive conectado en su cuenta de claude.ai** y acceso a esa
    Sheet — no funciona para cualquiera, y por usar `mcp` el Artifact no
    se puede compartir públicamente.
  - Auto-refresco cada 30s (piso de la plataforma) **mientras la pestaña
    esté visible/enfocada** — la plataforma pausa el polling si la
    pestaña queda en segundo plano (comportamiento documentado, no un
    bug). Recargar la página siempre trae el dato más reciente. Botón
    "Actualizar ahora" fuerza un refresco manual.
  - Fuentes (Fraunces + Public Sans) embebidas como `data:` URI para
    respetar la paleta/tipografía de marca dentro de la CSP del Artifact.

## Nota — "no se ve actualizado" casi siempre es caché del navegador (7 ago 2026)

Después de pushear el botón de WhatsApp y las 6 fotos de tarjetas
(commit `69bbbed`), Felipe reportó que en su dispositivo las tarjetas se
veían "como antes" y el botón de WhatsApp no aparecía. Se verificó con
`curl -I` directo al servidor (HTML y assets `.jpg`) y **todo estaba
correcto del lado del servidor**: `Last-Modified` con la fecha/hora del
deploy, `Cache-Control: max-age=600`. Una pestaña nueva de Chrome (vía
Claude-in-Chrome) mostró la página perfecta, hero y botón incluidos.
Conclusión: era **caché local del navegador/dispositivo de Felipe**, no un
problema real del sitio. Antes de investigar más a fondo un reporte de
"no se ve bien"/"no aparece", conviene primero: (1) `curl -I` al dominio y
a los assets sospechosos para comparar `Last-Modified` con la hora del
último commit, (2) abrir en una pestaña nueva/incógnito, y solo si eso
también falla, asumir que es un bug real. Pedir recarga forzada
(`Cmd+Shift+R` / cerrar y reabrir el navegador del celular) resuelve la
mayoría de estos casos.

## Mini-agente de demo en el hero — "Haz tu agente gratis" (13 ago 2026)

Rodolfo pidió reemplazar la tarjeta estática del hero ("Automatización en
acción", un feed falso) por algo interactivo: que el visitante pueda
probar un agente de IA de verdad, elegir el rubro de su negocio, chatear
unos mensajes, y que después de una demo corta aparezca el CTA de agendar.
Decisión explícita de Rodolfo: **IA real (Claude), no un guion
preescrito** — el costo por demo es bajo y es más honesto mostrar
exactamente lo que Khrono vende.

- **Frontend** (`index.html`, sección hero + script al final del body):
  - `#demoRubros`: 5 chips (Restaurante, Clínica dental, Inmobiliaria,
    Ferretería/Tienda, Otro negocio) — mismos rubros que la prospección de
    La Ligua/Cabildo. Al elegir uno se oculta el selector y aparece el
    chat con un saludo inicial hardcodeado en el cliente (sin llamar a la
    API todavía, para que la apertura sea instantánea).
  - Cada mensaje del usuario dispara un POST a `WEBHOOK_URL` (mismo Apps
    Script que ya usan leads/visitas) con
    `{tipo:'demo_chat', rubro:'restaurante', historial:[...], mensaje:'...'}`
    — **sin `mode:'no-cors'`** (a diferencia de los otros POST del sitio),
    porque acá sí hace falta leer la respuesta real. Se verificó
    empíricamente que Apps Script SÍ permite leer la respuesta en modo
    `cors` normal para este caso (`res.type === 'cors'`, `res.ok === true`).
  - Tope de **4 mensajes de usuario** (`MAX_USER_TURNS`): al llegar al
    límite aparece `#demoPaywall`, un overlay absoluto sobre la tarjeta
    (blur + texto + el mismo botón "Agendar reunión gratis" que ya
    intercepta el script de calendario existente, sin código nuevo para
    eso) y un link "Probar otro rubro" que resetea el widget.
  - **Bug real encontrado y corregido**: los elementos con atributo
    `hidden` (`#demoChat`, `#demoPaywall`, `#demoRubros`) no se ocultaban
    porque sus propias reglas CSS (`.demo-chat { display: flex; }`, etc.)
    tienen más especificidad que el estilo por defecto de `[hidden]` y lo
    pisan. Se agregó `.demo-chat[hidden], .demo-paywall[hidden] { display:
    none; }` explícito (mismo patrón para `.demo-rubros[hidden]`) — este
    es un gotcha general de CSS, no específico de este proyecto, vale la
    pena recordarlo si se agregan más elementos con `hidden` a futuro.
- **Backend** (Apps Script `Código.gs`, mismo proyecto "leads"):
  - `DEMO_RUBROS`: mapa `rubro clave -> system prompt` completo (persona,
    tono, qué inventa, límites). El cliente solo manda la **clave** del
    rubro (`'restaurante'`, no el prompt) — a propósito, para que no se
    pueda usar el endpoint como proxy libre de Claude mandando un prompt
    arbitrario desde devtools. Si la clave no existe en el mapa, responde
    un mensaje fijo sin llamar a la API.
  - `handleDemoChat(data)`: valida `historial.length <= 10` como límite
    duro adicional server-side (defensa en profundidad además del tope de
    4 en el cliente), llama a `https://api.anthropic.com/v1/messages` con
    `model: 'claude-haiku-4-5-20251001'`, `max_tokens: 150` (respuestas
    cortas a propósito, control de costo). La API key vive en
    **Propiedades de secuencia de comandos** (`ANTHROPIC_API_KEY`), nunca
    en el código ni en el cliente.
  - **Autorización nueva requerida**: la primera vez que un Apps Script
    llama a un dominio externo nuevo (`UrlFetchApp.fetch` a
    `api.anthropic.com`), Google exige una re-autorización de scope
    (`script.external_request`) — Apps Script normalmente muestra un
    diálogo de consentimiento automático al ejecutar desde el editor,
    pero si la llamada está dentro de un `try/catch` propio, la excepción
    de autorización se atrapa silenciosamente y el diálogo nunca aparece.
    Hubo que crear una función temporal sin try/catch
    (`UrlFetchApp.fetch(...)` pelado) para forzar el diálogo, aprobarlo, y
    recién ahí las llamadas dentro de `handleDemoChat` empezaron a
    funcionar. **Problema aparte que apareció en el camino**: el flujo de
    consentimiento de Google falló con 403 varias veces porque Chrome
    tenía más de una cuenta de Google logueada y el link abría el
    consentimiento con `authuser=2` (cuenta equivocada) — se resolvió
    editando la URL a mano (`authuser=0`) para forzar la cuenta correcta
    (`rodolfomena051@gmail.com`). Si se vuelve a tocar este script y hay
    que re-autorizar algo, tener esto presente antes de asumir que el
    proyecto está mal configurado.
- **Verificado en producción**: conversación completa de punta a punta
  (rubro → 4 intercambios con respuestas contextuales reales de Claude →
  paywall → botón abre el modal de agendar existente) probada en
  `khrono.cl` antes de dar la feature por terminada.
- **Pendiente si se retoma**: no hay rate-limiting real por IP/sesión más
  allá del tope de 4 mensajes del lado del cliente y el límite de 10 de
  historial del lado del servidor — si el tráfico crece mucho o alguien
  abusa deliberadamente del endpoint, conviene agregar algo más estricto
  (por ejemplo, throttling por IP en el propio Apps Script o mover esto a
  un servicio con mejor control de cuota).

### Adaptación mobile (13 ago 2026)

Ajustes para que el mismo widget sea cómodo en celular (mayoría del
tráfico de Ads es mobile):

- `@media (max-width: 640px)`: chips de rubro en grilla de 2 columnas
  (antes solo `flex-wrap`), botones con más padding para el dedo, el
  input del chat sube a `font-size: 16px` — **abajo de 16px, iOS hace
  zoom automático de toda la página al enfocar el campo**, un problema
  real de usabilidad que no se nota probando en desktop.
- `@media (max-width: 900px)` (mismo breakpoint donde `.hero-grid` pasa a
  una columna): `.widget-wrap { order: -1; }` — en mobile el widget del
  demo aparece **antes** que el texto del hero, no después. Decisión a
  propósito: en mobile la gente hace scroll rápido y se quería que la
  parte interactiva "enganche" antes de que se aburran con el copy. El
  orden en el DOM no cambió (SEO/lectores de pantalla siguen viendo el
  texto primero), solo el orden visual vía CSS `order`.
- JS: al hacer foco en el input del chat, se hace
  `demoCard.scrollIntoView({block:'center'})` con un pequeño delay — para
  que el teclado del celular no tape la conversación justo cuando el
  usuario empieza a escribir.
- **Importante — cómo se verificó (o no)**: `resize_window` de Claude en
  Chrome **no cambia el viewport real** en este entorno (se confirmó con
  `window.innerWidth` después del resize: seguía en el ancho de
  escritorio) — es la misma limitación ya documentada arriba en la
  sección de favicon/mobile de jul 2026. Estos cambios se escribieron a
  partir de la matemática real de anchos (`.container` padding 24px +
  `.widget-card` padding 18px en mobile) y buenas prácticas conocidas,
  pero **no se verificaron visualmente en un dispositivo real dentro de
  esta sesión** — si se retoma, pedirle a Rodolfo que confirme en su
  celular antes de asumir que quedó perfecto.

**Corrección (13 ago 2026, más tarde)**: Rodolfo mandó screenshots reales
de su iPhone — la primera pasada de arriba (aunque técnicamente
funcional) se veía "muy cargada, colapsada, apretada". El problema real
no era el widget del demo en sí (se veía bien organizado), sino que **al
poner el widget primero (`order:-1`) se agregó un bloque completo más al
stack vertical de la parte alta del hero**, y el espaciado entre bloques
(gap del grid, margin-top de stat-row, padding entre secciones) no se
había ajustado para compensar esa densidad extra. Se aumentó espaciado en
mobile de forma consistente — siempre sumando respecto al valor base de
escritorio, nunca restando:
- `.hero-grid` gap (900px): 32px → 52px (separación clara entre el
  widget del demo y el título del hero que viene después).
- `.stat-row` margin-top (900px): 46px base → 48px; gap 14px → 14px en
  mobile específico.
- `.hero` padding (560px): de `40px 0 32px` a `36px 0 56px` — más aire
  antes de pasar a la sección de servicios.
- `.contrast, .services, .why` padding (560px): 60px → 76px.
- `.section-head-block` margin-bottom (560px): 48px base → 56px.
- `.services-head-row` margin-bottom (560px): 40px base → 44px.
- `.stat-pill` padding, `.contrast-col` padding, `.team-grid` gap: todos
  con un poco más de aire interno en mobile.
- **Ojo con esto**: en el primer intento de este ajuste, por error se
  escribieron un par de valores que *reducían* el espaciado respecto al
  base de escritorio (contrario a lo pedido) y una regla `row-gap` en
  `.services/.contrast/.why` que no hacía nada porque esas secciones son
  bloques normales, no grid/flex — se corrigió antes de subir. Si se
  ajusta espaciado de nuevo, comparar siempre contra el valor base (sin
  `@media`) para confirmar que el cambio va en la dirección correcta.
- Sigue sin verificarse en dispositivo real — pedirle a Rodolfo que
  confirme con capturas nuevas.

**Segunda corrección (13 ago 2026, aún más tarde)**: con screenshots
nuevas del iPhone real de Rodolfo, confirmó que los espacios de la
corrección anterior sí se aplicaron (ya no hay elementos tocándose), pero
seguía sintiendo la página "apretada". Diagnóstico correcto esta vez: no
era un problema de margen entre bloques, sino de **densidad visual** —
demasiados elementos con borde/fondo tipo "caja" apilados uno tras otro
(el widget, el pill, cada stat, cada tarjeta de servicio), y el carrusel
de tarjetas de servicio mostraba un pedazo cortado de la siguiente
tarjeta en el borde de la pantalla, que se ve inconcluso. Se le preguntó
a Rodolfo la dirección a seguir (menos cajas vs. acortar contenido vs.
más números de espaciado) — eligió **menos cajas**. Cambios, solo en
`@media (max-width: 560px)`, sin tocar desktop:
- `.stat-pill`: se le sacó `border`/`background`/`border-radius`, queda
  como texto plano con un borde izquierdo de 3px color acento
  (`border-left: 3px solid var(--accent)`) en vez de una caja completa.
  `.stat-row` gap subido a 26px para compensar la falta del padding
  interno de la caja.
- `.service-card`: `flex-basis` pasó de un valor fijo (258px, dejaba
  ~80px de la siguiente tarjeta cortados a la vista) a
  `calc(100% - 44px)` — cada swipe del carrusel ahora muestra **una
  tarjeta completa** por pantalla, con un peek consistente y prolijo del
  borde de la siguiente en vez de un corte abrupto. `.services-scroll`
  gap bajado a 16px para acompañar.
- **Esta vez sí se verificó visualmente**: por primera vez en la sesión,
  `resize_window` (Claude en Chrome) funcionó — pero de forma poco
  confiable (no siempre respeta el ancho exacto pedido, a veces no
  cambia nada, a veces cambia en una llamada posterior sin patrón claro).
  Se logró un viewport real de ~500px de ancho (confirmado con
  `window.innerWidth` vía JS) y con eso se pudo confirmar visualmente
  que los stats quedaron sin caja y la tarjeta de servicio se ve completa
  sin corte. Si se necesita verificar mobile de nuevo, intentar
  `resize_window` en una pestaña recién creada (antes de navegar) — ahí
  tuvo más chance de funcionar que en una pestaña ya existente — pero no
  asumir que va a funcionar, seguir confirmando con `window.innerWidth`.

## Auditoría de seguridad completa (14 ago 2026)

Rodolfo pidió una auditoría completa para descartar riesgos de hackeo/robo
de datos. Se revisó todo lo que compone khrono.cl: `index.html` (estático,
GitHub Pages), el backend real en Apps Script (`Código.gs`, proyecto
"leads", no vive en este repo git), la Google Sheet de leads, el
despliegue del webhook, y el estado de HTTPS/dominio.

### Hallazgos corregidos en esta sesión (Código.gs, redesplegado como
### Versión 7, mismo `WEBHOOK_URL`)

1. **Fuga de errores internos al cliente**: el `catch` de `doPost`
   devolvía `{ok:false, error: String(err)}` a quien sea que llamara el
   webhook — cualquiera podía provocar errores a propósito y aprender
   detalles internos. Ahora el error se loggea server-side
   (`Logger.log`) y al cliente solo se le devuelve `{ok:false}`.
2. **Inyección de fórmulas en Google Sheets (formula/CSV injection)**:
   `doPost` escribía `data.nombre`, `data.inversion`, `data.problema`,
   `data.urgencia`, `data.origen` directo en celdas sin sanitizar. Como
   el webhook es público (la URL está en el código fuente de
   `index.html`, visible para cualquiera), alguien podía mandar un POST
   directo (sin pasar por la landing) con un valor tipo
   `=HYPERLINK("http://evil.com";"click")` y quedaba vivo en la Sheet —
   riesgo real si alguien del equipo abre la Sheet y hace clic, o si la
   fórmula usa algo como `IMPORTXML` para exfiltrar datos. Se agregó
   `sanitizeCell()`: antepone un apóstrofe a cualquier valor que empiece
   con `=`, `+`, `-` o `@` antes de escribirlo, forzándolo a texto
   plano. **A propósito no se aplicó a `data.whatsapp`** — ese campo ya
   tenía protección propia vía `setNumberFormat('@')` en la columna, y
   aplicarle el mismo prefijo habría roto números reales que empiezan
   con `+` (formato internacional `+569...`).
3. **Sin tope de gasto en la demo de IA**: no había ningún límite más
   allá del tope de 4 mensajes del lado del cliente (fácil de saltar
   recargando la página) y el límite de 10 mensajes de historial del
   lado del servidor. Se agregó `checkGlobalDemoLimit()`: un tope
   **global** (no por visitante — Apps Script no expone la IP de quien
   llama, así que no se puede limitar por IP) de 40 mensajes a la API de
   Claude cada 10 minutos, usando `CacheService`. No impide que una sola
   persona abuse mandando requests directo al webhook, pero pone un
   techo duro al gasto total posible si el tráfico se dispara o alguien
   ataca el endpoint en loop.

Los 3 fixes se verificaron en producción después del redeploy: `doGet`
responde bien (`curl`), y el chat del hero respondió con contenido real
de Claude en una prueba end-to-end en `khrono.cl` (Claude en Chrome).
**Nota para quien retome esto**: probar el webhook con `curl -X POST`
falla con un error genérico de Google Drive a menos que se agreguen
`--post302 --post303` — y aun así puede fallar, porque Apps Script
maneja el redirect 302 a la URL de ejecución real de forma particular
que `curl` no siempre replica bien. La forma confiable de probar es
`fetch()` real en un navegador (como hace la propia landing), no `curl`.

### Hallazgos que ya estaban bien (verificado, no se tocó nada)

- **Sin XSS**: todo el contenido dinámico se inserta con
  `textContent` (mensajes del chat, incluida la respuesta de la IA) o
  pasa por `escapeHtml()` (el único campo que se reinyecta como HTML es
  `contact.name` en la pantalla de éxito del cuestionario, y ya estaba
  bien escapado).
- **Sin secretos expuestos**: se revisó `index.html` completo y todo el
  historial de git — no hay API keys ni tokens hardcodeados en ningún
  commit.
- **`ANTHROPIC_API_KEY`** vive en Propiedades de secuencia de comandos
  de Apps Script, nunca llega al cliente ni al código fuente.
- **El demo no es un proxy libre de Claude**: `handleDemoChat` solo
  acepta una clave de rubro de una lista cerrada de 5 (`restaurante`,
  `clinica`, `inmobiliaria`, `ferreteria`, `otro`) — el system prompt
  real nunca lo manda el cliente, así que no se puede usar el endpoint
  para generar contenido arbitrario con un prompt propio.
- **Todos los `target="_blank"` tienen `rel="noopener"`** (protegidos
  contra tabnabbing). Sin mixed content real (el único `http://` en el
  archivo es el namespace XML de un SVG decorativo).
- **La Google Sheet de leads es privada**: verificado directo vía API
  de Google Drive (`get_file_permissions`) — el único permiso listado es
  el de Rodolfo como owner, no está compartida ni con "cualquiera con el
  link" ni con ninguna otra cuenta.
- **Un solo despliegue activo del Apps Script**: se revisó "Gestionar
  implementaciones" — no hay URLs viejas/abandonadas con código
  desactualizado todavía accesibles; las versiones archivadas solo
  existen como historial, no como endpoints separados. `Ejecutar como:
  Yo (rodolfomena051@gmail.com)` / `Quién tiene acceso: Cualquier
  usuario` es la configuración correcta y necesaria para que el fetch
  desde la landing funcione sin login — no es una mala configuración.
- **Meta Pixel** no manda ningún dato personal (nombre/teléfono) en los
  eventos `Lead`/`Schedule`/`PageView` — son triggers sin payload
  custom.

### Pendiente real — no se puede arreglar desde el código (14 ago 2026)

**El hallazgo más serio de toda la auditoría sigue siendo el mismo
bloqueado hace semanas**: `http://khrono.cl` (sin S) sigue sirviendo la
página completa **en texto plano**, sin redirigir a HTTPS
(`protected_domain_state: "unverified"`, `https_enforced: false`,
confirmado de nuevo vía API de GitHub y `curl -I` el 14 ago). Esto
importa para la auditoría porque un atacante en posición de
intermediario (wifi pública, router comprometido) podría **inyectar
JavaScript malicioso en la página mientras viaja sin cifrar** y desde
ahí robar cualquier dato que el visitante ingrese o redirigir los
botones de agendar/WhatsApp a un sitio de phishing — es el único
hallazgo de esta auditoría que compromete la integridad de la página
misma, no solo del backend. Depende 100% de que Felipe
(`felipeconstructor`, único Admin del repo) complete la verificación de
dominio + "Enforce HTTPS" en GitHub Pages (pasos detallados en la
sección "Estado — dominio propio" más abajo) — Rodolfo tiene rol Write
pero no Admin, confirmado de nuevo hoy vía API
(`admin:false`).

**Camino alternativo que no depende de Felipe**: Cloudflare ya está
delante del DNS de `khrono.cl` (ver "Estado — dominio propio"). Si
Rodolfo tiene acceso al dashboard de Cloudflare de este dominio, se
puede forzar HTTPS ahí mismo (regla "Always Use HTTPS" o una Page Rule)
sin depender de que GitHub verifique el dominio — no se intentó en esta
sesión porque no se confirmó el acceso a Cloudflare. Cloudflare también
permitiría agregar headers de seguridad (`Strict-Transport-Security`,
`Content-Security-Policy`) que GitHub Pages no deja configurar
directamente y que hoy no están presentes en ninguna respuesta del
sitio.

## Hero menos saturado, servicios apilados y caso de éxito Prolig (14 ago 2026)

Rodolfo mandó capturas reales de su iPhone del hero — se veía "muy saturado".
Cambios directos, sin re-verificación adicional en dispositivo real esta vez
(se probó con `python3 -m http.server` local + Claude en Chrome a ~500px de
ancho, suficiente para confirmar el layout mobile):

- **Hero**: se sacó el bloque de 3 stats (`24/7` / `<10s` / `0 leads sin
  seguimiento`) que iba debajo del botón "Agendar reunión gratis" —
  eran datos genéricos sin respaldo real, además de saturar. El
  mini-agente de demo (`.widget-wrap`) vuelve a aparecer **debajo** del
  texto principal en mobile: se sacó `.widget-wrap { order: -1; }` del
  media query de 900px (esa regla lo ponía primero, agregada el 13 ago
  y ahora revertida a pedido explícito). En desktop sigue side-by-side
  (grid 2 columnas), eso no se tocó.
- **Servicios**: pasaron de carrusel horizontal (`overflow-x: auto` +
  scroll-snap + botones prev/next + hint de swipe) a un **grid que
  crece hacia abajo** — `.services-grid` con `repeat(3, 1fr)` en
  desktop, `repeat(2, 1fr)` a los 900px, `1fr` a los 560px. Se borraron
  `.services-scroll-wrap`, `.scroll-nav`, `.scroll-hint` y su JS (ya no
  hay nada que scrollear). Las descripciones de cada tarjeta se
  acortaron a 1-2 líneas (antes eran párrafos largos) — el `card-meta`
  (implementación/ideal para) se dejó igual, no era lo que saturaba.
- **Sección nueva "Caso real" (`#caso-exito`)**, entre servicios y
  "Así se pierden clientes hoy": cuenta que Prolig (la corredora de
  propiedades de Felipe, cofundador) fue el primer negocio en usar el
  sistema antes de vendérselo a nadie más. **Reutiliza datos ya
  validados** de la sección "Quiénes somos" (90% de procesos
  automatizados, ahorro equivalente a 2 sueldos) en vez de inventar
  métricas nuevas sin confirmar — Rodolfo confirmó explícitamente que
  Prolig es esa misma corredora antes de escribir el texto. Sin
  imagen/logo (Rodolfo no tenía uno a mano), estilo consistente con
  `.contrast`/`.why` (secciones de solo texto). Reutiliza el
  componente visual `.stat-pill/.stat-num/.stat-label` que quedó libre
  al sacarlo del hero, en vez de duplicar CSS — en mobile (<560px) esos
  pills ya tenían el estilo "sin caja, borde izquierdo" del ajuste de
  densidad del 13 ago, así que la nueva sección hereda esa consistencia
  gratis.
- **Pendiente si se retoma**: no se verificó en un iPhone real después
  de este cambio (solo emulación ~500px vía Claude en Chrome) —
  confirmar con Rodolfo que el hero ya no se siente saturado y que el
  grid de servicios se ve bien en su celular antes de darlo por
  cerrado del todo.

## Caso de éxito Prolig: rediseño más visual, movido antes de servicios (14 ago 2026, más tarde)

Rodolfo vio la sección "Caso real" recién publicada y pidió que fuera más
llamativa — "que la gente se detenga a verlo" — y que se le diera más
énfasis que a servicios, con más imágenes, letras más grandes y menos
descripción.

- **Posición**: la sección `#caso-exito` se movió de entre
  servicios/contrast a **justo después del `</header>`**, antes de
  `#servicios` — ahora `#servicios` queda más abajo en el orden de la
  página (hero → caso de éxito → servicios → problema → quiénes somos →
  CTA final).
- **Imágenes reales, no gráficos genéricos**: Rodolfo compartió que
  Prolig tiene sitio propio y real en **prolig.cl** ("esa es la marca,
  ahí ve tú cómo la incorporas") — se capturaron 2 screenshots reales
  con Claude en Chrome (`assets/case-prolig-hero.jpg`: el hero real de
  prolig.cl con logo "PRO LIG" y tagline; `assets/case-prolig-pasos.jpg`:
  su sección "Solo tres pasos" en fondo verde oscuro, que casualmente
  combina bien con la paleta oscura de Khrono) y se comprimieron con
  `sips` a ~100-115KB cada una. Se muestran lado a lado (apiladas en
  mobile, breakpoint 700px) dentro de `.case-shot`, con badge tipo
  `.card-badge` superpuesto ("PROLIG.CL — CLIENTE REAL" / "Así de simple
  queda para el cliente"). Se agregó un link "Visitar prolig.cl"
  (`target="_blank"`) como prueba adicional de que es un cliente real.
- **Tipografía mucho más grande**: `.case-title` usa el mismo tratamiento
  Fraunces que `.section-title` pero con `clamp(2.2rem, 5vw, 3.4rem)`
  (más grande que el resto de headers de sección) y **texto centrado**
  (`.case-study { text-align: center; }`, único caso en el sitio — todas
  las demás secciones son left-aligned). Los 2 stats (`90%` / `2
  sueldos`) usan `.case-stat-num` con `clamp(3rem, 7vw, 4.6rem)` — mucho
  más grande que el `.stat-num` original (1.7rem) que se usaba en el
  hero — para que sean el elemento que "detiene" al visitante. En mobile
  angosto (~500px) el `flex-wrap` de `.case-stats-row` los apila uno
  debajo del otro automáticamente, sin media query adicional.
- **Menos texto**: se bajó de 2 párrafos + 2 stat-pill con descripción
  larga a **1 sola línea de subtítulo** (`.case-sub`) + 2 stats con
  label corto (una línea). El copy explicativo largo sobre qué se
  automatizó se sacrificó a propósito por impacto visual, a pedido
  explícito de Rodolfo.
- Verificado visualmente en 3 anchos distintos vía Claude en Chrome
  (servidor local `python3 -m http.server`, ya que `file://` no es
  navegable con estas herramientas): ~1280px (desktop, 2 imágenes lado a
  lado), ~606px y ~500px (imágenes apiladas, stats apilados en el más
  angosto). Sin verificar en dispositivo real.

## Webhook de leads devolvía 503 — 0 leads reales desde que arrancó la campaña (13 ago 2026)

Rodolfo reportó 320 visitas y 0 interacciones/agendamientos desde que la
campaña de Meta Ads empezó a correr. El diagnóstico inicial fue "landing
poco convincente", pero la causa real era un **bug técnico**, no de
conversión/copy:

- La Google Sheet de leads solo tenía 3 filas, **todas de antes del 10 ago**
  (pruebas de Rodolfo mismo). Cero leads nuevos desde que entró tráfico
  pagado real (confirmado por los `fbclid`/`utm_source=fb/ig` en la pestaña
  "Visitas", 130+ filas).
- Se probó el funnel completo a mano (Claude en Chrome): preguntas →
  contacto → calendario, todo funcionaba visualmente sin errores. Pero la
  llamada de red real al `WEBHOOK_URL` (Apps Script) devolvió **503** dos
  veces seguidas, en dos cargas de página distintas.
- **Causa raíz**: desde que se agregó el tracking de visitas (10 ago, ver
  "Panel de leads en tiempo real" abajo), el mismo Apps Script recibe una
  llamada `doPost` en **cada pageview**, no solo cuando alguien completa el
  cuestionario. Con tráfico de ads llegando en ráfagas, esas llamadas
  compiten por escribir en la misma Sheet — el registro de ejecuciones del
  script (script.google.com → Ejecuciones) mostró llamadas normales de
  1-2s mezcladas con picos de 7.5s y 10.2s. Cuando una ejecución se demora
  así, el proxy de Apps Script le devuelve 503 al navegador del visitante
  antes de que el script termine (aunque en el backend "eventualmente"
  complete) — y como `submitLead()` usa `fetch` fire-and-forget con
  `mode:'no-cors'` (sin leer la respuesta ni reintentar), cualquier lead
  que cayera en una de esas ráfagas se perdía en silencio: el usuario veía
  "¡Listo!" y pasaba al calendario sin que nadie se enterara de que el dato
  nunca llegó a la Sheet.
- Se descartó una pista falsa: el editor de Apps Script mostraba un banner
  de "entorno Rhino obsoleto", pero Configuración del proyecto confirmó que
  V8 sí está habilitado — el banner era un artefacto del panel de
  depuración, no reflejaba el runtime real usado por `doPost`.

**Fix aplicado** (Apps Script `Código.gs`, redesplegado como nueva versión
de la misma implementación — el `WEBHOOK_URL` no cambió):
- Todo el `doPost` envuelto en `try/catch`, siempre devuelve JSON válido en
  vez de dejar una excepción sin capturar.
- **Dedup de leads**: antes de escribir, compara nombre+whatsapp con la
  última fila; si coincide y la fecha es de hace menos de 60s, no
  duplica (devuelve `{ok:true, dedup:true}`).

**Fix aplicado** (`index.html`):
- El tracking de "visita" ahora se manda **una sola vez por sesión de
  navegador** (`sessionStorage`), no en cada pageview — reduce
  drásticamente la carga que compite con los leads por la misma cuota
  compartida de Apps Script. El Pixel de Meta ya trackea `PageView` aparte,
  así que no se pierde esa métrica.
- `submitLead()` ahora manda el POST del lead **dos veces**, separado por
  1.5s (`sendLead()` + `setTimeout`). Como `mode:'no-cors'` no permite leer
  si la llamada tuvo éxito, no hay forma de "reintentar solo si falló" —
  se manda dos veces siempre. Es un lead pagado: una fila duplicada
  ocasional (que el dedup del backend ya filtra en la mayoría de los
  casos) es un costo mucho menor que perder el dato.
- Verificado en vivo (Claude en Chrome, tráfico real desde khrono.cl) que
  la llamada al webhook ya no devuelve 503 después del fix.

**Pendiente si se retoma**: confirmar en la Google Sheet que empiezan a
llegar leads reales de la campaña activa, y revisar si el Google Calendar
de `khrono.ai@gmail.com` tiene reservas que no quedaron reflejadas en la
Sheet mientras el webhook estuvo fallando (el paso de agendar es
independiente del webhook, así que pudo haber gente que agendó igual sin
que quedara registrado como lead).

## Fotos reales del equipo en Quiénes somos (14 ago 2026)

Rodolfo pidió agregar fotos reales de él y de Felipe a la sección
"Quiénes somos" (antes cada `team-card` mostraba solo un ícono SVG
genérico). Confirmó explícitamente que quería usar las fotos aunque
tienen estética muy editorial/generada (ambientación tipo mansión de
lujo, iluminación de estudio) — se le preguntó si eran fotos reales o
generadas por IA dado que la sección dice textualmente "somos dos
personas reales, no una agencia genérica", y pidió instalarlas igual
("ponlas nomas") sin confirmar cuál de las dos opciones eran.

- **Origen de los archivos**: Rodolfo dejó los originales en
  `~/Documents/Automatizaciones/` (no en `assets/`), nombrados
  `Foto Rodolfo.PNG` (1023×1537, la foto frente al Porsche) y
  `Foto Felipe.JPG` (1086×1448, la foto del living con vista a
  Hollywood Hills) — **el mapeo nombre↔persona no es intuitivo por el
  contenido de la imagen**, hay que guiarse por el nombre de archivo
  que puso Rodolfo, no asumir por la escena.
- **Recorte**: `sips -c <h> <w> --cropOffset <offsetY> <offsetH>`
  permite recorte no centrado (a diferencia del crop centrado por
  defecto de `-c`). Se usó para centrar la cara de cada uno dentro de
  un cuadrado de 900×900 antes de reescalar. Advertencia real
  encontrada: `sips -z` conserva el **formato interno original** aunque
  el archivo de salida tenga extensión `.jpg` — el PNG de Rodolfo quedó
  como PNG disfrazado de `.jpg` (290KB en vez de ~40KB) hasta forzar
  `-s format jpeg` explícito.
- **Iteración 1 (foto circular chica, 72px)**: `.team-icon` reemplazado
  por `<img class="team-photo">` circular con anillo del color de
  marca. Rodolfo la vio en producción y pidió más espacio — a esa
  escala no se apreciaban bien.
- **Iteración 2 (banner con degradado, versión final)**: cada
  `team-card` ahora tiene `.team-photo-wrap` (foto de ancho completo,
  280px de alto, `object-fit: cover`) seguido de `.team-card-body`
  (el padding que antes tenía toda la tarjeta). El degradado
  (`.team-photo-wrap::after`, `linear-gradient` de transparente a
  `var(--card)`) funde la foto con el fondo de la tarjeta antes de que
  empiece el texto — mismo `var(--card)` que usa el gradiente de fondo
  de `.team-card`, así no se nota la costura. Imágenes regeneradas a
  640×640 (antes 480×480) porque a este tamaño de banner se notaba la
  falta de resolución.
- **Bug real encontrado y corregido**: con `object-position: center
  30%` (mismo valor para ambas fotos), la foto de Felipe quedaba
  cortada a la altura de la nariz — en su foto original la cara está
  mucho más arriba en el encuadre que en la de Rodolfo (headroom
  distinto en cada foto de origen), así que el mismo porcentaje de
  recorte no sirve para ambas. Se agregó un override específico por
  imagen (mismo patrón `[src*="..."]` que ya usan las `card-photo` de
  servicios): `.team-photo[src*="team-felipe"] { object-position:
  center 2%; }`. **Si se agregan más fotos de personas a futuro,
  revisar el encuadre de cada una por separado en vez de asumir que un
  solo `object-position` sirve para todas** — depende de dónde quedó
  la cara dentro del recorte cuadrado original.
- Verificado visualmente con servidor local (`python3 -m http.server`)
  + Claude en Chrome, con zoom sobre cada tarjeta para confirmar que no
  se cortaba la cara y que el degradado no dejaba una costura visible.
- 2 commits separados (iteración 1 y 2), ambos pusheados directo a
  `main`: `d75e521` (fotos circulares) y `644f7b3` (rediseño banner).

## Hint del widget del hero + calculadora nueva en /calculadora/ (19 ago 2026)

Rodolfo notó que mucha gente que ve la landing no se da cuenta de que el
widget "Haz tu agente gratis" del hero es interactivo. Pidió: (1) hacerlo
más notorio, y (2) una página/calculadora nueva, con link propio
compartible, que estime cuánto ganaría un negocio con Khrono.

- **Hint del widget** (`index.html`): se agregó `.widget-hint`, una franja
  naranja pegada arriba de `.widget-card` (bleed con `margin: -22px -22px
  18px`, mismo padding que la tarjeta) con el texto "↓ Prueba aquí cómo
  quedaría tu agente" y una animación sutil de vaivén horizontal
  (`hint-wiggle`, respeta `prefers-reduced-motion`). También se cambió el
  `widget-sub` de "Elige el rubro de tu negocio" a "Elige tu rubro y
  pruébalo tú mismo" para reforzar el mensaje.
- **Botón hacia la calculadora**: nueva sección `.calc-promo` insertada
  justo después de `#caso-exito` (antes de `#servicios`) — tarjeta con
  borde/glow naranja, "¿Cuánto ganaría tu negocio con Khrono?" +
  botón a `/calculadora/`.

### La calculadora (`/calculadora/index.html`, archivo nuevo)

Página standalone (no vive dentro de `index.html`), con su propio
`<head>`/nav/footer pero misma identidad de marca. **Diferencia clave de
implementación**: en vez de duplicar las fuentes embebidas en base64 de
`index.html` (~900KB), esta página carga Fraunces/Onest/Public Sans desde
Google Fonts (`fonts.googleapis.com`) — más liviano, aceptable porque es
un sitio público real (no un Artifact con CSP restrictiva).

**Iteración 1 → 2 → 3 de la interacción** (a pedido de Rodolfo, en la misma
sesión):
1. Primero: inputs numéricos con recálculo en vivo mientras se escribía,
   incluyendo un campo de "ticket promedio (CLP)" y resultados en horas /
   clientes / pesos.
2. Rodolfo pidió sacar el ticket promedio, agregar un botón "Calcular mi
   ahorro" con un estado de "Calculando..." de ~2.2s (spinner de 3 puntos,
   mismo patrón visual que `.demo-typing` del chat del hero) antes de
   mostrar el resultado, y que los resultados fueran **solo porcentajes**,
   nunca cifras fijas en CLP. Se implementaron 3 estados en la tarjeta de
   resultados (`#calcIdle` / `#calcLoading` / `#calcDone`), y cualquier
   cambio en las respuestas después de calcular vuelve la tarjeta a
   `calcIdle` (hay que presionar el botón de nuevo, a propósito, para que
   se sienta como "recalcular" y no como algo que se actualiza solo).
3. Rodolfo pidió además: (a) preguntar por **mensajes al día**, no al mes
   (b) que **todas** las preguntas fueran de alternativas (chips), sin
   ningún input numérico ni `<select>`, y (c) que el **marketing** entrara
   de verdad al cálculo (no solo como nota al pie) para mostrar el
   potencial de ganancia combinado.

**Estado final de las preguntas** (todas `.calc-chip` dentro de
`.calc-qgroup`, un solo `<button>` activo por grupo, sin inputs de
teclado): rubro (5 chips con emoji, igual que el demo del hero) → mensajes
por día (1 a 5 / 6 a 15 / 16 a 30 / +30) → tiempo de respuesta actual (<5min
/ 15-30min / 1-2h / +2h) → horas semanales dedicadas a responder a mano
(<3h / 3-7h / 8-15h / +15h) → un separador visual "TU MARKETING HOY"
(`.calc-qgroup-divider`) → cómo consiguen clientes nuevos hoy (casi no
hago marketing / publico de vez en cuando / ya invierto en campañas
pagadas). Cada rubro trae una combinación de respuestas por defecto
(`RUBROS` en el `<script>`) que se aplican como chips activos al elegirlo,
editables por el usuario.

**Fórmulas** (todas en el `<script>` al final del archivo, sin backend —
100% cálculo en el cliente):
- `perdidaPct` según tiempo de respuesta: <5min=5%, 15-30min=15%,
  1-2h=30%, +2h=45%. Con Khrono (respuesta instantánea 24/7) baja a un
  piso de 5% (pérdida irreducible, no todo es velocidad).
- **% clientes recuperados** = `(perdidaPct - 5%) / perdidaPct` — qué
  fracción de lo que hoy se pierde, se recupera.
- **% horas liberadas** = fijo por rubro (`automatizable`: restaurante
  80%, clínica 70%, inmobiliaria 65%, ferretería/tienda 75%, otro 70%) —
  no depende de las horas semanales declaradas (esas solo se usan en el
  texto de "cómo calculamos esto" para dar contexto).
- **% ingresos por automatización** = `(convertidoConKhrono -
  convertidoHoy) / convertidoHoy`, donde `convertido = 1 - perdidaPct`.
  Ojo: la primera versión de esta fórmula multiplicaba el delta de
  pérdida por una tasa de conversión por rubro y daba números chicos
  (~3%) poco convincentes — se cambió a esta razón directa (independiente
  de conversión) porque escala de forma más intuitiva con lo mal que
  respondes hoy (15-30min ≈ 12%, 1-2h ≈ 36%, +2h ≈ 73%).
- **% marketing** = fijo por nivel de marketing actual declarado:
  "casi no hago marketing" 35%, "publico de vez en cuando" 20%, "ya
  invierto en campañas pagadas" 10% — a propósito inversamente
  proporcional al esfuerzo actual (menos marketing hoy = más techo de
  crecimiento posible).
- **% total combinado** (el resultado grande y destacado,
  `.calc-result-total`) = `(1 + ingresoAutomatización) × (1 + marketing) -
  1` — compone ambos efectos en vez de sumarlos.
- El desplegable "¿Cómo calculamos esto?" (`<details class="calc-how">`)
  arma un párrafo con las 4 cifras y sus supuestos, para transparencia —
  mismo espíritu que el resto del sitio (nada de caja negra).

**Nota de marketing integrado**: la sección al pie de la calculadora
(`.calc-marketing-note`) se reescribió de "esto es solo automatización"
(cuando el marketing era solo un link) a "Automatización + marketing,
mejor juntos" ahora que el marketing es parte real del cálculo — mantiene
el botón "Ver todos los servicios →" hacia `/#servicios`.

**Verificación**: probado completo en local (`python3 -m http.server
8767`, servido desde la raíz del repo) vía Claude en Chrome — flujo de
llenar preguntas → Calcular → estado de carga → resultados con las 4
cifras → expandir "cómo calculamos esto" → click en "Ver marketing con
IA" navega correctamente a `/#servicios` en `index.html`. Verificado que
cambiar cualquier respuesta después de calcular vuelve al estado inicial.
**No verificado en mobile real** (mismo caveat de siempre — `resize_window`
de Claude en Chrome es poco confiable para esto), solo el layout responsive
por CSS (`@media max-width: 860px/560px`, chips a 2 columnas).

**Ruta y build**: `calculadora/index.html` (carpeta nueva en la raíz del
repo) — GitHub Pages sirve automáticamente `/calculadora/` apuntando a ese
`index.html`, sin configuración adicional. Todos los links internos
(nav, promo de la landing, footer) usan rutas absolutas (`/`, `/assets/...`,
`/calculadora/`, `/#servicios`) en vez de relativas, porque el sitio corre
en la raíz del dominio (`CNAME` = `khrono.cl`) tanto en local
(`python3 -m http.server` desde la raíz del repo) como en producción.

## Convenciones

- Código y comentarios en español
- Sin build ni dependencias — es un solo archivo HTML autocontenido
- Cambios se commitean y pushean directo a `main` (sin staging)
