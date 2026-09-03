# CG-013 — Facturación: Validación Fiscal

## Identificación del gate

| Campo | Valor |
|---|---|
| **Gate** | CG-013 |
| **Título** | Facturación: validación fiscal de series, tipos y rectificativas |
| **Área** | Fiscal |
| **Obligación** | Ley 58/2003 (General Tributaria), RD 1619/2012 (facturación), CComercio Art. 30 (6 años) |
| **Severidad** | **HIGH** |
| **Accountable** | Asesor fiscal externo + Legal & Compliance Advisor |
| **Estado actual** | `ANALYSIS_COMPLETE` — pendiente validación externa y decisión PO |
| **Fecha objetivo** | Cierre antes de pasar a PRO cualquier funcionalidad de facturación oficial |
| **Bloquea** | Emisión de facturas oficiales; facturación SaaS y POS en PRO |

---

## 1. ESTADO ACTUAL

### 1.1 Lo que ya está definido

| Elemento | Estado | Fuente |
|---|---|---|
| Contenido mínimo de factura (13 campos, art. 6 RD 1619/2012) | ✅ Definido por DPO | Dictamen CG-013 existente |
| Tipos de factura (normal, simplificada, rectificativa) | ✅ Documentados | Dictamen CG-013 existente |
| Rectificativas (art. 15 RD 1619/2012) | ✅ Evaluadas | Dictamen CG-013 existente |
| Retenciones IRPF (15% profesionales / 19% arrendamientos) | ✅ Analizadas | Dictamen CG-013 existente |
| Configuración básica de impuestos por empresa/parque | ✅ Aprobado funcional | REQ-BIL-005, ADR-FUNC-014 |
| Emisión de facturas propias por empresa/parque | ✅ Aprobado funcional | REQ-BIL-006 |
| Conservación de facturas 6 años | ✅ Confirmado por PO | CG-005, retención y disposición |
| Perfil fiscal configurable por empresa/parque | ✅ Definido | BR-EPO005-049, BR-EPO005-050 |
| Bloqueo de factura oficial hasta CG-013 | ✅ Implementado | BR-EPO005-055 |
| Factura funcional no oficial | ✅ Implementada | CU-INV-01, BR-EPO005-044 |

### 1.2 Lo que falta por definir y validar

| Elemento | Estado | Responsable |
|---|---|---|
| **Series de numeración por empresa/parque** | `PENDIENTE` | Asesor fiscal |
| **Tipos de factura exactos a configurar** | `PENDIENTE` | Asesor fiscal |
| **Numeración secuencial y formato** | `PENDIENTE` | Asesor fiscal |
| **Rectificativas: cuándo proceden y formato** | `PENDIENTE` | Asesor fiscal |
| **Integración con sistema de registro de facturas** | `PENDIENTE` | Asesor fiscal + Backend |
| **Tratamiento de facturas simplificadas** | `PENDIENTE` | Asesor fiscal |
| **Retenciones aplicables a proveedores** | `PENDIENTE` | Asesor fiscal |
| **Tratamiento Canarias, Ceuta y Melilla** | `PENDIENTE` | Asesor fiscal |
| **Facturación de comisiones (si aplica marketplace)** | `PENDIENTE` | Asesor fiscal + Legal |
| **Mapeo contable de IVA soportado/repercutido** | `PENDIENTE` | Contabilidad + Asesor fiscal |

---

## 2. CONTEXTO FISCAL DE PLAYGROUND

### 2.1 Identificación del contribuyente

| Campo | Valor pendiente de confirmación |
|---|---|
| **Razón social** | Playsuite Technologies, S.L. (confirmar) |
| **NIF** | Pendiente de confirmación por PO (bloquea CG-007) |
| **Domicilio fiscal** | Pendiente de confirmación |
| **Actividad económica (IAE)** | Pendiente: epígrafes del IAE relacionados con desarrollo de software, explotación de plataformas digitales y/o servicios de consultoría informática |
| **Alta censal (modelo 036/037)** | Pendiente de verificación |
| **Régimen de tributación** | General ( tipo impositivo 25% Sociedades, art. 29 LIS) |
| **Regime de IVA** | General trimestral o mensual (según volumen de operaciones) |
| **Periodo de liquidación** | Pendiente: mensual si supera 6.010.121€ de facturación; trimestral en caso contrario (art. 72 LIVA) |
| **Sujeto pasivo de IVA** | Sí, operador habitual |

### 2.2 Operaciones sujetas a IVA

| Operación | Lugar de realización | Tipo IVA | Sujeto pasivo |
|---|---|---|---|
| Suscripción SaaS a parques (B2B) | España (art. 69.Uno.2º LIVA) | 21% | Destinatario (inversión sujeto pasivo, art. 84.Uno.2º LIVA) |
| Módulos adicionales (B2B) | España | 21% | Destinatario (inversión sujeto pasivo) |
| Servicios profesionales (implantación, formación) | España | 21% | Destinatario (inversión sujeto pasivo) |
| Soporte técnico (si no está incluido en suscripción) | España | 21% | Destinatario (inversión sujeto pasivo) |
| Comisión por reserva (si aplica) | España | 21% | Prestador (Playground) o destinatario según configuración |
| Venta al consumidor final (si Playground factura) | España | 21% | Playground (sin inversión sujeto pasivo) |

### 2.3 Operaciones exentas o con régimen especial

| Operación | Tratamiento | Norma |
|---|---|---|
| Clientes en Canarias | Exento de IVA peninsular; sujeto a IGIC (0%, 3%, 7%, 9.5%) | Art. 28 LIVA; Ley 4/2012 IGIC |
| Clientes en Ceuta y Melilla | Exento de IVA peninsular; sujeto a IPM (0%, 4%, 7%, 10%) | Art. 28 LIVA; RDL 2021/2021 IPM |
| Clientes intracomunitarios (empresarios) | Exento de IVA en España; IVA en país del destinatario | Art. 70 LIVA; art. 196 Dir. 2006/112/CE |
| Clientes extracomunitarios | Exento de IVA en España | Art. 28 LIVA |
| Exportaciones de servicios digitales a no residentes | Exento de IVA | Art. 28 LIVA |

---

## 3. ANÁLISIS FISCAL POR OPERACIÓN

### 3.1 Suscripción SaaS — Tratamiento detallado

**Hecho imponible:** Prestación de servicios digitales (acceso a plataforma SaaS).

**Lugar de realización:**
- B2B intracomunitario: país del destinatario (art. 44 Dir. 2006/112/CE)
- B2B nacional: España (art. 69.Uno.2º LIVA)
- B2C: país del prestador (art. 44 Dir. 2006/112/CE) — en MVP, solo B2B

**Devengo:**
- Regla general: cuando se preste el servicio (art. 75.Uno.4º LIVA)
- Pagos periódicos: devengo por cobros sucesivos cuando se perciban
- Anticipos: devengo parcial al cobro total o parcial (art. 75.Uno.1º LIVA)

**Base imponible:**
- Importe total percibido (art. 78 LIVA)
- Descuentos conocidos al momento de la factura: reducción de base imponible (art. 78.2 LIVA)
- Impuestos no incluidos en la base imponible (art. 78.1 LIVA)

**Contenido obligatorio de la factura (art. 6 RD 1619/2012):**

| # | Campo | Ejemplo |
|---|---|---|
| 1 | Número de orden y, en su caso, serie | F-2026-00001 |
| 2 | Fecha de expedición | 03/09/2026 |
| 3 | Fecha de las operaciones (si difiere) | 01/09/2026 |
| 4 | NIF y nombre/razón social del emisor | B-XXXXXXXX / Playsuite Technologies, S.L. |
| 5 | NIF y nombre/razón social del destinatario | B-XXXXXXXX / Parque Infantil XYZ, S.L. |
| 6 | Domicilio fiscal de ambas partes | Calle..., Madrid / Calle..., Barcelona |
| 7 | Descripción de las operaciones | Suscripción mensual plataforma SaaS septiembre 2026 |
| 8 | Tipo impositivo y cuota tributaria | 21% — Cuota: XXX € |
| 9 | Base imponible | XXX € |
| 10 | Fecha de cobro (si difiere) | 03/09/2026 |
| 11 | Medio de pago | Transferencia / Tarjeta / Stripe |
| 12 | Operación sujeta y no exenta / exenta / no sujeta | Sjeta y no exenta — Regla general |
| 13 | Si procede: inversión del sujeto pasivo | Destinatario: art. 84.Uno.2º LIVA |
| 14 | Si procede: mención "Conf. Art. 68 LIVA" o similar | Para operaciones intracomunitarias |
| 15 | Si procede: retención IRPF | 15% profesionales / 19% arrendamientos |
| 16 | Si procede: recargo de equivalencia | No aplica en servicios |
| 17 | Si procede: tipo de bonificación | No aplica en servicios digitales |

### 3.2 Factura simplificada — Cuándo procede

**Requisitos del art. 6.1 RD 1619/2012:**
- Importe total no supere 400 EUR (IVA incluido)
- Operaciones en régimen especial de agricultura, ganadería y pesca
- Operaciones realizadas por revendedores a particulares
- Entregas de bienes usados, objetos de arte, antigüedades y efectos personales
- **NO aplica a prestaciones de servicios digitales B2B de cuantía superior a 400€**

**Contenido mínimo de factura simplificada:**
- Número y serie
- Fecha de expedición
- NIF y nombre del emisor
- Descripción de las operaciones
- Base imponible
- Tipo impositivo y cuota
- Total importe
- Si procede: régimen especial aplicable
- Si procede: mención "Art. 6.1 RD 1619/2012"

**En el contexto de Playground:**
- La factura simplificada puede aplicar a ventas presenciales en POS por importe < 400€ (entradas, productos, etc.)
- **NO aplica** a facturas de suscripción SaaS ni a servicios profesionales entre empresas

### 3.3 Factura rectificativa — Cuándo procede

**Causas de rectificación (art. 15 RD 1619/2012):**
- Rectificación de cuotas tributarias (errores en IVA repercutido)
- Devoluciones totales o parciales de mercancías o servicios
- Descuentos o bonificaciones concedidos con posterioridad
- Anulación total o parcial de operaciones
- Rescisión de contratos
- Omisión o error en los datos de la factura original

**Opciones de rectificación (art. 15.2 RD 1619/2012):**
- **Opción A:** Expedir una nueva factura que rectifique la original, consignando:
  - Número, serie y fecha de la factura rectificada
  - Fecha de expedición de la factura rectificativa
  - Datos del destinatario (misma empresa o cambio)
  - Concepto rectificado
  - Base imponible anterior y rectificada
  - Cuota tributaria anterior y rectificada
  - Diferencia

- **Opción B:** Expedir factura rectificativa donde:
  - Se especifiquen datos de la factura rectificada (número, fecha)
  - Se detallen los conceptos rectificados
  - Se indique la base imponible y cuota tributaria corregidas

**En Playground se recomienda:**
- Adoptar la **Opción B** para claridad y trazabilidad
- Cada factura rectificativa referencia explícitamente a la factura original
- La rectificación genera un nuevo número de factura dentro de la misma serie o serie específica (ej. serie "R" para rectificativas)

### 3.4 Tratamiento de retenciones IRPF

**Retenciones aplicables:**

| Concepto | Tipo | Norma | Cuándo aplica |
|---|---|---|---|
| Profesionales autónomos | 15% | Art. 75.1.b LIRPF | Cuando Playground contrata servicios a un profesional autónomo |
| Arrendamientos | 19% | Art. 75.1.a LIRPF | Cuando Playground alquila un espacio (si aplica) |
| Premios y loterías | 19% | Art. 75.1.d LIRPF | No aplica |
| Beneficios de无锡积累 | 19% | Art. 75.1.c LIRPF | No aplica |

**Obligaciones de retención:**
- Si Playground contrata a un profesional con NIF español → retener 15%
- Si el profesional está en el régimen de módulos → 1% (verificar)
- Si el profesional no está obligado a retener → exención justificada
- Declaración trimestral: modelo 111 (retenciones practicadas)
- Declaración anual: modelo 190 (relación de perceptores)
- Comunicación de retenciones al perceptor en la factura

---

## 4. PROPUESTA DE SERIES DE FACTURACIÓN

### 4.1 Estructura propuesta

**La numeración debe ser secuencial, ininterrumpida y por series separadas por tipo de operación o empresa/parque.**

#### Opción A — Una serie por empresa/parque (recomendada)

```
Formato: {código_pais}-{código_parque}-{serie}-{año}-{secuencia}
Ejemplo: ES-MAD01-A-2026-00000125
         ES-BCN02-A-2026-00000003
```

| Serie | Tipo | Uso |
|---|---|---|
| `A` | Facturas normales | Suscripciones SaaS, servicios profesionales, módulos adicionales |
| `R` | Facturas rectificativas | Devoluciones, descuentos posteriores, anulaciones |
| `S` | Facturas simplificadas | Ventas presenciales < 400€ en POS |

**Ventajas:**
- Separación clara por empresa/parque (multi-tenant)
- Cumple requisito art. 6 RD 1619/2012
- Fácil conciliación contable
- Trazabilidad por tenant

**Inconvenientes:**
- Más complejidad en la numeración
- Requiere configuración por empresa/parque

#### Opción B — Una serie global por tipo

```
Formato: {serie}-{año}-{secuencia}
Ejemplo: A-2026-00000125
         R-2026-00000012
         S-2026-00001450
```

**Ventajas:**
- Más simple de implementar
- Num. 1 por tipo de factura

**Inconvenientes:**
- No distingue empresa/parque a nivel de numeración
- Más difícil de auditar multi-tenant

### 4.2 Recomendación fiscal

**Recomendaría la Opción A** por las siguientes razones:
1. Cada empresa/parque puede ser un contribuyente independiente
2. La AEAT puede auditar por empresa/parque
3. La trazabilidad es clara: cada factura identifica inequívocamente al emisor
4. Se alinea con el modelo multi-tenant del sistema

**Pendiente de confirmación por asesor fiscal:**
- ¿Es obligatoria una serie por empresa/parque o puede usarse una serie global?
- ¿Qué longitud mínima debe tener la secuencia?
- ¿Es obligatorio el prefijo de país?
- ¿Puede mezclarse facturación de diferentes empresas en la misma serie?

### 4.3 Facturas rectificativas

**Recomendación:** Serie separada `R` que referencia a la factura original.

**Formato propuesto:**
```
ES-MAD01-R-2026-00000001 (rectificativa a ES-MAD01-A-2026-00000125)
```

**Contenido obligatorio de la rectificativa:**
1. Identificación de la factura rectificada (número, serie, fecha)
2. Fecha de expedición de la rectificativa
3. Datos del destinatario
4. Concepto rectificado
5. Base imponible anterior y rectificada
6. Cuota tributaria anterior y rectificada
7. Diferencia (positiva o negativa)
8. Tipo de rectificación: " Autofactura por devolución" / "Descuento posterior" / "Anulación" / "Error"

---

## 5. REQUISITOS FUNCIONALES PARA EL SISTEMA DE FACTURACIÓN

### 5.1 Configuración obligatoria

| # | Requisito | Descripción | Fuente |
|---|---|---|---|
| **CG013-R01** | Perfil fiscal por empresa/parque | Cada empresa/parque debe configurar: razón social fiscal, NIF, domicilio fiscal, régimen de IVA, periodo de liquidación, epígrafes IAE | Art. 6 RD 1619/2012 |
| **CG013-R02** | Series de facturación | Configurar series por empresa/parque (A, R, S) con numeración secuencial ininterrumpida | Art. 6.1 RD 1619/2012 |
| **CG013-R03** | Contenido mínimo factura | Implementar los 13+ campos del art. 6 RD 1619/2012 | Art. 6 RD 1619/2012 |
| **CG013-R04** | Facturas rectificativas | Implementar rectificativas con referencia a factura original | Art. 15 RD 1619/2012 |
| **CG013-R05** | Facturas simplificadas | Implementar para ventas < 400€ con contenido art. 6.1 | Art. 6.1 RD 1619/2012 |
| **CG013-R06** | Validación VIES | Verificar condición de empresario en VIES para intracomunitarios | Art. 70 LIVA |
| **CG013-R07** | Exenciones territoriales | Configurar tratamientos para Canarias (IGIC), Ceuta/Melilla (IPM), intracomunitarios y extracomunitarios | Art. 28 LIVA |
| **CG013-R08** | Retenciones IRPF | Configurar retenciones 15%/19% para profesionales y arrendamientos | LIRPF art. 75 |
| **CG013-R09** | Inversión sujeto pasivo | Marcar ISP en facturas B2B nacionales | Art. 84.Uno.2º LIVA |
| **CG013-R10** | Imposibilidad de borrado | Prohibir borrado/alteración de facturas emitidas; corrección solo por rectificativa | Art. 29 LGT; Ley antifraude |
| **CG013-R11** | Trazabilidad | Vincular cada factura a operación → pedido → pago | Art. 29 LGT |
| **CG013-R12** | Exportación contable | Mapeo automático de IVA soportado/repercutido a cuentas del PGC | PGC |
| **CG013-R13** | Conservación | Mantener facturas y evidencia fiscal 6 años mínimo | Art. 30 CComercio; Art. 29 LGT |
| **CG013-R14** | Auditoría | Registro inmutable de creación, modificación y consulta de facturas | Art. 29 LGT |
| **CG013-R15** | No duplicados | Una operación definitivamente facturada no genera segunda factura | Art. 6 RD 1619/2012 |

### 5.2 Estados de factura

| Estado | Descripción | Transiciones |
|---|---|---|
| `FACTURA_SOLICITADA` | Solicitud recibida | → `FACTURA_EN_PROCESO` |
| `FACTURA_EN_PROCESO` | En generación | → `FACTURA_EMITIDA` / `FACTURA_RECHAZADA` |
| `FACTURA_EMITIDA` | Factura válida emitida | Estado final (no modificable) |
| `FACTURA_RECTIFICADA` | Sustituida por rectificativa | → `FACTURA_RECTIFICADA` |
| `FACTURA_ANULADA` | Anulada (rectificativa completa) | → `FACTURA_ANULADA` |
| `FACTURA_PENDIENTE_CONFIG_FISCAL` | Configuración fiscal incompleta | → `FACTURA_SOLICITADA` (cuando se complete) |
| `FACTURA_RECHAZADA` | Error en datos o validación | → `FACTURA_SOLICITADA` (si se corrige) |

### 5.3 Datos mínimos de receptor para factura funcional (antes de factura oficial)

| Campo | Obligatorio | Fuente |
|---|---|---|
| Nombre o razón social del pagador | Sí | BR-EPO005-058 |
| Referencia a la operación | Sí | BR-EPO005-058 |
| NIF del receptor | Sí (para factura oficial) | Art. 6 RD 1619/2012 |
| Domicilio fiscal del receptor | Sí (para factura oficial) | Art. 6 RD 1619/2012 |

---

## 6. INTEGRACIÓN CONTABLE

### 6.1 Mapeo de cuentas (Plan General Contable)

| Concepto | Cuenta Debe | Cuenta Haber |
|---|---|---|
| Venta de servicios SaaS | 4100 Clientes | 7050 Prestación de servicios |
| IVA repercutido | 4100 Clientes | 4770 Hacienda Pública — IVA repercutido |
| IVA soportado deducible | 6291 Otros tributos | 4100 Proveedores |
| Suscripciones anuales cobradas por anticipado | 5720 Bancos | 4800 Anticipos de clientes |
| Rectificativa por devolución | 7050 Prestación de servicios (debe) | 4100 Clientes (haber) — nota de crédito |
| Retención IRPF soportada | 4751 Hacienda Pública — retenciones | 4100 Proveedores |

### 6.2 Conciliación con proveedor de pagos

| Concepto | Documento fuente | Asiento contable |
|---|---|---|
| Cobro por Stripe | Extracto Stripe | 5720 Bancos / 4100 Clientes |
| Comisión Stripe | Extracto Stripe | 6261 Servicios bancarios / 5720 Bancos |
| Transferencia a parque | Justificante transferencia | 4100 Proveedores / 5720 Bancos |
| Reembolso a cliente | Rectificativa + comprobante Stripe | 4100 Clientes / 5720 Bancos |

---

## 7. CASOS LÍMITE FISCALES

| # | Caso | Tratamiento fiscal | Requisito |
|---|---|---|---|
| **CL-01** | Cliente intracomunitario sin verificar en VIES | Facturar con IVA 21% hasta verificar condición | Validación VIES obligatoria antes de factura exenta |
| **CL-02** | Cambio de país del cliente durante el contrato | Emitir factura rectificativa y nueva factura con régimen correcto | Reconfiguración fiscal dinámica |
| **CL-03** | Descuento concedido después de la factura | Factura rectificativa por descuento posterior | Art. 78.2 LIVA |
| **CL-04** | Impago confirmado de factura | Factura rectificativa por devolución de base imponible | Art. 80 LIVA |
| **CL-05** | Anulación total de operación | Factura rectificativa con base imponible = 0 | Art. 15 RD 1619/2012 |
| **CL-06** | Error en NIF del destinatario | Factura rectificativa | Art. 15 RD 1619/2012 |
| **CL-07** | Cobro parcial de factura | Factura por el total; rectificativa si se renuncia al cobro | Devengo por cobros parciales |
| **CL-08** | Bono/cupón con caducidad | IVA devengado en momento de consumo o caducidad | Art. 75.Dos.3º LIVA |
| **CL-09** | Periodo gratuito de suscripción | No hay hecho imponible hasta que comience el cobro | Art. 75 LIVA |
| **CL-10** | Cambio de plan con prorrateo | Factura por importe neto de la diferencia; IVA sobre base prorrateada | Art. 78 LIVA |
| **CL-11** | No-show con cobro | El importe cobrado integra la base imponible de la venta original | No es hecho imponible nuevo |
| **CL-12** | Reserva cancelada con penalización | Penalización integra base imponible de la reserva | Art. 78 LIVA |
| **CL-13** | Cliente Canarias | Factura exenta de IVA peninsular; si el cliente es empresario, exento art. 28 LIVA | Verificar régimen IGIC |
| **CL-14** | Cliente Ceuta/Melilla | Factura exenta de IVA peninsular; sujeto a IPM | Verificar régimen IPM |
| **CL-15** | Factura a consumidor final sin NIF | Factura simplificada si < 400€; factura completa con "consumidor final" si > 400€ | Art. 6 RD 1619/2012 |

---

## 8. PREGUNTAS PARA EL ASESOR FISCAL EXTERNO

### 8.1 Series y numeración

| # | Pregunta | Contexto | Impacto |
|---|---|---|---|
| **AF-01** | ¿Es obligatoria una serie de facturación por cada empresa/parque, o puede existir una serie global si la empresa propietaria factura en nombre de todos sus parques? | Modelo multi-tenant: la empresa puede facturar para todos sus parques | Determina diseño del sistema de series |
| **AF-02** | ¿Qué formato de serie es válido? ¿Se admite formato libre o hay restricciones de la AEAT? | Ejemplo: `ES-MAD01-A-2026-000001` | Determina formato de numeración |
| **AF-03** | ¿Es obligatorio separar series por tipo de factura (normal, rectificativa, simplificada)? | Rectificativas vs normales | Determina si se necesita serie "R" y "S" |
| **AF-04** | ¿Puede comenzar la numeración en cualquier número o debe empezar en 1? | Primer año de operación | Determina si hay que migrar numeración |
| **AF-05** | ¿Qué longitud mínima debe tener la secuencia numérica? | Auditoría y ordenación | Determina formato |

### 8.2 Facturas rectificativas

| # | Pregunta | Contexto | Impacto |
|---|---|---|---|
| **AF-06** | ¿Qué formato es preferible para rectificativas: Nota de crédito con referencia a la factura original, o factura rectificativa con signo negativo? | Art. 15 RD 1619/2012 | Determina plantilla de rectificativa |
| **AF-07** | ¿Puede una rectificativa anular completamente una factura original, o debe emitirse una rectificativa por el 100%? | Anulaciones totales | Determina lógica de anulación |
| **AF-08** | ¿Puede una empresa/parque emitir rectificativas de facturas emitidas por otra empresa/parque del mismo grupo? | Multi-tenant | Determina permisos de rectificación |
| **AF-09** | ¿Qué plazo máximo hay para emitir una rectificativa? | Prescripción tributaria (4 años) | Determina ventanas de corrección |

### 8.3 IVA y territorio

| # | Pregunta | Contexto | Impacto |
|---|---|---|---|
| **AF-10** | Para clientes de Canarias: ¿debe Playground verificar el NIF-IGIC del destinatario, o basta con que el cliente declare su condición? | Régimen especial Canarias | Determina validación territorial |
| **AF-11** | Para clientes intracomunitarios: ¿es suficiente la validación VIES o se necesita documentación adicional? | Art. 70 LIVA | Determina proceso de validación |
| **AF-12** | ¿Qué tipo impositivo aplica al desarrollo de software encargado por un cliente? ¿21% o algún tipo especial? | Desarrollo a medida | Determina tratamiento de servicios de desarrollo |
| **AF-13** | ¿Cómo se factura si el servicio SaaS incluye componente de hosting y componente de licencia de uso? ¿Se pueden prorratear? | Paquete de servicios | Determina desglose en factura |

### 8.4 Retenciones

| # | Pregunta | Contexto | Impacto |
|---|---|---|---|
| **AF-14** | ¿Qué retenciones aplican a los desarrolladores freelance contratados por Playground? | Relación laboral vs profesional | Determina tipo de retención (15% o 19%) |
| **AF-15** | ¿Es obligatorio retener si el profesional factura menos de 1.200€ anuales? | Umbral de exención | Determina cuándo practicar retención |
| **AF-16** | ¿Cómo se gestiona la retención si el profesional es extranjero sin residencia en España? | Profesionales no residentes | Determina tratamiento fiscal |

### 8.5 Integración y registro

| # | Pregunta | Contexto | Impacto |
|---|---|---|---|
| **AF-17** | ¿Es obligatorio el registro de facturas emitidas en un libro registro de IVA? ¿Se admite formato electrónico? | Art. 62 Reglamento IVA | Determina requisitos técnicos |
| **AF-18** | ¿Qué información debe contener el registro de facturas emitidas? | Auditoría fiscal | Determina campos de auditoría |
| **AF-19** | ¿Se admite la facturación electrónica obligatoria para sujetos pasivos de IVA? | Verificar si aplica Llei Crea y Creca (Cataluña) o normativa futura | Determina si se necesita sistema de facturación electrónica |
| **AF-20** | ¿Cómo se gestiona la conservación de facturas electrónicas? ¿Formato específico? | 6 años mínimo | Determina formato de archivo |

### 8.6 Sociedades y estructura

| # | Pregunta | Contexto | Impacto |
|---|---|---|---|
| **AF-21** | ¿Qué epígrafes del IAE corresponden a la actividad de Playground? | Alta censal | Determina obligaciones formales |
| **AF-22** | ¿Es necesaria una filial o sucursal para operar en Canarias, Ceuta o Melilla? | Régimen territorial | Determina estructura societaria |
| **AF-23** | ¿Qué deducciones de I+D+i aplican al desarrollo de software propio? | Art. 35 LIS | Determina ahorro fiscal |

---

## 9. DECISIONES PENDIENTES DEL PRODUCT OWNER

| # | Decisión | Alternativas | Recomendación | Impacto |
|---|---|---|---|---|
| **PO-01** | ¿Factura funcional o factura oficial como primera release? | (a) Solo factura funcional (no oficial) hasta validar con asesor fiscal; (b) Factura oficial con validación externa | **(a)** — más seguro, evita errores fiscales | Determina si se puede facturar en PRO |
| **PO-02** | ¿Quién factura al consumidor por reservas online? | (a) El parque factura al consumidor; (b) Playground factura al consumidor como intermediario | **(a)** — menor riesgo fiscal para Playground | Determina alcance de facturación |
| **PO-03** | ¿Facturación electrónica obligatoria? | (a) Sí desde el primer día; (b) No, solo factura electrónica voluntaria | **(b)** — evaluar según volumen y normativa vigente | Determina complejidad técnica |
| **PO-04** | ¿Conservar facturas en formato estructurado (JSON/XML) o solo PDF? | (a) JSON/XML estructurado + PDF; (b) Solo PDF | **(a)** — más trazabilidad, conciliación automática | Determina arquitectura de almacenamiento |

---

## 10. CRITERIOS DE CIERRE DEL GATE CG-013

El gate CG-013 se cierra cuando se cumplen **todos** los siguientes criterios:

| # | Criterio | Evidencia requerida | Estado |
|---|---|---|---|
| **CC-01** | Asesor fiscal valida series y tipos de factura | Dictamen firmado con formato de series, numeración y tipos | `PENDIENTE` |
| **CC-02** | Rectificativas definidas con formato y causas | Documento con plantilla de rectificativa y reglas de uso | `PENDIENTE` |
| **CC-03** | Integración fiscal evaluada | Documento con mapeo contable de IVA soportado/repercutido | `PENDIENTE` |
| **CC-04** | Retenciones definidas para proveedores y profesionales | Tabla de retenciones por tipo de proveedor | `PENDIENTE` |
| **CC-05** | Decisión documentada sobre factura funcional vs oficial | Decisión PO con alcance aprobado | `PENDIENTE` |
| **CC-06** | Validación de emisión oficial por empresa/parque | Confirmación de que cada empresa/parque puede emitir con sus datos fiscales | `PENDIENTE` |
| **CC-07** | Tratamiento Canarias/Ceuta/Melilla validado | Confirmación del asesor fiscal sobre exenciones y regímenes especiales | `PENDIENTE` |
| **CC-08** | Contenido mínimo de factura verificado | Checklist de los 13+ campos del art. 6 RD 1619/2012 | `PENDIENTE` |

---

## 11. PLAN DE ACCIÓN

| Fase | Actividad | Responsable | Plazo | Dependencias |
|---|---|---|---|---|
| **1** | Recopilar toda la información fiscal de Playground (NIF, IAE, domicilio, régimen) | PO + Asesor fiscal | 3 días | PO proporciona datos |
| **2** | Sesión con asesor fiscal para validar series, tipos y rectificativas | Asesor fiscal + Legal | 5 días | Fase 1 completada |
| **3** | Definir plantilla de factura (normal, rectificativa, simplificativa) | Asesor fiscal + Backend | 3 días | Fase 2 completada |
| **4** | Definir mapeo contable (cuentas IVA soportado/repercutido) | Contabilidad + Asesor fiscal | 3 días | Fase 2 completada |
| **5** | Definir tratamiento territorial (Canarias, Ceuta, Melilla, UE, extracomunitario) | Asesor fiscal | 3 días | Fase 2 completada |
| **6** | Definir retenciones aplicables a proveedores y profesionales | Asesor fiscal | 2 días | Fase 1 completada |
| **7** | Decisión PO sobre factura funcional vs oficial | PO + Legal + Fiscal | 2 días | Fases 2-6 completadas |
| **8** | Implementar requisitos en sistema de facturación | Backend | 10 días | Fase 7 completada |
| **9** | Pruebas de facturación (QA fiscal) | QA + Backend | 5 días | Fase 8 completada |
| **10** | Cierre formal del gate CG-013 | Asesor fiscal + Legal + PO | 1 día | Fases 1-9 completadas |

**Plazo estimado total: 20-25 días laborables**

---

## 12. FUENTES NORMATIVAS

| Norma | Referencia | Contenido | URL |
|---|---|---|---|
| Ley 37/1992, LIVA | Arts. 69, 70, 75, 78, 80, 84, 90, 28 | Lugar de realización, devengo, base imponible, ISP, exenciones | https://www.boe.es/eli/es/l/1992/12/28/37/con |
| RD 1619/2012 | Arts. 6, 15 | Contenido mínimo factura, rectificativas | https://www.boe.es/eli/es/rd/2012/11/30/1619/con |
| Ley 58/2003, LGT | Arts. 29, 66 | Obligaciones formales, prescripción | https://www.boe.es/eli/es/l/2003/12/17/58/con |
| CComercio | Art. 30 | Conservación 6 años | https://www.boe.es/eli/es/rd/1885/08/22/(1)/con |
| LIRPF | Art. 75 | Retenciones profesionales y arrendamientos | https://www.boe.es/eli/es/l/2006/12/17/35/con |
| Plan General Contable | Mapeo cuentas | Registro contable de IVA | https://www.boe.es/eli/es/rd/2007/11/30/1584/con |
| Dir. 2006/112/CE | Arts. 44, 196 | IVA intracomunitario | https://eur-lex.europa.eu/legal-content/ES/TXT/?uri=CELEX:32006L0112 |
| Ley 11/2020 (Llei Crea y Creca) | Facturación electrónica | Obligación facturación electrónica (verificar vigencia y umbrales) | https://www.boe.es/eli/es/l/2020/12/31/11/con |

---

## 13. VALIDACIÓN EXTERNA

Este documento requiere validación de:

| Rol | Alcance de validación |
|---|---|
| **Asesor fiscal** | Series, numeración, tipos de factura, rectificativas, retenciones, tratamiento territorial, integración contable |
| **Abogado** | Contratos SaaS, condiciones con consumidores, responsabilidad fiscal |
| **Contabilidad** | Mapeo contable, conciliación, asientos de IVA |
| **AEAT** | Consulta vinculante si hay dudas sobre tratamiento fiscal de servicios digitales B2B |

---

## 14. HISTORIAL DE CAMBIOS

| Fecha | Versión | Cambio | Autor |
|---|---|---|---|
| 3 sept 2026 | 1.0 | Creación inicial del documento | Asesor Fiscal (Playground) |

---

*Este documento es un trabajo preparatorio para el cierre del gate CG-013. No constituye asesoramiento fiscal definitivo. Las conclusiones deben ser validadas por un asesor fiscal externo antes de su implementación.*
