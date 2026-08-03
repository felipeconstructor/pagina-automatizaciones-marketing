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
- Tarjetas de servicio con íconos contextuales
- Cuestionario de calificación de leads antes de poder agendar reunión
  (agregado para filtrar quién agenda)
- Mensaje central: automatizaciones **hechas a medida**, no plantillas
  genéricas (reforzado en hero, meta description y "Quiénes somos")

## Integraciones

- **Agendamiento**: los botones "Agendar reunión" apuntan a un evento de
  Google Calendar (`target="_blank"`):
  https://calendar.app.google/zKUp8LvVAKHzRGVF8
- **Meta Pixel + Conversions API**: EN PAUSA a pedido explícito de Felipe —
  no avanzar hasta que lo pida.
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

## Convenciones

- Código y comentarios en español
- Sin build ni dependencias — es un solo archivo HTML autocontenido
- Cambios se commitean y pushean directo a `main` (sin staging)
