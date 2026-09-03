# Consulta Fiscal — Playground

**De:** Playsuite Technologies, S.L.
**Para:** Asesor fiscal externo
**Fecha:** 3 septiembre 2026
**Asunto:** Validación de tratamiento fiscal para plataforma SaaS de gestión de parques infantiles
**Urgencia:** Alta — bloquea lanzamiento a producción

---

## 1. CONTEXTUALIZACIÓN

### 1.1 La empresa

Playsuite Technologies, S.L. desarrolla y explota una plataforma SaaS para la gestión de parques infantiles. La plataforma permite a los parques gestionar reservas, ventas, control de acceso, facturación y reportes.

### 1.2 Modelo de negocio

La plataforma opera bajo el modelo B2B SaaS con estas operaciones principales:

| Operación | Descripción | Cliente |
|---|---|---|
| **Suscripción SaaS** | Acceso mensual/anual a la plataforma por parque | Empresa propietaria del parque |
| **Módulos adicionales** | Funcionalidades extra (reservas online, reporting avanzado) | Empresa propietaria del parque |
| **Servicios profesionales** | Implantación, formación, soporte técnico | Empresa propietaria del parque |
| **Comisión por reserva** | Comisión por cada reserva gestionada online (pendiente de definir) | Parque infantil |

### 1.3 Arquitectura multi-tenant

La plataforma soporta múltiples empresas, cada una con uno o más parques. Cada empresa/parque puede:
- Tener perfil fiscal propio (razón social, NIF, domicilio fiscal)
- Emitir facturas con sus propios datos fiscales
- Configurar impuestos según su territorio

### 1.4 Territorio inicial

Primera versión comercial para **España peninsular y Baleares**. Posible expansión futura a Canarias, Ceuta, Melilla y países de la UE.

---

## 2. PREGUNTAS PARA VALIDACIÓN

### 2.1 Suscripción SaaS — Tratamiento del IVA

**Contexto:** Playground factura mensualmente a cada parque por el uso de la plataforma. El servicio es una prestación de servicios digitales B2B.

**Pregunta 1:** ¿El servicio SaaS a parques (empresas en España peninsular) está sujeto a IVA al 21% como prestación de servicios digitales B2B (art. 69.Uno.2º LIVA)?

**Pregunta 2:** ¿Se aplica inversión del sujeto pasivo (art. 84.Uno.2º LIVA) cuando el destinatario es un empresario en España? Es decir, ¿Playground debe facturar sin IVA y es el parque quien declarara el IVA?

**Pregunta 3:** ¿Qué ocurre si el parque cliente es un autónomo sin actividad empresarial acreditada? ¿Se le aplica IVA o se considera consumidor final?

**Pregunta 4:** ¿Qué ocurre si el parque es un consumidor final (persona física sin NIF de empresario)? ¿Playground debe repercutir IVA?

### 2.2 Canarias, Ceuta y Melilla

**Contexto:** Algunos parques podrían estar ubicados en Canarias, Ceuta o Melilla.

**Pregunta 5:** ¿Cómo se factura a un parque de Canarias? ¿Está exento de IVA peninsular? ¿Qué documento justificativo se necesita del cliente?

**Pregunta 6:** ¿Qué tipo impositivo de IGIC aplica al servicio SaaS si el parque es canario y Playground factura desde la peninsular? ¿O está exento?

**Pregunta 7:** ¿Es necesario constituir una filial en Canarias para operar, o se puede facturar desde la peninsular con exención?

**Pregunta 8:** ¿Cómo se factura a parques de Ceuta y Melilla? ¿Exención de IVA peninsular? ¿IPM?

### 2.3 Clientes intracomunitarios

**Contexto:** Posible expansión a parques en otros países de la UE.

**Pregunta 9:** ¿Cómo se factura a un parque en Portugal, Francia u otro país UE? ¿Exención de IVA en España? ¿El parque declara el IVA en su país?

**Pregunta 10:** ¿Es suficiente la validación VIES para acreditar la condición de sujeto pasivo del destinatario intracomunitario? ¿Se necesita documentación adicional?

**Pregunta 11:** ¿Qué mención debe figurar en la factura para operaciones intracomunitarias exentas?

### 2.4 Facturación — Series y numeración

**Pregunta 12:** ¿Es obligatoria una serie de facturación por cada empresa/parque, o puede existir una serie global bajo un mismo NIF?

**Pregunta 13:** ¿Qué formato de serie es válido? ¿Se admite formato libre o hay restricciones de la AEAT?

**Pregunta 14:** ¿Es obligatorio separar series por tipo de factura (normal, rectificativa, simplificada)?

**Pregunta 15:** ¿Puede comenzar la numeración en cualquier número o debe empezar en 1?

**Pregunta 16:** ¿Qué longitud mínima debe tener la secuencia numérica?

### 2.5 Facturas rectificativas

**Pregunta 17:** ¿Qué formato es preferible para rectificativas: nota de crédito con referencia a la factura original, o factura rectificativa con signo negativo?

**Pregunta 18:** ¿Puede una rectificativa anular completamente una factura original? ¿Cómo se contabiliza?

**Pregunta 19:** ¿Puede una empresa/parque emitir rectificativas de facturas emitidas por otra empresa/parque del mismo grupo?

**Pregunta 20:** ¿Qué plazo máximo hay para emitir una rectificativa?

### 2.6 Retenciones IRPF

**Contexto:** Playground contrata servicios de desarrolladores freelance, consultores y profesionales.

**Pregunta 21:** ¿Qué retención aplica a los profesionales autónomos contratados? ¿15% general?

**Pregunta 22:** ¿Es obligatorio retener si el profesional factura menos de 1.200€ anuales?

**Pregunta 23:** ¿Cómo se gestiona la retención si el profesional es extranjero sin residencia en España?

### 2.7 Reservas online — Rol de Playground

**Contexto:** Playground gestiona un módulo de reservas online donde los padres reservan actividades en parques. El pago se realiza a través de Stripe.

**Pregunta 24:** Si el parque factura al consumidor final por la reserva, y Playground factura la suscripción SaaS al parque, ¿Playground tiene alguna obligación fiscal adicional por la reserva?

**Pregunta 25:** Si Playground cobra una comisión al parque por cada reserva, ¿debe facturar esa comisión al parque? ¿Con qué IVA?

**Pregunta 26:** Si Playground actúa como intermediario y cobra el importe de la reserva al consumidor para luego transferirlo al parque menos comisión, ¿está sujeto a DAC7? ¿Necesita modelo 236?

**Pregunta 27:** ¿Qué obligaciones de información tiene Playground si opera como plataforma digital que facilita pagos?

### 2.8 Facturación electrónica

**Pregunta 28:** ¿Existe obligación de facturación electrónica para Playground? ¿Aplica la Llei Crea y Creca catalana?

**Pregunta 29:** ¿Qué formato de conservación de facturas electrónicas es válido? ¿PDF, XML, JSON?

**Pregunta 30:** ¿Qué campos debe contener el libro registro de IVA de facturas emitidas?

### 2.9 I+D+i

**Pregunta 31:** ¿El desarrollo de software propio de Playground (la plataforma SaaS) podría calificar como I+D+i para deducciones en el Impuesto sobre Sociedades (art. 35 LIS)?

**Pregunta 32:** ¿Qué documentación se necesita para justificar la deducción de I+D+i?

### 2.10 Impuesto sobre Sociedades

**Pregunta 33:** ¿Qué tipo impositivo aplica a Playground en el Impuesto sobre Sociedades? ¿25% general?

**Pregunta 34:** ¿Existen bonificaciones o incentivos fiscales para empresas tecnológicas en los primeros años de actividad?

---

## 3. INFORMACIÓN QUE FACILITAMOS

Para agilizar la consulta, adjuntamos:

| Documento | Descripción |
|---|---|
| `cg-013-facturacion-validacion-fiscal.md` | Análisis fiscal completo del modelo de negocio |
| `cg-013-respuestas-cuestionario-fiscal.md` | Nuestras respuestas preliminares a las 23 preguntas originales |
| Modelo de negocio (este documento) | Contexto del proyecto y operaciones económicas |

---

## 4. FORMATO DE RESPUESTA SOLICITADO

Para cada pregunta, agradeceríamos:

| Campo | Descripción |
|---|---|
| **Respuesta** | Tratamiento fiscal aplicable |
| **Fundamento normativo** | Artículo, apartado y norma de referencia |
| **Certeza** | Alta / Media / Baja |
| **Condiciones** | Si la respuesta depende de hechos no conocidos |
| **Recomendación** | Acción concreta a tomar |
| **Riesgo** | Consecuencias de no seguir la recomendación |

---

## 5. PLAZO

Dado que este gate (CG-013) bloquea el lanzamiento a producción del sistema de facturación, agradeceríamos la respuesta en un plazo de **10 días laborables**.

Si se necesita información adicional, por favor indicarlo a la mayor brevedad.

---

## 6. CONTACTO

| Campo | Valor |
|---|---|
| **Empresa** | Playsuite Technologies, S.L. |
| **Proyecto** | Playground |
| **Contacto** | [Nombre del responsable fiscal interno] |
| **Email** | [email] |
| **Teléfono** | [teléfono] |

---

*Este documento es confidencial y está destinado exclusivamente al asesor fiscal externo para la prestación de servicios de asesoría fiscal a Playsuite Technologies, S.L.*
