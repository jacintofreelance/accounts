# Expediente único para revisión fiscal — comportamiento del sistema y parámetros de producto

**Proyecto:** invoice-compliance  
**Versión:** 0.4 — addendum de vigencia normativa; consulta aún no enviada  
**Fecha:** 2026-11-27 (base de consulta v0.3: 2026-11-26)  
**Trazabilidad interna:** OQ-0010 permanece separada y fuera de esta consulta; aplicabilidad de perfiles en §§4 y 7  
**Baseline técnica documental:** `AEAT-VERIFACTU-CANDIDATE-2026-11-26`  
**Estado del producto:** especificación documental; no hay backend/adaptador implementado o autorizado, perfiles fiscales habilitados ni pruebas de producto acreditadas.

## 1. Propósito del expediente

Este documento reúne en un solo lugar la descripción funcional que se solicita revisar: alcance previsto, parámetros de producto, datos de contexto y comportamiento esperado del sistema ante emisión, registro SIF, remisión, incidencias y correcciones.

La revisión solicitada es sobre la **coherencia fiscal del comportamiento y de los límites del producto** para el alcance indicado. No se pide a la asesoría que diseñe el software, inspeccione código inexistente ni responda una lista de decisiones técnicas aisladas. Se agradece que identifique incompatibilidades, supuestos fiscales incorrectos, condiciones de aplicación, excepciones y qué hechos/documentos serían necesarios para habilitar cada perfil.

### Fuera del alcance de esta revisión

- No se pide aprobar cómo el adaptador traduce códigos, líneas, cabeceras, estados o `Fault` de AEAT a estados internos o públicos.
- No se fija ni se solicita aquí validar un mapping de respuesta AEAT. La matriz de resultados SIF permanece en OQ-0010 como trabajo separado y provisional.
- No se solicita validar el contrato público de API/webhooks, infraestructura, atomicidad física, permisos técnicos, entrega de mensajes ni resultados de pruebas.
- No se afirma conformidad normativa, certificación/homologación AEAT ni idoneidad para operar en producción.
- Las referencias AEAT archivadas son una captura candidata con hashes; no se ha acreditado que sea el bundle de un adaptador efectivo.

## 2. Cómo interpretar los estados de este documento

Las decisiones de producto aprobadas son instrucciones de alcance o comportamiento del producto; **no equivalen a una conclusión fiscal profesional**. Se distinguen:

| Etiqueta | Significado |
|---|---|
| **APROBADO POR PRODUCTO** | Decisión comunicada/aprobada por Product Owner para orientar el comportamiento. No acredita aceptación fiscal, técnica, legal ni implementación. |
| **DISEÑO PROPUESTO** | Diseño documentado aún sujeto a aprobación especializada/técnica; no se presenta como comportamiento existente. |
| **CANDIDATO / DESHABILITADO** | Caso o perfil a evaluar; el sistema no debe ofrecerlo como capacidad habilitada. |
| **PENDIENTE DE VALIDACIÓN** | La regla o su aplicación depende de revisión fiscal/legal, evidencia o decisión competente. |
| **NO IMPLEMENTADO / NO PROBADO** | No existe evidencia de comportamiento ejecutable o de pruebas del sistema. |

La asesoría puede concluir que el alcance descrito es adecuado, adecuado con cambios, no conforme o no concluyente. Esa conclusión profesional se limita al alcance y los hechos revisados; no constituye aprobación del sistema ni habilita por sí sola un perfil.

## 3. Resumen del sistema

El producto previsto es una API SaaS B2B para gestionar facturas de organizaciones clientes y asistir con la generación del registro SIF y su remisión posterior en modalidad VERI*FACTU, cuando el emisor, operación y perfil estén evaluados y habilitados.

El sistema distingue:

1. **Tenant integrador:** organización que usa la API.
2. **Obligado/emisor fiscal:** persona o entidad por cuya cuenta se expide la factura.
3. **Representante o tercero expedidor/remitente:** persona que puede actuar materialmente por el emisor; su autoridad no se deduce de ser tenant o usuario.
4. **Factura:** documento/operación comercial y fiscal.
5. **Registro SIF:** registro fiscal asociado a una factura; no es la factura ni su representación.
6. **Remisión lógica:** acto de negocio de presentar uno o varios registros a AEAT.
7. **Intento técnico:** comunicación concreta para una remisión; un reintento de transporte no crea por sí solo una remisión nueva.
8. **Resultado externo:** determinación que solo puede atribuirse a un registro/remisión cuando la evidencia sea suficiente y esté conciliada.

**Aprobado por Producto:** mantener separadas estas identidades y no deducir autoridad de representación a partir de la relación comercial o del tenant. El ledger interno se propone como autoridad de correlación de remisiones, registros, intentos y respuestas; esto no sustituye la evidencia fiscal externa ni acredita persistencia implementada. La elección de representación, mandato, certificado y quién está autorizado a remitir requiere validación legal/fiscal para el caso real.

## 4. Alcance inicial y parámetros generales

| Parámetro | Dirección de producto | Estado/condición |
|---|---|---|
| Territorio inicial | Territorio común español. | Aprobado como alcance de producto; cada obligado/operación requiere determinar aplicación. |
| Modalidad SIF | VERI*FACTU como modalidad MVP prevista. | No habilitada hasta cerrar perfil, requisitos y revisión aplicable. |
| NO VERI*FACTU | No se plantea como fallback automático al perder conectividad. | Aprobado por Producto; cualquier cambio requiere decisión expresa y análisis fiscal. |
| Forales, IGIC/IPSI y modalidades/territorios no analizados | Excluidos inicialmente del alcance habilitado. | La exclusión es de producto, no afirmación de que la norma los excluya. |
| Perfiles fiscales | Requieren evaluación versionada por obligado, territorio, operación, fecha efectiva y hechos/evidencia. | Actualmente los candidatos están deshabilitados. |
| Hechos desconocidos | No se convierten automáticamente en falsos, exentos, tipo cero o elegibles. | Comportamiento previsto: mantener evaluación pendiente o bloquear antes de la expedición. |
| SII y otras exclusiones | Deben evaluarse como parte de aplicabilidad antes de habilitar. | Regla y evidencia del caso real pendientes de revisión. |

Los escenarios fiscales siguientes son supuestos sintéticos aportados para analizar el producto; no son clientes, documentos ni expedientes verificados:

| Caso candidato | Hechos resumidos del supuesto | Situación actual |
|---|---|---|
| TAX-001 educativo | Sociedad docente en Madrid; inglés presencial de secundaria/materia oficial; profesorado empleado; factura al representante del alumno; **90 EUR/mes**, fecha del supuesto **15/01/2027**. | `INCOMPLETE / CANDIDATE / DISABLED`; falta resolver condición del prestador y evidencia aplicable. |
| TAX-002 sanitario | Médico autónomo colegiado expide directamente consulta de diagnóstico/tratamiento en clínica de Madrid; **80 EUR**, fecha del supuesto **20/01/2027**. | `CONFIRMED_CONDITIONALLY / CANDIDATE / DISABLED`; sujeto a condiciones y documentación reales, no incluye automáticamente estética/bienestar/otros servicios. |
| TAX-003 alquiler residencial | Persona física; vivienda residencial en Madrid; contrato de **12 meses con prórrogas**; **950 EUR/mes**; sin servicios hoteleros ni empleado a jornada completa para ordenar el alquiler; capital inmobiliario declarado; fecha del supuesto **01/02/2027**. | `CONFIRMED_CONDITIONALLY / CANDIDATE / DISABLED`; clasificación de renta, efectos temporales, alcance obligatorio y ruta voluntaria requieren revisión específica. DEC-018 indica que los gates propios de TAX-003 no bloquean por sí mismos el release core, pero no habilita ni permite anunciar este perfil. |
| TAX-004 servicio B2B UE | SaaS a sociedad alemana con sede/uso en Berlín; VIES indicado como válido; sin EP español receptor; **149 EUR/mes**, fecha del supuesto **01/03/2027**. Que el proveedor/establecimiento sea español es una hipótesis pendiente de acreditación. | `CONFIRMED_CONDITIONALLY / CANDIDATE / DISABLED`; datos sintéticos, no verifican situación del proveedor, destino ni obligaciones reales. |
| TAX-005 servicio B2B extra-UE | Desarrollo API a medida para sociedad de Nueva York, que recibe/usa allí; sin EP español receptor; **5.000 EUR**, fecha del supuesto **15/03/2027**. Que el proveedor/establecimiento sea español es una hipótesis pendiente de acreditación. | `CONFIRMED_CONDITIONALLY / CANDIDATE / DISABLED`; derecho de destino, contrato, presencia/nexus y obligaciones locales no revisados. |

Las etiquetas condicionales anteriores describen revisiones documentales previas y **no son aprobación fiscal para operar**. Los perfiles siguen deshabilitados.

Los importes, fechas y demás hechos anteriores son **datos sintéticos del supuesto**, no documentos reales comprobados. En TAX-004/005 la sede del destinatario indicada no demuestra la identidad, establecimiento o situación fiscal del proveedor. La evidencia real de emisores, representación, establecimientos y operaciones sigue pendiente. Las conclusiones anteriores deben releerse con sus limitaciones temporales antes de cualquier operación en 2027.

## 5. Comportamiento funcional previsto

### 5.1 Evaluación previa a expedir

**Aprobado por Producto:** antes de numerar/expedir, el sistema debe evaluar la identidad del emisor y la autoridad de actuación, aplicabilidad territorial y de facturación, sujeto y modalidad SIF, naturaleza de operación, perfil habilitado, datos y cálculo. La descripción del producto no sustituye el análisis profesional de estos elementos.

- Una etiqueta elegida por el integrador es una solicitud, no una determinación fiscal.
- No se selecciona una regla por industria, idioma, divisa, IP, tarjeta o domicilio aislado.
- No se convierte «exento», «no sujeto», inversión del sujeto pasivo o tipo cero en equivalentes.
- Si faltan hechos, evidencia, mandato o perfil habilitado, el flujo queda pendiente o se bloquea **antes de confirmar la expedición**; no se elige un tipo por defecto ni se cambia silenciosamente el documento solicitado.
- La asesoría debe revisar si los filtros y la secuencia reflejan adecuadamente las obligaciones y excepciones de los escenarios aplicables.

### 5.2 Factura, numeración y generación del registro

**Dirección aprobada por Producto; ADR-0001 sigue `PROPOSED`:**

- El contenido fiscal usado para expedir debe quedar versionado y vinculado al emisor/perfil aplicable.
- La numeración confirmada no se reutiliza; reservas, huecos, migración y recuperación requieren reglas revisadas.
- El alta SIF debe generarse de forma simultánea o inmediatamente anterior a confirmar la expedición; el registro no se genera por primera vez en un worker posterior.
- El objetivo de diseño es confirmar coordinadamente snapshot, serie/número, alta/huella/cadena, estado de factura, evidencia requerida y outbox de remisión. La unidad ACID común es una condición de arquitectura **no acreditada ni aprobada técnicamente**; este documento no declara que el sistema ya la implemente.
- Un error de generación impide declarar una expedición exitosa bajo la dirección de producto. No se compensa borrando una factura expedida, reutilizando su número ni suprimiendo el registro histórico.

Se solicita revisar la coherencia fiscal de esta secuencia y cualquier alternativa permitida, sin tomar la propuesta de transacción como evidencia de cumplimiento.

#### 5.2.1 Ámbito y continuidad de la cadena SIF

**Dirección aprobada por Producto, sujeta a aceptación fiscal/SIF:** no particionar por tenant, serie o establecimiento por defecto. La cadena se delimitaría según la identidad y configuración efectiva del SIF que genera registros para cada obligado; obligados distintos deben conservar ámbitos/cadenas separados. El tenant que integra el servicio no se presupone obligado tributario.

El primer registro, varias series/establecimientos de un mismo obligado, varios obligados atendidos por un sistema, migración/sustitución de SIF y recuperación tras restore requieren reglas específicas. No se inventa predecesor, no se reinicia automáticamente una cadena si falta historia y no se presenta una fórmula/hash como ya revisada. La forma física de persistir/serializar la cadena es decisión técnica fuera de este dictamen.

Se solicita que la asesoría/owner fiscal confirme el ámbito del obligado y las condiciones de continuidad, primer registro, migración y restore, e indique excepciones o evidencia que falte.

### 5.3 Remisión y contingencias

**Dirección de producto:** la remisión es posterior a la generación/expedición local y puede ser asíncrona. La entrega del registro a AEAT no es la factura ni su expedición.

- La cola/outbox debe referirse al registro ya generado; reintentar transporte reutiliza el mismo registro y no genera otra factura/alta.
- Un timeout o falta de respuesta no demuestra que AEAT no recibiera la remisión ni equivale a rechazo.
- El resultado local incierto se conserva como incertidumbre pendiente de conciliación; no se publica como aceptación/rechazo ni se transforma en ausencia histórica.
- La remisión fallida no deshace una expedición local ya confirmada.
- Cualquier flujo de contingencia debe respetar el alcance VERI*FACTU previsto; no hay fallback automático a NO VERI*FACTU.

La spec SIF DRAFT incluye los siguientes **parámetros de producto/operación propuestos**, todavía sujetos a validación de vigencia, aplicabilidad y contrato técnico:

- no retrasar el primer envío disponible por el intervalo entre envíos;
- aplicar inicialmente **60 segundos entre envíos** y ajustar el intervalo al parámetro de respuesta aplicable;
- programar el siguiente envío cuando venza el intervalo o se alcance el máximo de registros previsto, lo que ocurra primero;
- ante incidencia, remitir en cuanto sea posible respetando el orden cronológico de generación;
- reintentar periódicamente los registros pendientes afectados, con frecuencia prevista de **al menos una vez por hora**;
- mientras persista la incidencia, comunicarla por el mecanismo reglamentario aplicable y mostrar el número de registros pendientes.

Estos valores se transcriben desde SIF-R04/SIF-S15 del OpenSpec DRAFT; este expediente no los vuelve a verificar como obligaciones vigentes. Se solicita confirmación del asesor contra las fuentes/versiones aplicables y que identifique cualquier condición, excepción o parámetro que dependa de la modalidad/entorno. No se solicita aquí aprobar mappings de respuestas AEAT.

### 5.4 Anulación, rectificación y subsanación

- Una anulación no es una cancelación genérica de una venta ni elimina el registro original; solo se inicia cuando existe una causa y procedimiento habilitados.
- Una rectificativa/corrección es una nueva actuación vinculada a su antecedente conforme al caso; no se reescribe silenciosamente la factura/registro histórico.
- Reintentar el transporte no es una rectificación ni una subsanación.
- No se presupone que todo resultado con errores obligue a subsanar; la obligación depende de regla, modalidad, error y operación aplicables.
- La asesoría debe revisar qué operaciones de alta/anulación/rectificación/subsanación son admisibles para el alcance, qué hechos y documentos las justifican y qué conservación exige cada una.

Los códigos y valores técnicos AEAT asociados a estas actuaciones no forman parte de la aprobación solicitada en este expediente.

### 5.5 Representación de factura, QR y declaración responsable por versión

La spec SIF DRAFT describe el siguiente comportamiento candidato para los documentos, sujeto a revisión de aplicabilidad y versión:

- La representación en papel o imagen digital que esté sujeta al requisito incluiría un QR reglamentario, con tamaño propuesto de **30×30 a 40×40 mm**, conforme a ISO/IEC 18004, nivel de corrección M y requisitos aplicables de legibilidad/resolución.
- Una factura electrónica estructurada usaría la URL reglamentaria en el campo previsto en lugar de exigir una imagen QR.
- El QR/URL utilizaría los datos prescritos por la versión aplicable (incluidos NIF, serie/número, fecha e importe total, URL/codificación cuando corresponda).
- Para la modalidad VERI*FACTU se incluiría la mención reglamentaria aplicable («VERI*FACTU» o «Factura verificable en la sede electrónica de la AEAT»); la spec no condiciona la mención a haber recibido aceptación de la remisión. La mención no se extiende automáticamente a otras modalidades.
- La spec también propone que, antes de distribuir cada versión SIF aplicable, el fabricante prepare una declaración responsable que identifique producto/versión/alcance, contenga los elementos exigibles y esté accesible desde el sistema y a disposición de los destinatarios previstos; no se presentaría como homologación/certificación general AEAT.

**Estado:** son parámetros y comportamientos recogidos en SIF-R05/R07 del OpenSpec DRAFT, no una validación actual del QR, sus dimensiones/codificación, de cada modalidad ni de la declaración responsable. QR no está incluido en la captura AEAT candidata SIF-E09. Se solicita al asesor confirmar aplicabilidad y fuentes/versiones vigentes, y especificar qué artefactos oficiales faltan antes de habilitar cada representación. La aprobación de QR no acredita la aceptación de una remisión.

### 5.6 Resultado externo y comunicación al integrador

**Dirección de producto aprobada:** un eventual evento `FiscalSubmissionOutcomeRecorded` representaría una determinación externa concluyente, conciliada para un registro en una remisión lógica concreta y registrada localmente; no la recepción de transporte, el estado local de emisión ni una mera observación de consulta.

- Una retransmisión de la misma presentación no crea por sí sola otra remisión.
- Un resultado ambiguo, incompleto o contradictorio no se presenta como conclusión.
- Una determinación externa distinta no sobrescribe el hecho histórico; su identidad y relación pública requieren contrato separado.
- El candidato de evento y payload aún no es contrato aprobado ni está publicado.

**No se define en este expediente** qué código/estado de AEAT produce qué estado interno, ni la precedencia entre estados AEAT. El catálogo `ACCEPTED`, `ACCEPTED_WITH_ERRORS`, `REJECTED`, su semántica pública y cualquier mapping están pendientes de revisión separada Tax/SIF/Product/API. No se pide al asesor que apruebe esas traducciones.

## 6. Información y evidencia que el producto prevé manejar

El análisis previo al perfil puede requerir, según el caso:

- identidad y condición fiscal del emisor, territorio/establecimiento y régimen;
- identidad y condición del destinatario cuando sea pertinente;
- naturaleza y fecha de operación, prestación/puesta a disposición, anticipos y periodo;
- clase de factura, serie/número, líneas, bases, cuotas, menciones y causa legal;
- hechos que sustenten exención/no sujeción u otro tratamiento;
- mandato/representación cuando expida o remita un tercero;
- estado de SII/exclusiones y perfil/versionado aplicables;
- referencias a documentación de soporte, procedencia y estado de verificación.

La API no debe tratar una afirmación del integrador como documento verificado. La política de datos, retención, acceso y minimización sigue sujeta a Seguridad, DPO y Legal; este listado no autoriza recopilar todos los datos para todos los perfiles.

## 7. Temas fiscales agrupados para dictamen

Se solicita un dictamen consolidado con excepciones materiales por perfil. Para cada tema, citar fuente y versión aplicables, distinguir obligación legal de política de producto y señalar los hechos/documentos reales que faltarían para trasladar una conclusión del supuesto sintético a una operación.

| # | Pregunta agrupada | Evidencia/resultado solicitado |
|---|---|---|
| 1 | **Alcance, aplicabilidad y representación (§§3, 4, 5.1, 6):** Para la API SaaS prevista, inicialmente limitada al territorio común y modalidad VERI*FACTU, ¿qué sujetos, operaciones, exclusiones —incluido SII— y fechas de aplicación deben comprobarse por obligado? Si tenant, emisor y tercero expedidor/remitente son distintos, ¿qué facultades deben acreditarse antes de expedir o remitir y qué requiere revisión jurídica adicional? | Matriz breve de aplicabilidad/calendario con fuentes vigentes; requisitos de mandato y documentación del obligado, separando la cuestión jurídica de la fiscal. |
| 2 | **Perfiles candidatos (§4):** Para los cinco supuestos sintéticos fechados en 2027 —docencia de inglés, consulta médica, alquiler residencial, SaaS B2B Alemania y desarrollo API B2B Nueva York—, ¿qué tratamiento, obligación de facturación y aplicabilidad SIF puede sostenerse, bajo qué condiciones y qué permanece indeterminado? Considerar en particular condición del prestador educativo, finalidad sanitaria, alcance obligatorio/voluntario del arrendador y establecimiento del proveedor aún no acreditado en TAX-004/005. | Anexo por perfil con conclusión condicionada, fundamento, hechos relevantes y documentos reales necesarios; identificar casos fuera del alcance o no concluyentes. No se pide habilitar perfiles. |
| 3 | **Expedición, numeración y cadena SIF (§§5.2, 5.2.1):** ¿Es coherente generar el alta simultáneamente o inmediatamente antes de confirmar la expedición y no confirmarla si falla su generación? ¿Qué reglas fiscales/documentales aplican a numeración, primer registro, ámbito por obligado/SIF, varias series/establecimientos, cambio de sistema y recuperación, sin reutilizar números ni inventar antecedentes? | Reglas y excepciones con fuentes oficiales, y evidencia que debe conservarse para huecos, continuidad, migración y recuperación. No se solicita aprobar una transacción ACID ni persistencia física. |
| 4 | **Remisión y contingencia operativa (§5.3):** Para la modalidad prevista, ¿qué condiciones permiten separar expedición y remisión y operar durante una incidencia, sin fallback automático a NO VERI*FACTU? ¿Son aplicables los parámetros candidatos —primer envío sin demora por intervalo, 60 segundos iniciales, ajuste según respuesta, envío por intervalo o máximo, orden cronológico, reintento al menos horario y comunicación de incidencia/pendientes— al alcance y versión revisados? | Tabla de parámetros obligatorios, condicionados o recomendados, con fuente/versión, límites y procedimiento de comunicación/recuperación. No revisar mappings ni estados de respuesta AEAT. |
| 5 | **Factura, QR y URL (§5.5):** ¿En qué representaciones y operaciones resultan exigibles QR, URL y mención VERI*FACTU? ¿Son correctos los candidatos de dimensiones 30–40 mm, nivel M, datos/codificación, URL en factura estructurada y mención sin condicionarla a la aceptación de la remisión? | Fuentes y especificaciones oficiales vigentes por representación, campos/menciones exigibles y criterios verificables; identificar artefactos oficiales faltantes. No hay QR implementado que aprobar. |
| 6 | **Declaración responsable (§5.5):** ¿Quién debe asumir las obligaciones de fabricante/productor para el producto y sus componentes, qué declaración exige cada versión aplicable, qué cambios requieren actualizarla y cómo debe ponerse a disposición, sin presentarla como homologación AEAT? ¿Qué documentación debe aportarse antes de asignar esa responsabilidad a una entidad concreta? | Fuente oficial y contenido mínimo, criterio de versionado/disponibilidad y documentación pendiente sobre producto, componentes y responsable. |
| 7 | **Correcciones y conservación (§§5.4, 6):** ¿Qué hechos distinguen rectificación de factura, anulación de registro y subsanación, qué actuación/documentos habilita cada caso y qué historial debe permanecer inalterado? ¿Qué categorías de documentos, registros y evidencias se conservan, durante cuánto tiempo y desde qué hito para el alcance revisado? | Matriz breve causa → actuación → evidencia/documento → conservación, con fundamento, excepciones y condiciones de ampliación del plazo. DPO/Legal concretarán después privacidad, acceso y tratamiento. |

No se requiere contestar cada campo de software ni cada variante combinatoria. Se solicita identificar reglas generales, excepciones materiales y perfiles que no puedan concluirse. Una conclusión sobre el supuesto sintético no confirma elegibilidad de una operación real ni habilita un perfil.

## 8. Evidencia de respuesta solicitada

Para que la conclusión pueda aplicarse sin ambigüedad, incluir:

- persona/entidad revisora, cualificación y ámbito de competencia;
- fecha y alcance (territorio, modalidad, operaciones y versiones documentales);
- conclusión global **sobre el alcance fiscal consultado**: adecuada, adecuada con cambios, no conforme o no concluyente (no es aprobación del sistema);
- cambios obligatorios y su ubicación en el comportamiento descrito;
- perfiles/operaciones cubiertos y expresamente excluidos;
- fuentes oficiales/profesionales, versión y referencia precisa;
- condiciones, hechos reales o documentos que el cliente debe aportar antes de habilitar cada perfil;
- si la conclusión es asesoramiento profesional, interpretación condicionada a hechos o recomendación de producto;
- límites de la opinión y fecha/condición para revalidarla.

La asesoría puede devolver una conclusión global con un anexo breve de excepciones. No se solicita certificar el software ni validar pruebas que no se han ejecutado.

## 9. Decisiones aprobadas por Producto y límites de aprobación

Las siguientes direcciones son decisiones de Producto registradas en DEC-042/decisiones posteriores y ADR-0001; se exponen para revisión, no se presentan como aprobación fiscal:

- separar tenant, obligado/emisor, sistema SIF, instalación cuando aplique, serie y sistema externo;
- no asumir identidad o facultad de representación a partir del tenant;
- evaluar aplicabilidad y perfil antes de expedir; no usar valores fiscales por defecto cuando falta información;
- mantener la generación del registro y su remisión como procesos distintos, con remisión posterior y reintento de transporte sin regenerar el alta;
- mantener la historia de factura/registro y no aplicar «último resultado gana» ni sustitución automática ante evidencia posterior;
- no emitir una conclusión pública ante incertidumbre, correlación insuficiente o contradicción;
- considerar un evento de expedición y otro de resultado externo para visibilidad pública futura, manteniendo generación SIF y evidencia Audit internas por defecto.

**Pendiente/no aprobado:** ADR-0001 permanece `PROPOSED`; no hay decisión técnica de implementación, API/webhook final, esquema de persistencia, perfil habilitado, paquete efectivo AEAT, mapping de resultados, especificación QR validada, pruebas ejecutadas ni autorización de desarrollo.

## 10. Referencias internas de trazabilidad

Estas rutas permiten mantener trazabilidad en el expediente del proyecto; la asesoría no necesita consultar múltiples documentos para comprender el comportamiento descrito:

- `00-governance/open-questions.md` — OQ-0010, SIF-E07…E10 y estado `OPEN`.
- `00-governance/decision-requests/sif-tax-qa-decision-request-001.md` — investigación técnica de respuestas AEAT separada del presente encargo fiscal.
- `00-governance/references/aeat/verifactu/2026-11-26-candidate/README.md` — manifiesto, URLs, versiones visibles, hashes y límites de validación.
- `06-architecture/adrs/adr-0001-issue-invoice-and-sif-record.md` — dirección de producto y límites de diseño; ADR `PROPOSED`.
- `13-tax-einvoicing/p0-tax-profile-matrix.md` — supuestos TAX-001…005, revisión documental y límites; matriz DRAFT, perfiles deshabilitados.
- `15-openspec/specs/sif-compliance/spec.md` — requisitos SIF candidatos; OpenSpec DRAFT, no implementación ni aprobación fiscal.

**Resultado de esta solicitud:** pendiente. El dictamen profesional, si se emite, debe identificar expresamente el alcance fiscal revisado. No convierte en aprobados los mappings/estados de respuesta AEAT, el contrato público, la ADR ni la implementación.

## 11. Revisión documental interna del borrador

Una revisión fiscal documental asistida por IA indicó que el expediente no estaba listo como documento único hasta incorporar: parámetros operativos de contingencia de SIF-R04/S15; ámbito/continuidad de cadena; importes, fechas y condición sintética de TAX-001…005, incluida la hipótesis de proveedor español en TAX-004/005 y el alcance de DEC-018 para TAX-003; y comportamiento candidato QR/URL y declaración responsable por versión de SIF-R05/R07. Estos puntos se incorporaron en §§4, 5.2.1, 5.3, 5.5 y 7. La revisión es editorial/documental, no validación normativa independiente ni dictamen profesional. El expediente requiere revisión de una asesoría antes de tratarse como aprobado.

## 12. Nota de remisión a asesoría — texto preparado, no enviado

**Asunto:** Revisión fiscal del comportamiento y parámetros propuestos para el sistema de facturación/SIF

Solicitamos una revisión profesional consolidada del comportamiento descrito en este expediente para el alcance inicial indicado: territorio común español y modalidad VERI*FACTU prevista, sin habilitar todavía perfiles fiscales. OQ-0010 se mantiene como referencia interna separada y está explícitamente fuera de esta consulta.

El encargo es revisar coherencia fiscal, aplicabilidad, excepciones, hechos que faltan y evidencia necesaria para cada perfil. Por favor, responda a las siete preguntas agrupadas de §7 y use el formato de respuesta de §8. Para TAX-001…005, indique por separado si el caso sintético queda dentro de su alcance, qué conclusión permite, qué hechos/documentos reales serían necesarios y qué condición impide concluir, si aplica. Para cada parámetro normativo u operativo, cite fuente oficial/profesional, versión/fecha y ámbito de aplicación.

**No se solicita** diseñar el software, aprobar la arquitectura transaccional, el contrato API/webhooks, permisos técnicos o pruebas; tampoco se solicita revisar o aprobar mappings, códigos, estados ni precedencia de respuestas AEAT, que permanecen fuera de este expediente en OQ-0010. Una opinión fiscal no aprobará por sí sola ADR-0001, habilitará perfiles, declarará conformidad del producto ni autorizará implementación.

El expediente contiene supuestos sintéticos, no datos de clientes ni evidencia de operaciones reales. Si la conclusión requiere hechos o documentos adicionales, identifíquelos por categoría y señale a qué perfil o tema afectan. No es necesario responder combinaciones exhaustivas: se busca una conclusión global con excepciones materiales y límites claramente delimitados.

**Material incluido:** este expediente único, versión 0.3. Sus referencias internas identifican fuentes y antecedentes; las capturas técnicas citadas son candidatas y no se presentan como bundle efectivo de un adaptador. El paquete no ha sido enviado y no contiene dictamen externo.

## 13. Addendum de contraste oficial — 2026-11-27

**Estado:** actualización documental de fuentes oficiales posterior a v0.3; no enviada a asesoría. No es dictamen profesional, aceptación especializada, aprobación adicional de Producto ni evidencia de conformidad. Este addendum prevalece sobre las referencias temporales y parámetros candidatos de §§4, 5.3 y 5.5 cuando exista contradicción, sin reescribir el registro histórico de la consulta preparada.

### 13.1 Calendario y ámbito temporal

- La DF cuarta del RD 1007/2023 fue modificada por la DF primera del RDL 43/2026, publicado el 25/11/2026 y vigente desde el 26/11/2026. Para obligados del art. 3.1, la adaptación queda vinculada al plazo de la DF cuarta.1.b del RD 238/2026. La Orden HAC/1028/2026 entró en vigor el 06/10/2026 y el plazo de veinticuatro meses conduce al **06/10/2028**; AEAT informa que los sistemas deben estar adaptados antes de esa fecha.
- Las fechas 01/01/2027 y 01/07/2027 asociadas al RDL 15/2025 en el material previo son históricas y no deben presentarse como calendario vigente de los usuarios al corte de este addendum.
- El plazo de productores/comercializadores del art. 3.2 es distinto: nueve meses desde la entrada en vigor de la Orden HAC/1177/2024 (29/10/2024), con la previsión específica para sistemas incluidos en contratos de mantenimiento plurianual anteriores al plazo. No se traslada automáticamente a 2028.
- Este cambio temporal no modifica por sí mismo el ámbito subjetivo/objetivo, las exclusiones, la modalidad VERI*FACTU voluntaria ni la decisión de producto de mantener perfiles deshabilitados. Deben distinguirse RRSIF, VERI*FACTU y factura electrónica B2B. La fecha debe revalidarse antes de cualquier habilitación u operación futura.

**Fuentes:** RD 1007/2023, DF cuarta consolidada y redacción vigente desde 26/11/2026; RDL 43/2026, DF primera y final; RD 238/2026, DF cuarta.1.b; Orden HAC/1028/2026, DF única; nota informativa AEAT de ampliación de plazo, actualización 25/11/2026. Los textos consolidados BOE son informativos; para fines jurídicos prevalece la publicación oficial.

### 13.2 Remisión VERI*FACTU y límite por fichero

La revisión directa de la Orden HAC/1177/2024 distingue las reglas siguientes:

- El **art. 16** regula el control de flujo y la contingencia VERI*FACTU: parámetro inicial de espera de 60 segundos entre envíos, ajustable conforme al parámetro de respuesta; siguiente envío al vencer el intervalo o alcanzarse el máximo de registros aplicable, lo que ocurra antes; recuperación tan pronto como sea posible y en orden cronológico; reintento al menos horario durante la incidencia; comunicación de la incidencia e información visible de los pendientes mientras subsista alguno pendiente. El primer envío no debe demorarse por la espera entre envíos.
- El límite de **1.000 registros es por fichero remitido** y se encuentra en el **anexo, apartado 2.2**, no como cifra autónoma en el art. 16. No debe confundirse con el máximo/parametrización del control de flujo.
- La remisión puede desacoplarse técnicamente, pero la asincronía no permite demorarla discrecionalmente ni sustituye el envío continuado exigido para VERI*FACTU.
- No confundir el art. 9 del RD 1007/2023 (alta simultánea o inmediatamente anterior a la expedición) con el art. 9 de la Orden HAC/1177/2024 (registro de eventos). El art. 16 de la Orden contiene la contingencia VERI*FACTU.
- Por el art. 3 de la Orden, mientras opera como VERI*FACTU no son aplicables, entre otros, los arts. 8 y 9 de esa Orden. Esto no elimina la conservación de las facturas ni permite perder/borrar registros todavía pendientes de remitir.

La clasificación precedente es lectura normativa documental, no aceptación del contrato/adaptador, validación de un protocolo concreto ni cierre de OQ-0010. Se deben fijar versiones técnicas y oráculos del adaptador por la vía competente.

### 13.3 QR, declaración responsable, representación y conservación

- Orden HAC/1177/2024, arts. 20–21: dimensiones QR 30×30 a 40×40 mm, ISO/IEC 18004 y corrección M son especificaciones reglamentarias en el ámbito aplicable; para factura electrónica estructurada la URL puede sustituir la imagen QR como campo independiente. La mención VERI*FACTU solo corresponde a la modalidad indicada. Falta fijar y probar la especificación técnica AEAT versionada del QR/URL; no se acredita un QR implementado.
- La declaración responsable corresponde al productor/sistema y sus componentes funcionales conforme al art. 1.2 y art. 15 de la Orden. Debe evaluarse por producto, componente y versión; no toda librería/dependencia constituye automáticamente componente fiscal. Su declaración no es homologación AEAT. La entidad responsable y el inventario real de componentes siguen sin acreditarse.
- Separar encargo de expedición material, facultad de remitir ante AEAT, otras actuaciones y tratamiento de datos. Un certificado electrónico identifica al remitente, pero no acredita por sí solo todas las facultades. Requieren validación legal/fiscal los mandatos, representación, colaboración social, alcance y revocación.
- No establecer un plazo de conservación universal. Separar factura, registro SIF, respuestas/remisiones, mandatos, declaraciones, auditoría, copias y datos derivados; identificar sujeto obligado, finalidad, fuente, hito inicial, ampliaciones/interrupciones, acceso y destino final. La obligación de conservar la factura no desaparece por remitir registros a AEAT. Para VERI*FACTU no trasladar automáticamente los requisitos de conservación/exportación propios del régimen NO VERI*FACTU.

### 13.4 Límites y estado

El resultado consultivo recibido y las revisiones documentales asistidas no son un dictamen fiscal firmado ni una aceptación atribuible de Tax/Legal/SIF. Permanecen sin cambios: perfiles `CANDIDATE / DISABLED`; OQ-0010 `OPEN`; ADR-0001 `PROPOSED`; readiness `BLOCKED — NO IMPLEMENTATION HANDOFF`. El addendum no habilita perfiles, no aprueba arquitectura ni autoriza desarrollo, pruebas, distribución u operación.

**Fuentes oficiales consultadas:** [RD 1007/2023 consolidado](https://www.boe.es/buscar/act.php?id=BOE-A-2023-24840); [RDL 43/2026](https://www.boe.es/buscar/doc.php?id=BOE-A-2026-24701); [RD 238/2026](https://www.boe.es/eli/es/rd/2026/03/25/238/con); [Orden HAC/1028/2026](https://www.boe.es/buscar/act.php?id=BOE-A-2026-20587); [Orden HAC/1177/2024 consolidada](https://www.boe.es/buscar/act.php?id=BOE-A-2024-22138); [AEAT — nota ampliación de plazo](https://sede.agenciatributaria.gob.es/Sede/iva/sistemas-informaticos-facturacion-verifactu/nota-informativa-ampliacion-plazo-adaptacion-facturacion.html), actualizada 25/11/2026; [AEAT — FAQ SIF/VERI*FACTU](https://sede.agenciatributaria.gob.es/Sede/iva/sistemas-informaticos-facturacion-verifactu/preguntas-frecuentes.html), actualizada 27/11/2026.

### 13.5 Consulta externa reducida propuesta — no enviada

El contraste oficial reduce la consulta: no volver a preguntar si el calendario de usuarios es 01/01/2027/01/07/2027, si el máximo por fichero es 1.000, si el anexo normativo exige QR de 30–40 mm/M, ni si Orden art. 16 contiene la contingencia. Los puntos siguientes requieren asesoría y/o definición del sistema real:

1. **Aplicabilidad individual TAX-001…005:** confirmar, con la cadena normativa vigente, filtros y excepciones por emisor/operación/modalidad; indicar evidencia real necesaria. No se solicita volver a decidir las fechas ni habilitar perfiles.
2. **Representación por actuación:** revisar encargo de expedición material, facultad de remisión ante AEAT, otras actuaciones y tratamiento/subencargo; identificar evidencias y efectos de revocación. Distinguir certificado electrónico de mandato/representación.
3. **Productor y componentes:** determinar, sobre el producto y componentes efectivamente existentes, qué entidad realiza funciones de producción/comercialización, qué declaración corresponde a cada sistema/componente/versión y cómo se conserva y facilita. No se atribuye esta responsabilidad a una entidad por contrato o marca solamente.
4. **Correcciones y conservación:** precisar por categoría los supuestos de rectificación, anulación y subsanación y su evidencia; determinar sujeto, fuente, hito, interrupciones/ampliaciones, acceso y destino de factura, registro, respuesta, mandato, declaración y evidencia, sin plazo universal.
5. **QR/URL ejecutable:** identificar versión vigente del documento técnico AEAT, estructura/encoding/endpoint aplicable y oráculos necesarios antes de validación técnica. La norma dimensional ya está identificada, pero no hay QR implementado ni bundle técnico probado.
6. **Confirmación de alcance consultivo:** identificar profesional, competencia, fecha, fuentes y hechos cubiertos, conclusiones condicionadas y límites. La respuesta debe quedar diferenciada de decisión de Producto, aceptación técnica, aprobación de ADR y autorización de implementación.

La consulta reducida conserva fuera de alcance mappings/outcomes/duplicados y elegibilidad por línea de OQ-0010; estos requieren su proceso separado con aceptación atribuible Tax/SIF. No se adjunta ni se atribuye una respuesta externa recibida como dictamen firmado.
