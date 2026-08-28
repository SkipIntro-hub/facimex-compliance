# Módulo Compliance — Framework de specs para desarrollo

**Destinatario:** equipo de desarrollo MatchFin
**Antecede:** `compliance-encuadre-interno.md` (contexto, diagnóstico y roadmap)
**Convención:** misma nomenclatura que el proyecto Blotter — `Fn-nn`

---

## 1. Reglas de escritura de specs para este módulo

Cinco reglas propias de Compliance, distintas a las del Blotter. Van al inicio de cada spec.

### R1 — Toda spec declara qué desaparece
El principio rector del proyecto es que no se digitaliza el flujo actual, se rediseña. Una spec que describe el proceso de hoy con una pantalla encima está mal escrita. **Campo obligatorio: "Qué deja de hacerse".**

### R2 — Separar regla estable de parámetro variable
Ninguna regla normativa se hardcodea. Toda spec que implemente una norma la descompone en tres capas:

| Capa | Ejemplo | Dónde vive |
|---|---|---|
| **Dato crudo** | % de participación accionaria de cada entidad | Modelo de datos (C1) |
| **Parámetro** | Umbral de dominancia para grupo económico: 10% / 20% | Tabla de parámetros, editable sin deploy |
| **Regla** | Si una del grupo liquida contra dólares, la otra pierde acceso al MULC | Motor de reglas (C2) |

Criterio:  (Gabriel De Seta). **Si un cambio normativo obliga a un deploy, la spec está mal descompuesta.**

### R3 — Parametrización por entidad desde el día uno
Facimex y PISA aplican criterios distintos sobre las mismas normas. Nada de criterio single-tenant con parche multi-entidad después. **Toda tabla de parámetros lleva `entidad_id`.**

### R4 — El sistema expone el output, no el insumo
Existe una caja negra legítima: información que Compliance usa para dictaminar y que no debe exponerse. El sistema publica **el resultado** (cupo, restricción, semáforo, habilitación) hacia Comercial y hacia el cliente. **El insumo del dictamen queda en el ámbito de Compliance.** Esto define permisos de lectura, no solo de escritura.

### R5 — El humano entra en el paso siguiente a la contingencia, no en la contingencia
Regla de diseño para todo flujo de excepción:

```
detección → acción automática (siempre la misma) → recolección completa → HUMANO → decisión → registro
```

No: `detección → HUMANO`. Si la respuesta a una contingencia es siempre la misma, se automatiza. El humano decide **con todo el material ya sobre la mesa**.

---

## 2. Plantilla de spec

```markdown
# Fn-nn · <Título>

**Capacidad:** C<n> — <nombre>
**Fase:** F<n>  ·  **Estimación:** <sem>  ·  **Estado:** <Pendiente|En curso|Listo>
**Depende de:** <IDs>  ·  **Bloquea a:** <IDs>
**Input de negocio requerido:** <B1..B8 o "ninguno">

## 1. Objetivo funcional
Una frase. Qué resuelve, para quién.

## 2. Qué deja de hacerse  ← obligatorio (R1)
Qué tarea manual, planilla o paso del proceso actual se elimina.

## 3. Actores y permisos
| Rol | Puede | No puede |

## 4. Modelo de datos
Entidades nuevas o afectadas. Relaciones. Campos con tipo y obligatoriedad.
Marcar cuáles son dato crudo (C1) y cuáles derivados.

## 5. Reglas de negocio
Descompuestas según R2: dato / parámetro / regla.
Tabla de parámetros con entidad_id, valor por defecto y responsable de definirlo.

## 6. Flujo
Camino feliz + flujos de excepción. Explicitar dónde entra el humano (R5).

## 7. Criterios de aceptación
Formato Dado / Cuando / Entonces. Numerados. Verificables sin ambigüedad.

## 8. Integraciones
Sistemas externos, endpoints, modo (push/pull/batch), manejo de indisponibilidad.

## 9. Fuera de alcance
Explícito. Qué NO hace esta spec y en qué Fn-nn se hace.

## 10. Riesgo normativo
Qué norma toca, qué pasa si la regla se implementa mal, qué queda auditado.

## 11. Trazabilidad al relevamiento
Cita textual del participante que originó el requerimiento.
```

**Sobre el punto 11:** parece burocrático y no lo es. En un módulo de Compliance, la mitad de los requerimientos son criterios de una persona, no reglas escritas en ningún lado. Cuando el dev tenga una duda de interpretación dentro de seis semanas, la cita le dice **a quién preguntarle**.

---

## 3. Backlog por fase

Estado: 🟢 especificable ya · 🟡 requiere input de negocio · ⚪ dependiente de fase previa

### F1 — Núcleo de datos y legajo digital

| ID | Título | Capacidad | Estado |
|---|---|---|---|
| F1-01 | Modelo de datos desagregado por entidad — persona humana / jurídica y subtipos | C1 | 🟢 |
| F1-02 | Motor de nexos entre entidades — grupo económico, directores comunes, participación accionaria | C1 | 🟢 |
| F1-03 | Modelo de combos gerente–custodio para FCI + gestor de pendientes documentales | C1 | 🟢 |
| F1-04 | Ficha única de entidad — vista consolidada por CUIT | C1 | 🟢 |
| F1-05 | Ingesta documental: carga por comitente y clasificación de documentos | C3 | 🟢 |
| F1-06 | Motor de extracción por checklist — set de preguntas por default | C3 | 🟡 B2 |
| F1-07 | Búsqueda a demanda sobre el corpus documental del comitente | C3 | 🟢 |
| F1-08 | Reporte de faltantes — qué preguntas quedaron sin responder | C3 | 🟡 B2 |
| F1-09 | Registro de vencimientos documentales + alertas a 3 meses | C3 | ⚪ F1-06 |
| F1-10 | Tabla de parámetros normativos con `entidad_id` — base del motor de reglas | C2 | 🟢 |

**Nota sobre F1-03:** el gestor de pendientes es requisito explícito de Federico. Un botón "resolver combo" sin gestor de pendientes **empeora** la situación: habilita operar con clientes no documentados sin dejar rastro de la deuda documental.

**Nota sobre F1-09:** existe trabajo previo de Camacho que no funciona por falta de inputs registrados. Revisar antes de construir — puede ser reutilizable una vez que F1-06 alimente los datos.

### F2 — Onboarding escalonado

| ID | Título | Capacidad | Estado |
|---|---|---|---|
| F2-01 | Modelo de niveles de habilitación y transiciones | C4 | 🟡 B1 |
| F2-02 | Motor de desbloqueo — requisitos por nivel y validación de cumplimiento | C4 | 🟡 B1, B3 |
| F2-03 | Alta de comitente sin habilitación operativa (comitente abierto ≠ operativo) | C4 | 🟢 |
| F2-04 | Separación pedir / firmar — clasificación de campos del legajo | C4 | 🟡 **B6 sin dueño** |
| F2-05 | Generación del extracto firmable + trazabilidad a lo declarado en digital | C4 | ⚪ F2-04 |
| F2-06 | Validación automática de firma digital vía web del Gobierno | C4 | 🟢 |
| F2-07 | Clasificación de riesgo con datos mínimos — matriz multivariable | C2 | 🟡 B3 |
| F2-08 | Re-evaluación de nivel ante evento de screening (PEP, OFAC, lista) | C4+C5 | ⚪ F3 |

**F2-04 es el ítem crítico de la fase y no tiene dueño de negocio.** Sin criterio de qué se firma y qué no, F2-05 tampoco existe.

**Sobre F2-06:** tres vías de validación relevadas — página del Gobierno, Encode, Adobe local. **Ir por la web del Gobierno**: Federico señaló que los certificados locales hay que actualizarlos manualmente, la web siempre está actualizada. Válido para Argentina, Uruguay y al menos un país más.

### F3 — Screening & Monitoring consolidado · track paralelo

| ID | Título | Capacidad | Estado |
|---|---|---|---|
| F3-01 | Consolidación de RePET + Sujeto Obligado bajo interfaz única | C5 | 🟢 · base ya construida |
| F3-02 | Integración Worldsys — PEP y listas internacionales | C5 | 🔄 en curso |
| F3-03 | Monitoreo periódico — reevaluación de cartera contra listas | C5 | 🟢 |
| F3-04 | Consulta por CUIT sobre no-clientes y no-prospectos | C5 | 🟢 |
| F3-05 | Integración PJN — causas judiciales, fuero nacional/provincial/municipal | C5 | 🟡 B8 |
| F3-06 | Integración Nosis — estructura societaria y capital accionario | C5 | 🟢 |
| F3-07 | Cláusula de derecho de admisión: modelo, versionado y aceptación en legajo | C10 | 🟢 |
| F3-08 | Bloqueo automático por evento de screening + notificación al cliente | C10 | ⚪ F3-07 |
| F3-09 | Override de Compliance con argumentación y registro | C10 | ⚪ F3-08 |

**F3-04 es la spec de mayor apalancamiento comercial de todo el módulo** y la más barata: la consulta por CUIT no requiere convenio ni relación con el sujeto. Federico: 

**F3-08 y F3-09 son inseparables.** Bloquear sin override registrado deja a Compliance sin salida operativa y garantiza que el bloqueo se termine desactivando por presión comercial.

### F4 — Pre-Trade Auditor y cupo dinámico

| ID | Título | Capacidad | Estado |
|---|---|---|---|
| F4-01 | Motor de las 4 preguntas — qué / cuánto / quién / por qué vía | C6 | ⚪ F2 |
| F4-02 | Validación de canal: rechazo de orden cursada por vía no habilitada | C6 | 🟢 |
| F4-03 | Validación de persona habilitada a instruir | C6 | 🟢 |
| F4-04 | Cupo operativo — panel de control configurable por cliente | C6 | 🟡 B4 |
| F4-05 | Cálculo de cupo a partir de variables, no de carga manual del resultado | C6 | 🟡 B4 |
| F4-06 | Consumo y liberación de cupo por tipo de operación y documento | C6 | 🟡 B4 · requiere C9 |
| F4-07 | Motor de contingencias — acción automática + recolección + handoff a humano | C6 | ⚪ F4-04 |
| F4-08 | Vínculo automático entre resultado del análisis y cada operatoria | C6 | 🟢 |
| F4-09 | Restricciones por inversor calificado | C2 | 🟢 |
| F4-10 | Validación de perfil de inversor vs. instrumento operado | C2 | 🟢 |
| F4-11 | Pantalla única de permisos para el cliente | C10 | ⚪ F4-01 |

**Sobre F4-04:** la definición de Gabriel es que MatchFin **no devuelva un número sino un panel**. Configurable por cliente: acumular vs. netear según ingresos y egresos, márgenes del 40 o 60% sobre activo corriente según riesgo, extras tomables al 100% con caducidad a los 2 meses porque la liquidez se perdió. 

**Sobre F4-06:** es la spec con mayor dependencia de C9. Requisito de Pablo: MatchFin tiene que saber, **por cada tipo de documento y cada tipo de operación**, si suma o resta cupo. Sin integración limpia con Aune / Visual Bolsa, no se puede construir.

**Sobre F4-10:** hoy no se controla. La norma no lo exige del todo, pero permitir que un conservador opere instrumentos agresivos sin advertencia es exposición innecesaria. La salida acordada no es prohibir sino **advertir y registrar que el cliente asume el riesgo**.

### F5 — Hub de reportes y conducta · ~

| ID | Título | Capacidad | Estado |
|---|---|---|---|
| F5-01 | Inventario de reportes — catálogo con fuentes, template y destinatario | C8 | 🟡 B5 |
| F5-02 | Motor de composición — coctelera de fuentes a template | C8 | ⚪ F5-01 |
| F5-03 | Piloto: 3 reportes de sectores distintos | C8 | ⚪ F5-02 |
| F5-04 | Control inverso automático — validación por proceso espejo | C8 | ⚪ F5-02 |
| F5-05 | Base estadística de comportamiento operado por cliente | C7 | 🟢 |
| F5-06 | Motor de alertas configurable — KPIs más allá del exceso de cupo | C7 | 🟢 |
| F5-07 | Carga de características de producto — comportamiento esperable | C7 | 🟡 B7 |
| F5-08 | Detección de patrones: churning, rotación sin cambio de duration, rulos | C7 | ⚪ F5-07 |
| F5-09 | Generación automática de ROI | C7 | 🟢 |
| F5-10 | Ciclo de vida de ROS — plazos UIF, ROS sin comitente, requerimientos derivados | C7 | 🟢 |

**Sobre F5-04:**  (Gabriel). El control humano de reportería masiva introduce más error del que detecta. Se reemplaza por proceso espejo + supervisión muestral periódica.

**Sobre F5-10:** el ROS **no cuelga del comitente**. Puede existir un ROS de alguien a quien nunca se le abrió la cuenta — una operación tentada. El modelo de datos tiene que soportar ROS huérfano de comitente, y distinguir los tres carriles UIF: lavado, financiamiento del terrorismo, proliferación de armas, cada uno con sus propios plazos.

---

## 4. Spec desarrollada de ejemplo

Sirve como referencia de nivel de detalle esperado. Elegí F1-06 porque es la de mayor ROI de F1 y la que mejor muestra las cinco reglas aplicadas.

---

# F1-06 · Motor de extracción por checklist

**Capacidad:** C3 — Ingesta documental inteligente
**Fase:** F1 · **Estimación:** 3-4 semanas · **Estado:** Pendiente
**Depende de:** F1-05 (carga y clasificación), F1-01 (modelo de entidad)
**Bloquea a:** F1-08, F1-09, F2-07
**Input de negocio requerido:** **B2** — checklist de preguntas por Pablo + Federico

## 1. Objetivo funcional
Que un analista de Compliance obtenga, sobre el corpus documental de un comitente, las respuestas a un set finito y predefinido de preguntas — sin abrir los documentos uno por uno.

## 2. Qué deja de hacerse
Se elimina la **revisión manual documento por documento** para extraer datos societarios. Hoy es control manual sobre lo que el cliente sube en el onboarding: poderes, estatutos, actas, libro de registro de acciones.

**No se elimina** el criterio del analista sobre el resultado. El sistema extrae y señala faltantes; **no dictamina**.

## 3. Actores y permisos

| Rol | Puede | No puede |
|---|---|---|
| Analista de Compliance | Ejecutar extracción, ver resultados, corregir valores extraídos, agregar preguntas ad hoc | Editar el checklist por default |
| Responsable de Compliance | Todo lo anterior + editar el checklist por default de su entidad | Editar el de otra entidad (R3) |
| Comercial | — | Sin acceso a resultados de extracción (R4) |

## 4. Modelo de datos

**Entidades nuevas**
- `checklist_pregunta` — `id`, `entidad_id`, `codigo`, `texto`, `tipo_dato` (fecha/texto/numérico/booleano/porcentaje), `tipo_documento_esperado`, `es_default`, `activa`
- `extraccion` — `id`, `comitente_id`, `fecha`, `usuario_id`, `estado`, `documentos_alcanzados[]`
- `extraccion_respuesta` — `id`, `extraccion_id`, `pregunta_id`, `valor`, `confianza`, `documento_origen_id`, `ubicacion_en_documento`, `estado` (respondida / no_encontrada / ambigua), `valor_corregido`, `corregido_por`

**Campos derivados que alimentan C1** — al confirmarse, escriben en el núcleo de datos:
`vigencia_sociedad`, `fecha_vencimiento_sociedad`, `objeto_social`, `composicion_accionaria[]`, `beneficiarios_finales[]`, `apoderados[]` con `alcance`, `facultades`, `fecha_vencimiento`, `modo_firma` (conjunta/indistinta)

**Trazabilidad obligatoria:** todo valor extraído guarda documento de origen y ubicación. Un dato de Compliance sin fuente citable no es auditable.

## 5. Reglas de negocio

| Capa | Contenido |
|---|---|
| **Dato crudo** | Contenido de los documentos cargados |
| **Parámetro** (`entidad_id`) | Set de preguntas por default · umbral de confianza para marcar "ambigua" · tipos de documento esperados por tipo societario |
| **Regla** | Toda pregunta del checklist activo se evalúa contra el corpus completo, no contra un documento individual · Confianza bajo umbral ⇒ estado `ambigua`, nunca `respondida` · Vigencia de sociedad expresada en años ⇒ calcular fecha de vencimiento contra fecha de constitución |

**Set de preguntas por default (provisorio — pendiente B2):**

| Código | Pregunta | Tipo | Documento esperado |
|---|---|---|---|
| VIG-01 | Vigencia de la sociedad | numérico (años) | Estatuto |
| VIG-02 | Fecha de constitución | fecha | Estatuto |
| OBJ-01 | Objeto social | texto | Estatuto |
| ACC-01 | Composición accionaria y % por accionista | lista | Estatuto / libro de registro |
| ACC-02 | Versión vigente del estatuto | numérico | Estatuto |
| BEN-01 | Beneficiarios finales | lista | DDJJ / estatuto |
| POD-01 | Apoderados habilitados | lista | Poder |
| POD-02 | Alcance y facultades por apoderado | texto | Poder |
| POD-03 | Fecha de vencimiento del poder | fecha | Poder |
| POD-04 | Modo de firma: conjunta o indistinta | enum | Poder |

## 6. Flujo

**Camino feliz**
1. Analista abre la ficha del comitente y ejecuta "Extraer datos"
2. El sistema toma todos los documentos cargados y activos
3. Evalúa cada pregunta del checklist activo de la entidad
4. Devuelve tabla: pregunta → valor → documento de origen → confianza
5. Analista revisa, corrige lo que corresponda y confirma
6. Los valores confirmados escriben en el núcleo de datos (C1)

**Excepción — pregunta sin respuesta**
Estado `no_encontrada`. Alimenta F1-08 (reporte de faltantes). **No bloquea la confirmación del resto.**

**Excepción — confianza bajo umbral**
Estado `ambigua`. Se muestra el valor propuesto con la cita, marcado. **Requiere confirmación explícita del analista** — nunca escribe en C1 sin intervención.

**Aplicación de R5:** el sistema **no** interrumpe al analista pregunta por pregunta. Procesa todo, recolecta todo, y **recién entonces** presenta el conjunto para revisión humana.

## 7. Criterios de aceptación

1. **Dado** un comitente con estatuto cargado, **cuando** se ejecuta la extracción, **entonces** VIG-01, VIG-02 y OBJ-01 devuelven valor con cita al documento y página.
2. **Dado** un estatuto que expresa vigencia en años, **cuando** se extrae VIG-01, **entonces** el sistema calcula y persiste la fecha de vencimiento contra la fecha de constitución.
3. **Dado** un comitente sin poderes cargados, **cuando** se ejecuta la extracción, **entonces** POD-01 a POD-04 quedan en estado `no_encontrada` y el resto se procesa normalmente.
4. **Dado** un valor extraído con confianza bajo el umbral de la entidad, **cuando** se presenta el resultado, **entonces** queda en estado `ambigua` y **no** escribe en el núcleo de datos hasta confirmación explícita.
5. **Dado** un analista que corrige un valor extraído, **cuando** confirma, **entonces** se persiste el valor corregido, el original, el usuario y el timestamp.
6. **Dado** un responsable de Compliance de la entidad A, **cuando** intenta editar el checklist de la entidad B, **entonces** el sistema lo rechaza server-side.
7. **Dado** un usuario con rol Comercial, **cuando** intenta acceder a resultados de extracción, **entonces** el sistema lo rechaza server-side.
8. **Dado** un comitente con estatuto en múltiples versiones, **cuando** se extrae ACC-02, **entonces** el sistema identifica la versión vigente y lo indica.

**Sobre el criterio 6 y 7:** en el Blotter, el enforcement server-side de permisos quedó como punto abierto. Acá se especifica como criterio de aceptación explícito para no repetirlo.

## 8. Integraciones
Ninguna externa en esta spec. Consume el almacenamiento documental de F1-05 y escribe en el modelo de C1.

## 9. Fuera de alcance
- Búsqueda a demanda fuera del checklist → **F1-07**
- Reporte consolidado de faltantes → **F1-08**
- Alertas por vencimiento de documentos → **F1-09**
- Clasificación de riesgo a partir de los datos extraídos → **F2-07**
- **Cualquier dictamen automático.** El sistema extrae; Compliance dictamina.

## 10. Riesgo normativo
Toca directamente la diligencia de conocimiento del cliente (Res. UIF 78). Una extracción incorrecta que escriba en el núcleo de datos sin revisión propaga el error a la clasificación de riesgo, al cupo y a los reportes. **Por eso el estado `ambigua` no escribe nunca sin confirmación humana, y toda escritura queda trazada a documento, página y usuario.**

## 11. Trazabilidad al relevamiento
- **Pablo Ruiz:** identificó la necesidad como "lector de PDF con IA" — beneficiarios finales, vencimiento de poderes, datos puntuales que hoy se revisan a mano.
- **Gastón:** reencuadre del requerimiento — no es un lector, es un set finito de preguntas contra un corpus. 
- **Gabriel De Seta:** aportó el detalle del set — vigencia de la sociedad calculada a fecha, versiones del estatuto, cambios accionarios, libro de registro. Y el requisito de búsqueda a demanda además del default.
- **Federico Villegas:** viabilidad — . Y el alcance: artículos del estatuto, poderes, facultades.
