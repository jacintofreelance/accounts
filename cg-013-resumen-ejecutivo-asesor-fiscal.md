# Resumen Ejecutivo — Consulta Fiscal Playground

**Playsuite Technologies, S.L. — Septiembre 2026**

---

## Qué es Playground

Plataforma SaaS para gestión de parques infantiles. Modelo B2B: los parques pagan una suscripción mensual/anual por usar la plataforma.

## Qué necesitamos validar

Tratamiento fiscal de 4 operaciones principales antes de lanzar a producción:

| # | Operación | Pregunta clave |
|---|---|---|
| **1** | Suscripción SaaS a parques (B2B) | ¿IVA 21% con inversión del sujeto pasivo? |
| **2** | Facturación (series, numeración, rectificativas) | ¿Qué formato de factura es válido? |
| **3** | Reservas online (si Playground factura al consumidor) | ¿Está sujeto a DAC7? |
| **4** | Desarrollo software propio (I+D+i) | ¿Deducción 25% en Sociedades? |

## Operación 1 — Suscripción SaaS

**Escenario:** Playground factura 500€/mes a un parque en Madrid por uso de la plataforma.

**Nuestra interpretación:**
- Servicio digital B2B → sujeto a IVA 21%
- Destinatario es empresario → inversión del sujeto pasivo (el parque declara el IVA)
- Playground factura sin IVA, el parque autoliquida

**Preguntas para validar:**
1. ¿Es correcta esta interpretación?
2. ¿Qué pasa si el parque es autónomo sin actividad empresarial?
3. ¿Qué pasa si es consumidor final (persona física)?

## Operación 2 — Facturación

**Escenario:** Playground necesita emitir facturas válidas fiscalmente a los parques.

**Nuestra interpretación:**
- Serie por parque con formato: `ES-MAD01-A-2026-000001`
- Separar series: A (normales), R (rectificativas), S (simplificadas)
- Rectificativas con referencia a factura original
- Conservación 6 años

**Preguntas para validar:**
4. ¿Es obligatoria serie por parque o vale una serie global?
5. ¿Qué formato de serie es válido?
6. ¿Qué mención debe llevar la factura para operaciones B2B nacionales?

## Operación 3 — Reservas online

**Escenario:** Un padre reserva una actividad en un parque a través de la plataforma online. El pago se realiza por Stripe.

**Nuestra interpretación:**
- El parque presta el servicio de ocio al consumidor
- El parque factura al consumidor la reserva
- Playground factura la suscripción SaaS al parque
- Si Playground cobra comisión al parque, factura la comisión

**Preguntas para validar:**
7. Si Playground no cobra al consumidor directamente, ¿está sujeto a DAC7?
8. Si cobra comisión, ¿con qué IVA se factura?
9. ¿Qué pasa si Playground actúa como intermediario que cobra y transfiere fondos?

## Operación 4 — I+D+i

**Escenario:** Playground invierte ~300.000€ anuales en desarrollo de la plataforma.

**Nuestra interpretación:**
- Podría calificar como I+D+i (art. 35 LIS)
- Deducción del 25% sobre gastos = 75.000€ de ahorro fiscal potencial

**Preguntas para validar:**
10. ¿El desarrollo de software propio califica como I+D+i?
11. ¿Qué documentación se necesita?

## Lo que necesitamos de usted

| Entregable | Plazo |
|---|---|
| Respuesta a las 34 preguntas (documento adjunto) | 10 días laborables |
| Sesión de 2 horas para resolver dudas | Tras recibir respuesta |
| Dictamen por escrito para cerrar el gate CG-013 | Tras la sesión |

## Documentación adjunta

- Consulta completa con 34 preguntas detalladas
- Análisis fiscal del modelo de negocio
- Nuestras respuestas preliminares

---

**Contacto:** [Nombre] — [email] — [teléfono]

*Confidencial. Uso exclusivo para asesoría fiscal a Playsuite Technologies, S.L.*
