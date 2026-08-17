# Conocimiento base — mercado de agencias de IA (KHRONO IA)

> Destilado del informe estratégico de agosto 2026. **Sin precios** — los montos siempre se calculan con la calculadora interna de Felipe, nunca se inventan ni se citan aquí.

## 1. Contexto de mercado

- El 51% de las empresas ya tiene agentes de IA en producción — ya no es piloto, es infraestructura operativa.
- El mercado de IA aplicada a marketing crece 25% anual; 71% de las empresas ya usa IA generativa en marketing.
- Los e-commerce que usan IA activamente crecen 2-3x más rápido que los que no.
- La barrera técnica bajó (cualquiera arma un chatbot en una tarde), por lo que el mercado se llenó de "agencias" superficiales. El cliente que paga bien ya distingue entre eso y una agencia que entiende su negocio. Ahí está el espacio de KHRONO.

### Cómo estructuran su oferta las agencias grandes (Infosys y similares)
1. **AI Readiness** — antes de vender IA, venden orden: datos limpios, procesos documentados. Es la puerta de entrada de confianza.
2. **Optimización de valor** — una vez el sistema está en producción, cobran por mantenerlo funcionando bien (ingreso recurrente, no proyecto que se cierra y se olvida).
3. **Servicios orientados a resultado** — el precio se ata al resultado de negocio, no a las horas ni a la tecnología. Es el modelo más avanzado, para clientes de confianza ya ganada.

**Lectura para KHRONO:** empezar con orden → sistema en producción → contrato atado a resultado. Ese es el camino de cliente chico a cliente grande.

### Cambio de modelo de precio en la industria
De cobrar por hora/asiento a cobrar por valor. Seis modelos conviven: retainer, por proyecto, por desempeño, híbrido (setup + mensualidad — el que usa KHRONO hoy), productizado, y basado en valor (el objetivo a mediano plazo con clientes de confianza ya ganada).

## 2. Cómo vender sin vender IA

**Error #1 de agencias jóvenes:** entrar hablando de tecnología. El cliente no compra "RAG sobre Supabase" — compra dejar de perder ventas o dejar de pagar horas extra por trabajo repetitivo. La tecnología es el cómo; el cliente solo necesita el qué y el cuánto (dinero/tiempo).

### El framework de las tres preguntas (obligatorio antes de cotizar)
1. **¿Dónde duele hoy, en plata o en tiempo?** ("¿Cuántas horas a la semana se van en esto?", "¿Cuántos leads se pierden por no responder rápido?")
2. **¿Cómo se ve el éxito en 90 días, en un número?** (qué número cambia si esto funciona)
3. **¿Quién decide y quién lo va a usar todos los días?** (evita construir algo perfecto que nadie usa)

### Guion de descubrimiento
- "Antes de hablar de qué herramienta usar, cuéntame cómo se hace esto hoy, paso a paso, como si me lo explicaras a alguien que no sabe nada del rubro." (dejar hablar sin interrumpir — ahí está el 80% del diagnóstico)
- "¿Qué pasa cuando esto falla o se atrasa? ¿A quién le cae el problema encima?"
- "Si mañana tuvieras una persona más dedicada 100% a esto, ¿qué harías distinto?" (revela el verdadero valor: lo que se vende es esa persona adicional, disponible 24/7, que nunca se enferma ni renuncia)

### Frases prohibidas → frases que funcionan
| No digas | Di esto |
|---|---|
| "Te instalamos un chatbot con IA" | "Ponemos a alguien respondiendo por ti en WhatsApp todo el día, que nunca se demora" |
| "Usamos RAG y embeddings sobre tu base de datos" | "El sistema conoce cada producto, precio y política tuya, y no inventa nada que no esté en tus documentos" |
| "Implementamos agentes autónomos multi-step" | "El sistema hace solo el trabajo repetitivo que hoy hace una persona a mano, y avisa antes de decisiones importantes" |
| "Tenemos el stack más avanzado del mercado" | "Elegimos la herramienta que resuelve tu problema con el menor costo de mantención posible" |
| "Te vamos a automatizar todo" | "Partimos por el proceso que más plata o tiempo te está costando hoy, y medimos el resultado antes de seguir" |

### Cómo se ancla el precio (sin decir montos aquí)
El precio nunca se presenta solo — se ancla contra el costo actual del problema (horas perdidas, ventas que se caen, reclamos). Siempre se llega a la conversación de precio con esa cuenta ya hecha y mostrada al cliente, **antes** de dar el número. El número lo entrega la calculadora interna, no se improvisa.

## 3. Los 8 servicios — estructura, venta y ruta técnica

Para cada servicio: qué incluyen los grandes players (referencia de mercado), cómo se vende en una frase, y la ruta técnica de implementación. (Precios: usar calculadora interna, nunca inventar cifras.)

### 3.1 Automatización integral de procesos
- **Mercado:** agencias tipo AGIX ofrecen automatización de flujos repetitivos con monitoreo y mejora continua incluida en la mensualidad.
- **Venta:** "Mapeamos las tres tareas que más tiempo le quitan a tu equipo cada semana y las dejamos corriendo solas, con una persona real avisada solo cuando algo necesita su firma."
- **Ruta técnica:** (1) mapear el proceso actual en diagrama simple; (2) elegir motor — n8n (self-hosted, control total, ideal si se comparte infraestructura entre proyectos) o Make.com (más rápido de armar, mejor si el cliente quiere ver el flujo visual); (3) conectar sistemas de origen/destino vía API o webhook; (4) definir puntos de control humano; (5) monitoreo con alertas y log de ejecución; (6) manual de una página "si esto se cae, revisa acá primero".

### 3.2 Copiloto empresarial
- **Mercado:** Microsoft 365 Copilot como referencia — asistente que vive en las herramientas que el equipo ya usa y responde con el conocimiento propio de la empresa.
- **Venta:** "Le damos a tu equipo un compañero de trabajo que ya se leyó todos los manuales, contratos y procedimientos de la empresa, y responde en segundos lo que hoy toma una llamada."
- **Ruta técnica:** (1) levantar corpus de conocimiento (manuales, políticas, catálogo, FAQ real del equipo); (2) construir base vectorial (misma arquitectura RAG del 3.4); (3) definir canal (Slack/Teams o panel web simple); (4) permisos por rol; (5) capacitación de 30 min + hoja de "10 preguntas que le puedes hacer"; (6) medir adopción real las primeras 4 semanas y ajustar con lo que la gente realmente pregunta.

### 3.3 Marketing con IA
- **Mercado:** contenido generado con IA rinde en promedio 3.2x ROI. Personalización por segmento y optimización de pauta en tiempo real.
- **Venta:** "No te vendemos publicidad genérica. El sistema aprende qué mensaje convierte con cada tipo de cliente tuyo y ajusta la pauta y el contenido solo, todos los días."
- **Importante:** la inversión en pauta (Meta/Google Ads) siempre se cotiza aparte de la gestión — nunca se mezcla, es presupuesto del cliente, no margen de KHRONO.
- **Ruta técnica:** (1) definir 2-3 buyer personas con datos reales; (2) motor de generación de contenido entrenado con la voz de marca (prompt de marca reutilizable en todos los canales); (3) conectar Pixel/CAPI desde el día uno; (4) automatizar A/B testing continuo; (5) dashboard de resultados con costo por lead/venta, no solo alcance; (6) reporte mensual con 3 aprendizajes accionables.

### 3.4 RAG de datos con chatbot conversacional
- **Mercado:** sistemas de conocimiento que responden solo con información real de la empresa, sin inventar. Es, en la práctica, el corazón técnico de Nova, ya productizado.
- **Venta:** "Tu cliente hace la pregunta que sea, a la hora que sea, y recibe la respuesta exacta que está en tu catálogo o tus políticas — nunca una respuesta inventada."
- **Ruta técnica:** (1) recolectar y limpiar la fuente de verdad; (2) trocear contenido en chunks y generar embeddings (ej. text-embedding-3-small); (3) guardar vectores en Supabase + pgvector (multi-tenant, respeta permisos, sin sumar proveedor aparte); (4) pipeline de consulta: pregunta → vector → búsqueda de similitud → contexto → respuesta; (5) conectar canal (WhatsApp vía Kapso, web, Instagram) con system prompt de tono y límites claros; (6) proceso de actualización de contenido ("aprende esto") para que el cliente sume información sin depender de un dev.

### 3.5 CRM a medida
- **Mercado:** no se compite construyendo un CRM genérico desde cero cada vez — se compite construyendo sobre una base propia (Supabase) que se adapta rápido a cada cliente, bajando el costo de desarrollo frente a un CRM 100% a medida tradicional.
- **Venta:** "No te adaptamos a un CRM genérico que no calza con cómo vendes — construimos el CRM alrededor de tu proceso de venta real."
- **Ruta técnica:** (1) mapear el pipeline de ventas real (etapas, quién mueve un lead, qué info es obligatoria); (2) esquema de datos en Supabase (contactos, oportunidades, actividades, multi-tenant si se reutiliza entre clientes); (3) panel simple con las vistas que el equipo realmente usa (kanban, ficha de cliente, recordatorios); (4) conectar canales de entrada de leads (WhatsApp/Kapso, formularios, campañas) para que caigan solos al CRM; (5) automatizar seguimiento (ej. "lead sin contactar 24h → alerta"); (6) capacitar al equipo comercial + dashboard de cierre semanal.

### 3.6 Landing pages
- **Mercado:** la landing como activo de conversión conectado a pauta y analítica, no como página estática. Herramientas como Unbounce optimizan tráfico automáticamente entre variantes.
- **Venta:** "No es una página bonita — es la puerta de entrada que convierte visitas en reuniones agendadas o leads calificados, medida y optimizada, no adivinada."
- **Ruta técnica:** (1) un único objetivo de conversión por página; (2) HTML/CSS a medida, liviano, sin frameworks pesados (un solo archivo, carga instantánea en mobile — mismo patrón que ya usa Khrono); (3) cuestionario de calificación de leads antes de agendar; (4) Meta Pixel + CAPI desde el día uno, probado en Events Manager antes de lanzar pauta; (5) Open Graph + velocidad verificada en dispositivo real, no solo en escritorio; (6) revisión de seguridad básica antes de lanzar pauta (HTTPS forzado, sin secretos expuestos, dominio verificado).

### 3.7 Agentes de IA (el servicio más técnico, mayor margen)
- **Mercado:** la decisión arquitectónica que más importa es agente único vs. multi-agente — el 80% de los casos reales se resuelven bien con un solo agente bien diseñado.
- **Venta:** "Construimos un empleado digital que hace un trabajo específico de principio a fin — busca información, toma una decisión dentro de reglas claras, y ejecuta una acción — sin que nadie esté mirando pantalla a pantalla."
- **Ruta técnica ("toda la estructura al 100%"):**
  1. Definir el objetivo del agente en una frase verificable con resultado medible. Si no se puede escribir así, todavía no está listo para construirse.
  2. Elegir arquitectura: agente único con herramientas (tool calling) para la mayoría de los casos; multi-agente (patrón CrewAI/LangGraph) solo cuando hay etapas realmente independientes.
  3. Elegir framework: **Claude Agent SDK** (Python/TypeScript, recomendado por defecto en KHRONO — mismo loop de agente que Claude Code, soporte nativo de **MCP** para conectar a sistemas externos como "puertos USB"); **n8n con nodos de IA** cuando el agente debe vivir junto a automatizaciones existentes sin código; **CrewAI** cuando el caso es explícitamente multi-agente con roles diferenciados.
  4. Dar herramientas reales al agente (consultar CRM, buscar catálogo, agendar, mandar WhatsApp) — vale por lo que puede *hacer*, no solo responder.
  5. Definir límites: qué decide solo y qué necesita aprobación humana (ej. puede cotizar, no puede cerrar un descuento mayor al 10% sin avisar).
  6. Probar casos límite antes de entregar (pedido fuera de catálogo, error de sistema, dos leads simultáneos).
  7. Monitoreo en producción: logs de cada decisión, costo de API por conversación, alerta si el agente se atasca o repite un error.

### 3.8 Dashboard en vivo para toma de decisiones
- **Mercado:** Metabase (open source, 90.000+ empresas) y Looker permiten dashboards en vivo con alertas por umbral y consulta en lenguaje natural sobre los datos.
- **Venta:** "Dejas de esperar el reporte de fin de mes — ves en tiempo real cómo va tu negocio hoy, y el sistema te avisa solo cuando algo se sale de lo normal."
- **Ruta técnica:** (1) definir con el cliente las 5 métricas que de verdad mueven decisiones (menos es más); (2) conectar fuentes de datos (Supabase, CRM, pauta, WhatsApp/Kapso) a una capa central; (3) montar sobre Metabase (self-hosted, económico) para pyme, o panel a medida si el cliente necesita algo muy específico; (4) alertas automáticas por umbral (ej. "si las ventas caen 20% vs semana pasada, avisa por WhatsApp al dueño"); (5) consulta en lenguaje natural cuando el volumen lo justifique; (6) capacitación de 20 min: qué mirar cada lunes en 5 minutos.

## 4. Stack tecnológico recomendado

| Capa | Herramienta | Para qué |
|---|---|---|
| Orquestación/automatización | n8n (self-hosted, compartido entre proyectos) | Workflows, integraciones, base de casi todos los servicios |
| Modelo/agentes | Claude (API + Agent SDK) | Razonamiento, generación, agentes con herramientas vía MCP |
| Base de datos/backend | Supabase (Postgres + pgvector + Auth) | CRM, RAG, multi-tenant, autenticación — una sola base para todo |
| Canal WhatsApp | Kapso | WhatsApp Business API, multi-número por cliente |
| Hosting frontend | Railway / GitHub Pages | Apps y landings, según necesiten backend o sean estáticas |
| BI/dashboards | Metabase (self-hosted) | Dashboards en vivo económicos de mantener |
| Voz (cuando aplique) | Retell AI o Vapi | Agentes de voz entrantes/salientes, se integran con n8n |
| Pauta y tracking | Meta Business Suite + Pixel/CAPI, Google Ads | Marketing con IA, medición real de conversión |

**Qué NO usar todavía:** bases vectoriales dedicadas (Pinecone) mientras Supabase+pgvector alcance — suma proveedor y costo que casi nunca se justifica al tamaño de cliente actual. Tampoco multiplicar frameworks de agentes sin criterio claro (se paga en soporte y curva de aprendizaje).

## 5. Cómo crecer la empresa (orden de prioridad de inversión)

1. **Casos de éxito documentados primero** — antes de gastar en pauta, tener 2-3 clientes con un número concreto de resultado.
2. **Tiempo del fundador en ventas, no en producción** — la primera contratación debe liberar al que sabe vender de programar todo el día.
3. **Herramientas que bajen el costo de entregar** — plantillas propias de n8n, boilerplate de RAG reutilizable, CRM propio productizado. Cada servicio "productizado" reduce el costo marginal del siguiente cliente.
4. **Marketing propio de KHRONO** — recién cuando lo anterior existe. Una agencia de IA sin su propia máquina de leads pierde credibilidad de entrada.

### Errores que matan agencias de IA jóvenes
- Cobrar por hora en vez de por resultado (castiga la eficiencia).
- Construir todo a medida desde cero en cada cliente sin reutilizar nada (mata el margen).
- No tener proceso de soporte post-entrega (el cliente se siente abandonado apenas falla algo una vez).
- Vender tecnología que el equipo interno todavía no domina en producción — probar internamente primero, nunca con el primer cliente como conejillo de indias.

## 6. Radar — lo que más se está vendiendo con IA ahora mismo
1. Agentes de voz (inbound/outbound) integrados con automatización.
2. Copilotos internos conectados al conocimiento propio de la empresa (51% de empresas ya con agentes en producción).
3. Contenido generado con IA para marketing (71% de adopción, 3.2x ROI medido).
4. Automatización de seguimiento de leads — fácil de vender porque el dolor es universal y el ROI se demuestra en semanas.
5. Servicios de "AI readiness" — ordenar datos y procesos antes de meter IA, puerta de entrada de confianza.
