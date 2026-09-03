# CG-013 — Respuestas al cuestionario fiscal (Bloque 8)

**Fecha:** 3 septiembre 2026
**Contexto:** Análisis preliminar de 23 preguntas fiscales para cierre del gate CG-013.
**Advertencia:** Estas respuestas son un análisis interno de trabajo. Cada respuesta requiere confirmación por un asesor fiscal externo colegiado. Ninguna constituye asesoramiento fiscal vinculante ni sustituye consulta a la AEAT o a la Dirección General de Tributos.

---

## Convención de niveles de certeza

| Nivel | Significado |
|---|---|
| `ALTA` | Fundamento normativo claro y directo; alta probabilidad de corrección |
| `MEDIA` | Interpretación razonable pero con margen de duda; requiere confirmación profesional |
| `BAJA` | Interpretación arriesgada o con normativa ambigua; necesita consulta vinculante |

---

## 8.1 Series y numeración

### AF-01 — ¿Es obligatoria una serie por empresa/parque o puede existir una serie global?

**Respuesta recomendada:** Se recomienda serie por empresa/parque.

**Fundamento normativo:**
- Art. 6.1 RD 1619/2012: las facturas deben ser expedidas por orden correlativo de número.
- Art. 6.1.c RD 1619/2012: "un número de orden o, en su caso, **varias series** basándose en uno o varios criterios que determinen la totalidad de las operaciones realizadas".
- La normativa **no exige** serie por empresa/parque; admite series por criterios (tipo de operación, punto de venta, período, etc.).

**Análisis:**
- Si la empresa propietaria (Playsuite Technologies, S.L.) factura a todos los parques bajo su NIF, una serie global es válida.
- Si cada parque tiene NIF propio (filiales, sucursales), cada uno necesita su propia numeración.
- La serie por empresa/parque facilita la auditoría multi-tenant pero **no es obligatoria** desde el punto de vista fiscal si todas las facturas salen bajo el mismo NIF.

**Nivel de certeza:** `MEDIA`
**Requiere confirmación:** Sí, por asesor fiscal. Pregunta concreta: "¿Es fiscalmente válido usar una única serie de facturación para todas las facturas emitidas bajo un mismo NIF, o se exige serie separada por centro de actividad o establecimiento?"

**Implicación práctica:**
- Si se aprueba serie global: simplifica la implementación pero dificulta la auditoría por tenant.
- Si se aprueba serie por parque: más compleja la gestión de numeración pero mejor trazabilidad.

---

### AF-02 — ¿Qué formato de serie es válido?

**Respuesta recomendada:** Formato libre que incluya al menos un prefijo de serie y la secuencia numérica.

**Fundamento normativo:**
- Art. 6.1 RD 1619/2012: exige "número de orden" y admite "varias series".
- No existe un formato oficial impuesto por la AEAT para la serie, solo que sea **secuencial e ininterrumpido**.

**Análisis:**
- Formatos válidos: `A-2026-00001`, `FAC-00001`, `ES-MAD01-A-2026-00001`, `001/2026`.
- La AEAT solo requiere que la numeración sea correlativa dentro de cada serie.
- El prefijo de país (`ES-`) es opcional pero recomendable para identificación internacional.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero con alta probabilidad de ser correcta.

**Implicación práctica:**
- Usar formato `ES-{parkCode}-{serie}-{año}-{secuencia}` proporciona claridad sin violar ninguna norma.

---

### AF-03 — ¿Es obligatorio separar series por tipo de factura?

**Respuesta recomendada:** Sí, se recomienda separar series para normales, rectificativas y simplificadas.

**Fundamento normativo:**
- Art. 6.1.c RD 1619/2012: admite varias series basándose en "uno o varios criterios".
- Art. 15.2 RD 1619/2012: las facturas rectificativas deben consignar su propio número de orden.
- La práctica habitual y la interpretación administrativa favorecen series separadas para normales y rectificativas.

**Análisis:**
- No hay obligación estricta de series separadas, pero la **separación de series normales/rectificativas es práctica recomendada** y facilita:
  - La consulta de rectificativas por la AEAT
  - La conciliación contable
  - La prevención de errores en la numeración

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la recomendación es sólida.

**Implicación práctica:**
- Serie `A` = facturas normales
- Serie `R` = facturas rectificativas
- Serie `S` = facturas simplificadas

---

### AF-04 — ¿Puede comenzar la numeración en cualquier número?

**Respuesta recomendada:** Sí, puede comenzar en cualquier número, pero es recomendable comenzar en 1.

**Fundamento normativo:**
- Art. 6.1 RD 1619/2012: exige "número de orden" correlativo, sin especificar punto de partida.

**Análisis:**
- La normativa no exige empezar en 1.
- Si se empieza en un número alto (ej. 10000), la AEAT podría cuestionar la correlatividad si hay saltos.
- **Recomendación práctica:** empezar en 1 para mayor claridad.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero es un tema de claridad más que de obligación.

---

### AF-05 — ¿Qué longitud mínima debe tener la secuencia numérica?

**Respuesta recomendada:** Mínimo 8 dígitos (ej. 00000001).

**Fundamento normativo:**
- Art. 6.1 RD 1619/2012: no especifica longitud mínima.

**Análisis:**
- La normativa no impone longitud mínima.
- Una secuencia de 8 dígitos (00000001 a 99999999) es suficiente para años de operación y permite.ordering.
- Para series anuales, 6 dígitos (000001) pueden ser suficientes.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** No estrictamente, pero se recomienda confirmar con asesor fiscal.

---

## 8.2 Facturas rectificativas

### AF-06 — ¿Qué formato es preferible para rectificativas: nota de crédito o factura con signo negativo?

**Respuesta recomendada:** Factura rectificativa con referencia explícita a la factura original, señalando la base imponible y cuota corregidas.

**Fundamento normativo:**
- Art. 15.2 RD 1619/2012: admite dos opciones:
  - **Opción A:** Nueva factura que rectifique la original, consignando datos de la factura rectificada.
  - **Opción B:** Factura rectificativa que consigne los datos de la factura rectificada.
- Ambas opciones son válidas legalmente.

**Análisis:**
- La **Opción B** (factura rectificativa con referencia) es la más habitual en España y la más clara para auditoría.
- Debe consignar:
  - Número, serie y fecha de la factura rectificada
  - Fecha de expedición de la rectificativa
  - Datos del destinatario
  - Concepto rectificado
  - Base imponible anterior y rectificada
  - Cuota tributaria anterior y rectificada
  - Diferencia

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la Opción B es la más extendida.

**Implicación práctica:**
- Plantilla de rectificativa con campos dinámicos que referencian la factura original.

---

### AF-07 — ¿Puede una rectificativa anular completamente una factura original?

**Respuesta recomendada:** Sí, una rectificativa puede anular completamente una factura original.

**Fundamento normativo:**
- Art. 15.2.b RD 1619/2012: la rectificativa puede anular la factura original total o parcialmente.

**Análisis:**
- Para anulación total: la rectificativa consigna base imponible y cuota = 0 (o el importe contrario para cancelar).
- La factura original **no se borra** del registro; la rectificativa referencia y la factura original queda "rectificada" o "anulada".

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la interpretación es clara.

---

### AF-08 — ¿Puede una empresa/parque emitir rectificativas de facturas emitidas por otra empresa/parque?

**Respuesta recomendada:** No. Cada empresa/parque solo puede rectificar sus propias facturas.

**Fundamento normativo:**
- Art. 6.1 RD 1619/2012: la factura debe identificar al emisor con su NIF.
- Art. 15 RD 1619/2012: la rectificativa debe referenciar la factura rectificada y ser emitida por el mismo emisor.

**Análisis:**
- Si la factura fue emitida por el parque A, la rectificativa debe emitirla el parque A con su NIF.
- Si Playground emite la factura, la rectificativa debe emitirla Playground.
- **Excepción:** si existe poder notarial o autorización expresa (art. 163.4 LIVA), podría emitirse autofactura, pero es un caso muy específico.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la regla general es clara.

---

### AF-09 — ¿Qué plazo máximo hay para emitir una rectificativa?

**Respuesta recomendada:** Dentro del plazo de prescripción tributaria (4 años, prorrogable a 6).

**Fundamento normativo:**
- Art. 66.1 LGT: prescripción a los 4 años desde que se pudo ejercitar el derecho.
- Art. 30 CComercio: conservación de 6 años.

**Análisis:**
- La rectificativa puede emitirse mientras no prescriba el derecho a rectificar.
- En la práctica, la rectificativa debe emitirse **lo antes posible** tras detectar el error o la devolución.
- La AEAT podría cuestionar rectificativas emitidas fuera de plazo razonable.

**Nivel de certeza:** `MEDIA`
**Requiere confirmación:** Sí, por asesor fiscal. La prescripción de 4 años es la norma general, pero hay interrupciones y supuestos especiales.

---

## 8.3 IVA y territorio

### AF-10 — Para clientes de Canarias: ¿verificar NIF-IGIC o basta con declaración del cliente?

**Respuesta recomendada:** Verificar la condición del destinatario, no basta con la declaración.

**Fundamento normativo:**
- Art. 28 LIVA: exención para Canarias, Ceuta y Melilla.
- Art. 70 LIVA: justificación de exenciones intracomunitarias.
- Regla general: el prestador debe conservar justificante de la condición del destinatario.

**Análisis:**
- Para Canarias: el cliente debe acreditar su residencia o establecimiento en Canarias. Se recomienda:
  - Copia del NIF/CIF con dirección en Canarias
  - O declaración del destinatario de su condición de canario
- No existe un "NIF-IGIC" oficial equivalente al NIF-IVA; el NIF español es el mismo.
- La validación puede hacerse mediante declaración responsable del cliente, pero **el prestador debe conservar justificante**.

**Nivel de certeza:** `MEDIA`
**Requiere confirmación:** Sí, por asesor fiscal. Pregunta concreta: "¿Qué medio de prueba es suficiente para justificar la exención de IVA a clientes de Canarias?"

**Implicación práctica:**
- Implementar campo "Régimen territorial" en el perfil fiscal del cliente con opciones: Peninsular, Canarias, Ceuta, Melilla, Intracomunitario, Extracomunitario.
- Adjuntar documento justificativo (NIF con dirección canaria o declaración responsable).

---

### AF-11 — Para clientes intracomunitarios: ¿basta validación VIES o se necesita documentación adicional?

**Respuesta recomendada:** Validación VIES es suficiente como prueba principal, complementar con factura correctamente identificada.

**Fundamento normativo:**
- Art. 70 LIVA y art. 196 Dir. 2006/112/CE: la exención se aplica cuando el destinatario es un sujeto pasivo en otro Estado miembro.
- Art. 8 Reglamento 904/2010: la prueba de la condición de sujeto pasivo se acredita mediante el NIF-IVA validado en VIES.

**Análisis:**
- La **validación VIES** es el mecanismo oficial de verificación.
- Si VIES confirma el NIF-IVA, se presume la condición de sujeto pasivo.
- Se recomienda conservar:
  - Captura o registro de la validación VIES (fecha, resultado)
  - La factura debe consignar el NIF-IVA del destinatario y la mención "Inversión del sujeto pasivo" o "Art. 196 Dir. 2006/112/CE"
- No se necesita documentación adicional si VIES es positivo.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la validación VIES es el estándar.

**Implicación práctica:**
- Implementar validación VIES automática antes de aplicar exención intracomunitaria.
- Registrar resultado de VIES por factura (fecha de consulta y resultado).

---

### AF-12 — ¿Qué tipo impositivo aplica al desarrollo de software encargado por un cliente?

**Respuesta recomendada:** 21% IVA, sujeto y no exento.

**Fundamento normativo:**
- Art. 90.1 LIVA: tipo general 21%.
- Art. 91.Uno.14 LIVA: existe exención para "entregas, arrendamientos y explotación de programas informáticos" — pero **NO aplica al desarrollo de software a medida**; aplica a la cesión de derechos de uso de software preexistente.
- Art. 91.Uno.20 LIVA: tipo reducido 10% para "entregas de viviendas" — no aplica.

**Análisis:**
- El desarrollo de software **a medida** para un cliente es una prestación de servicios sujeta al 21%.
- La **cesión de derechos de uso de software** (licencia) podría estar sujeta al 10% o incluso exenta según interpretación.
- Para Playground: el servicio SaaS es una cesión de derechos de uso de software, pero al ser un servicio digital (acceso remoto), se aplica el 21% como prestación de servicios digitales.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero el 21% es el tipo más probable para servicios de desarrollo y SaaS.

---

### AF-13 — ¿Cómo se factura si el servicio SaaS incluye hosting y licencia de uso? ¿Se pueden prorratear?

**Respuesta recomendada:** Facturar como prestación de servicios digitales unificada al 21%; no es necesario prorratear si el servicio se presta como paquete.

**Fundamento normativo:**
- Art. 69.Uno LIVA: regla general B2B.
- Art. 78 LIVA: base imponible = contraprestación total.
- Art. 90.1 LIVA: tipo general 21%.

**Análisis:**
- Si el servicio SaaS incluye hosting + licencia + soporte, se puede facturar como **prestación única** al 21%.
- No hay obligación de prorratear si el servicio se presta como paquete integral.
- Si el cliente solicita desglose (ej. para deducir IVA soportado), se pueden mostrar líneas separadas, pero la base imponible total es la misma.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la interpretación es clara.

---

## 8.4 Retenciones

### AF-14 — ¿Qué retenciones aplican a desarrolladores freelance contratados por Playground?

**Respuesta recomendada:** 15% si son profesionales autónomos; verificar si alguno está en módulos.

**Fundamento normativo:**
- Art. 75.1.b LIRPF: retención del 15% sobre rendimientos de actividades profesionales.
- Real Decreto 439/2007: modalidades de retención (15%, 7% primeros 2 años, 1% módulos).

**Análisis:**
- **Profesional autónomo general:** 15% de retención.
- **Profesional en los dos primeros años de actividad:** 7% (si cumple requisitos).
- **Profesional en módulos:** 1% (verificar).
- **Profesional extranjero sin residencia:** retención especial (ver AF-16).
- **Si el profesional no está obligado a retener:** presentar modelo 180 sin retenciones y justificar exención.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero el 15% es la regla general.

---

### AF-15 — ¿Es obligatorio retener si el profesional factura menos de 1.200€ anuales?

**Respuesta recomendada:** No, existe un umbral de exención.

**Fundamento normativo:**
- Art. 75.1.b LIRPF: no están sujetas a retención las percepciones íntegras cuando su cuantía sea inferior a 1.200€ anuales.
- Real Decreto 439/2007, art. 9: no practica retención si la suma de las percepciones íntegras del año no supera 1.200€.

**Análisis:**
- Si un profesional factura menos de 1.200€ anuales a Playground, **no se le practica retención**.
- Si factura más de 1.200€ anuales, se retiene el 15% (o 7% o 1% según corresponda).
- El umbral se calcula por pagador y año自然.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la regla es clara.

**Implicación práctica:**
- Registrar en el sistema el total facturado por cada profesional y年度, y aplicar retención solo cuando se supere 1.200€.

---

### AF-16 — ¿Cómo se gestiona la retención si el profesional es extranjero sin residencia en España?

**Respuesta recomendada:** Verificar si tiene representante fiscal en España; si no, retención especial del 24% sobre rentas de fuente española.

**Fundamento normativo:**
- Art. 75.1.b LIRPF: retención general del 15%.
- Art. 93 LIRPF: no residentes sin establecimiento permanente → retención del 24% sobre rentas de fuente española.
- Art. 13 LIRPF: rentas obtenidas sin mediación de establecimiento permanente.

**Análisis:**
- **Profesional no residente sin EP:** retención del 24% sobre pagos de fuente española.
- **Profesional no residente con EP en España:** retención del 15% (como residente).
- **Convenio para evitar la doble imposición:** puede reducir o eliminar la retención según el país de residencia.
- Se recomienda solicitar al profesional no residente:
  - Certificado de residencia fiscal en su país
  - Si aplica convenio: certificado de residencia fiscal emitido por las autoridades de su país

**Nivel de certeza:** `MEDIA`
**Requiere confirmación:** Sí, por asesor fiscal. El tratamiento de no residentes es más complejo y depende del convenio fiscal.

---

## 8.5 Integración y registro

### AF-17 — ¿Es obligatorio el registro de facturas emitidas en libro registro de IVA? ¿Se admite formato electrónico?

**Respuesta recomendada:** Sí, es obligatorio; se admite formato electrónico.

**Fundamento normativo:**
- Art. 62 Reglamento IVA (RD 1624/1992): los sujetos pasivos de IVA deben llevar libros registro de facturas emitidas y recibidas.
- Art. 63 Reglamento IVA: los libros registro pueden llevarse por medios electrónicos.
- Art. 29.2.j LGT: obligación de llevar contabilidad por medios electrónicos cuando la AEAT lo requiera.

**Análisis:**
- **Obligación de libro registro de IVA:** sí, para todos los sujetos pasivos.
- **Formato electrónico:** admitido y recomendado. Debe permitir:
  - Búsqueda por número de factura, fecha, destinatario
  - Exportación a formato compatible con la AEAT (formato XML o TXT)
  - Inmutabilidad del registro
- **Contenido del libro registro:**
  - Número de factura
  - Fecha de expedición
  - Fecha de operación
  - NIF del destinatario
  - Nombre/razón social
  - Base imponible
  - Tipo impositivo
  - Cuota tributaria
  - Tipo de operación (sujeta y no exenta, exenta, no sujeta)

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la obligación es clara.

**Implicación práctica:**
- Implementar exportación del libro registro de IVA en formato estructurado (JSON/XML).
- La AEAT puede requerir formato específico; verificar formato actual.

---

### AF-18 — ¿Qué información debe contener el registro de facturas emitidas?

**Respuesta recomendada:** Los campos del art. 6 RD 1619/2012 más los campos del libro registro del art. 62 Reglamento IVA.

**Fundamento normativo:**
- Art. 6 RD 1619/2012: contenido mínimo de factura.
- Art. 62 Reglamento IVA: contenido del libro registro.

**Análisis:**
El libro registro de facturas emitidas debe contener, al menos:

| Campo | Descripción |
|---|---|
| Número de factura | Secuencial |
| Fecha de expedición | Fecha de la factura |
| Fecha de las operaciones | Si difiere de la de expedición |
| NIF destinatario | Identificador fiscal |
| Nombre/razón social | Del destinatario |
| Base imponible | Importe base |
| Tipo impositivo | 21%, 10%, 4% |
| Cuota tributaria | Importe de IVA |
| Cuota total | Base + cuota |
| Tipo de operación | Sujeta y no exenta / exenta / no sujeta |
| Régimen especial | Si aplica (Canarias, etc.) |
| Factura rectificada | Si es rectificativa: número y fecha |
| Retención aplicada | Si aplica |
| Medio de pago | Transferencia, tarjeta, etc. |

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la lista es estándar.

---

### AF-19 — ¿Se admite la facturación electrónica obligatoria?

**Respuesta recomendada:** Verificar si aplica la normativa de facturación electrónica en Cataluña (Llei Crea y Creca) o normativa futura estatal.

**Fundamento normativo:**
- Ley 11/2020 (Llei Crea y Creca): obligación de facturación electrónica para empresas con facturación > 800.000€ en Cataluña (verificar vigencia y umbrales).
- No existe obligación general estatal de facturación electrónica para todas las empresas (a diferencia de Italia o México).
- La AEAT puede requerir formato SII (Suministro Inmediato de Información) para grandes empresas.

**Análisis:**
- **Si Playground está en Cataluña y supera 800.000€:** verificar si la Llei Crea y Creca le aplica.
- **Si no supera el umbral:** no hay obligación, pero se puede adoptar voluntariamente.
- **Formato SII:** solo para grandes empresas (> 6M€ de facturación) y grupos de consolidación fiscal.
- **Para MVP:** no es necesario facturación electrónica obligatoria; se puede adoptar en fases posteriores.

**Nivel de certeza:** `BAJA`
**Requiere confirmación:** Sí, por asesor fiscal. La Llei Crea y Creca tiene matices y puede no ser aplicable a todas las empresas tecnológicas.

**Implicación práctica:**
- Para MVP: factura en PDF + registro estructurado.
- Para futura expansión: evaluar facturación electrónica cuando se superen umbrales.

---

### AF-20 — ¿Cómo se gestiona la conservación de facturas electrónicas? ¿Formato específico?

**Respuesta recomendada:** Conservar en formato electrónico con integridad y autenticidad garantizadas; no se exige formato específico.

**Fundamento normativo:**
- Art. 29.2.j LGT: los sujetos pasivos deben garantizar la integridad, autenticidad y disponibilidad de los datos contables y fiscales.
- Art. 63 Reglamento IVA: los libros registro pueden llevarse por medios electrónicos.
- Art. 30 CComercio: conservación de 6 años.

**Análisis:**
- **Formato:** no hay formato oficial impuesto; se admite PDF, XML, JSON, TXT.
- **Requisitos de conservación:**
  - Integridad: que no se altere el contenido
  - Autenticidad: que se pueda demostrar la autoría
  - Disponibilidad: que se pueda recuperar durante 6 años
  - Legibilidad: que sea legible durante todo el período
- **Recomendación práctica:**
  - Almacenar facturas en formato **estructurado (JSON/XML) + PDF** para legibilidad.
  - Aplicar firma digital o hash para garantizar integridad.
  - Almacenar en sistema con auditoría inmutable.
  - Respaldar con copias de seguridad.

**Nivel de certeza:** `ALTA`
**Requiere confirmación:** Sí, pero la interpretación es estándar.

---

## 8.6 Sociedades y estructura

### AF-21 — ¿Qué epígrafes del IAE corresponden a la actividad de Playground?

**Respuesta recomendada:** Epígrafes del Grupo 62 (Programación informática) y 63 (Consultoría informática).

**Fundamento normativo:**
- Real Decreto Legislativo 1175/1990: tarifa del IAE.
- Grupo 62: "Programación informática".
- Grupo 63: "Consultoría informática y actividades similares".

**Análisis:**

| Epígrafe | Descripción | Actividad Playground |
|---|---|---|
| **620** | Planificación, diseño, programación, pruebas y mantenimiento de software | Desarrollo de la plataforma SaaS |
| **621** | Actividades de programación informática por encargo | Desarrollo de módulos a medida para parques |
| **629** | Otras actividades de informática | Servicios adicionales |
| **631** | Actividades de consultoría y gestión de instalaciones informáticas | Soporte y consultoría |
| **639** | Otras actividades de consultoría informática | Servicios de formación y asesoría |

**Recomendación:**
- Alta en epígrafes **620** y **631** como principales.
- Epígrafe **621** si se ofrece desarrollo a medida.
- Verificar si existe obligación de alta en IAE (depende del volumen de operaciones).

**Nivel de certeza:** `MEDIA`
**Requiere confirmación:** Sí, por asesor fiscal. La elección de epígrafes puede tener implicaciones en el IAE y en la cuota municipal.

---

### AF-22 — ¿Es necesaria una filial o sucursal para operar en Canarias, Ceuta o Melilla?

**Respuesta recomendada:** No es obligatoria, pero puede ser fiscalmente ventajosa.

**Fundamento normativo:**
- Ley 19/1994 (Canarias): régimen fiscal especial.
- Ley 12/2000 (Ceuta y Melilla): incentivos fiscales.
- No hay obligación legal de constituir filial para operar en estos territorios.

**Análisis:**
- **Sin filial:** Playground puede facturar desde la peninsular a clientes de Canarias con exención de IVA. El cliente canario aplica IGIC en sus declaraciones.
- **Con filial en Canarias:** ventajas potenciales:
  - Beneficios fiscales del régimen especial de Canarias (bonificación 50% en cuota del Impuesto sobre Sociedades).
  - Posible bonificación en IAE.
  - Proximidad con clientes.
- **Con filial en Ceuta/Melilla:** ventajas similares pero menos relevantes para una empresa tecnológica.

**Recomendación:**
- Para MVP: **no constituir filial**. Operar desde la peninsular y facturar con exención a clientes de Canarias/Ceuta/Melilla.
- Para futuro: evaluar constitución de filial en Canarias si la facturación a clientes canarios es significativa (> 20% del total).

**Nivel de certeza:** `MEDIA`
**Requiere confirmación:** Sí, por asesor fiscal. La decisión depende del volumen de operaciones y de la estrategia fiscal.

---

### AF-23 — ¿Qué deducciones de I+D+i aplican al desarrollo de software propio?

**Respuesta recomendada:** Deducción del 25% sobre gastos de I+D+i (art. 35 LIS), con posibilidad de deducción adicional del 8% sobre inversiones.

**Fundamento normativo:**
- Art. 35 Ley 27/2014 (LIS): deducciones por I+D+i.
- Art. 35.1 LIS: deducción del 25% de los gastos de I+D+i (42% para gastos de personal).
- Art. 35.2 LIS: deducción del 8% sobre inversiones en elementos de I+D+i.
- Art. 35.4 LIS: deducción del 12% por actividades de innovación tecnológica.

**Análisis:**

| Concepto | Deducción | Requisitos |
|---|---|---|
| **Gastos de I+D+i** | 25% (gastos corrientes) / 42% (gasto personal) | Proyecto de I+D+i documentado, registro en Ministerio |
| **Inversiones en I+D+i** | 8% sobre inversiones | Elementos nuevos destinados a I+D+i |
| **Innovación tecnológica** | 12% sobre gastos | Mejora significativa de producto/proceso |

**Para Playground:**
- El desarrollo de la plataforma SaaS **podría** calificar como I+D+i si:
  - El proyecto tiene un avance tecnológico significativo
  - Hay resolución de incertidumbres científicas/tecnológicas
  - El resultado es novedoso
- Se recomienda:
  1. Documentar el proyecto de I+D+i con la estructura del Ministerio de Ciencia e Innovación
  2. Registrar el proyecto en el registro oficial de actividades de I+D+i
  3. Calcular las deducciones en la declaración del Impuesto sobre Sociedades
  4. Considerar la bonificación por I+D+i en la cuota del Impuesto sobre Sociedades

**Nivel de certeza:** `BAJA`
**Requiere confirmación:** Sí, por asesor fiscal. La calificación de I+D+i para desarrollo de software es un tema frecuente de disputa con la AEAT. Se recomienda consulta previa o apoyo de un consultor especializado.

**Implicación fiscal potencial:**
- Si se aprueba como I+D+i: reducción significativa de la cuota del Impuesto sobre Sociedades.
- Si no se aprueba: no hay deducción y se pierde la oportunidad fiscal.

---

## RESUMEN DE RESPUESTAS

| # | Pregunta | Respuesta | Certeza | Requiere confirmación externa |
|---|---|---|---|---|
| AF-01 | Serie por parque o global | Recomendada por parque; global válida si mismo NIF | `MEDIA` | Sí |
| AF-02 | Formato de serie | Libre, secuencial e ininterrumpido | `ALTA` | Sí |
| AF-03 | Separar series por tipo | Sí, recomendado (A, R, S) | `ALTA` | Sí |
| AF-04 | Punto de partida numeración | Puede empezar en cualquier número; recomendado: 1 | `ALTA` | Sí |
| AF-05 | Longitud secuencia | Mínimo 6-8 dígitos | `ALTA` | No estrictamente |
| AF-06 | Formato rectificativa | Factura rectificativa con referencia (Opción B) | `ALTA` | Sí |
| AF-07 | Anulación total con rectificativa | Sí, posible | `ALTA` | Sí |
| AF-08 | Rectificar factura ajena | No, solo las propias | `ALTA` | Sí |
| AF-09 | Plazo para rectificar | Prescripción 4 años (6 conservación) | `MEDIA` | Sí |
| AF-10 | Canarias: verificación | Verificar con NIF/dirección; no basta declaración | `MEDIA` | Sí |
| AF-11 | Intracomunitarios: VIES | VIES es suficiente como prueba principal | `ALTA` | Sí |
| AF-12 | Tipo IVA desarrollo software | 21% (servicios digitales) | `ALTA` | Sí |
| AF-13 | SaaS con hosting+licencia | Servicio único 21%, no necesario prorratear | `ALTA` | Sí |
| AF-14 | Retención profesionales | 15% general | `ALTA` | Sí |
| AF-15 | Umbral retención 1.200€ | Sí, exención bajo 1.200€ anuales | `ALTA` | Sí |
| AF-16 | Profesionales extranjeros | 24% sin EP; verificar convenios | `MEDIA` | Sí |
| AF-17 | Libro registro IVA | Obligatorio; formato electrónico admitido | `ALTA` | Sí |
| AF-18 | Contenido libro registro | Campos del art. 6 RD 1619/2012 + art. 62 Reglamento IVA | `ALTA` | Sí |
| AF-19 | Facturación electrónica | No obligatoria salvo Llei Crea y Creca (verificar) | `BAJA` | Sí |
| AF-20 | Conservación facturas | Formato estructurado + PDF, integridad, 6 años | `ALTA` | Sí |
| AF-21 | Epígrafes IAE | 620, 631 (principales) | `MEDIA` | Sí |
| AF-22 | Filial Canarias/Ceuta/Melilla | No obligatoria; evaluar ventajas fiscales | `MEDIA` | Sí |
| AF-23 | Deducciones I+D+i | 25% gastos, 8% inversiones; requerir documentación | `BAJA` | Sí |

---

## PRÓXIMOS PASOS RECOMENDADOS

1. **Enviar este cuestionario a un asesor fiscal externo** para obtener respuestas confirmadas.
2. **Programar una sesión de 2-3 horas** con el asesor fiscal para validar:
   - Series y numeración (AF-01 a AF-05)
   - Rectificativas (AF-06 a AF-09)
   - IVA territorial (AF-10, AF-11)
   - Retenciones (AF-14 a AF-16)
3. **Solicitar consulta vinculante** a la DGT si hay dudas sobre el tratamiento del SaaS como servicio digital B2B.
4. **Registrar las respuestas confirmadas** en un documento oficial que cierre el gate CG-013.

---

*Este documento es un análisis interno de trabajo. No constituye asesoramiento fiscal definitivo. Las respuestas deben ser validadas por un asesor fiscal externo colegiado antes de su implementación.*
