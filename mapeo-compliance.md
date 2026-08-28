# Mapeo de Compliance — Esquematización de la reunión

**Fecha del audio:** 27/08/2026 · **Duración:** 2h 16m · **Fuente:** transcripción automática de Zoom (calidad baja — ver §0.2)

---

## 0. Encuadre

### 0.1 Participantes y rol funcional

| Persona | Rol según lo dicho en la reunión |
|---|---|
| **Gastón** (identificado como "." en la transcripción) | Conduce. Releva para construir el mapa funcional que después se vuelca a MatchFin. No aporta contenido de compliance: extrae, ordena, etiqueta y devuelve modelo. |
| **Federico Villegas** | A cargo de Compliance en **Facimex**. Voz principal del relevamiento. Se va de viaje — de ahí la urgencia de la reunión. |
| **Pablo Ruiz** | Compliance del lado **PISA / PISA**. Pasó por Facimex. Aporta el contraste entre ambas realidades y el encuadre normativo estricto. |
| **Gabriel De Seta** | Perfil normativo-operativo senior. Aporta la capa de arquitectura de datos y de reportería. Se suma tarde a la llamada. |
| **Mauro Barnatan** | PISA, +2 años. Interviene poco pero en puntos precisos: matriz de riesgo, control de fondeo, credencial PJN. |

### 0.2 Advertencia sobre la fuente

La transcripción tiene degradación fonética severa. Las siguientes normalizaciones se aplicaron en todo este documento:

| En la transcripción | Se lee | Confianza |
|---|---|---|
| "cumpleaños", "con playas", "complayers", "complence" | **Compliance** | Alta |
| "Marfil", "Machwin", "Matching", "Max fin", "madz" | **MatchFin** | Alta |
| "fácil", "fassi", "fascinar", "faz", "Fashie" | **Facimex** | Alta |
| "lonboarding", "longboarding", "lombording", "el amor" | **onboarding** | Alta |
| "Wi Fi", "Wii", "la Ue", "huiste" | **UIF** | Alta |
| "cine B", "C.N.B", "S.N.B" | **CNV** | Alta |
| "el central", "B.C.R.a" | **BCRA** | Alta |
| "quid", "cuid", "quit", "circuito", "Wii" | **CUIT** | Alta |
| "la 78" | **Res. UIF 78/2023** (agentes del mercado de capitales) | Media-alta |
| "Mab", "Map" | **MAV** (segmentos garantizado / no garantizado / avalado) | Media-alta |
| "contado con ligui / con lic" | **Contado con Liqui (CCL)** | Alta |
| "responsables Cripto" | **Responsable Inscripto** | Media-alta |
| "falta", "Facta", "fadcast" | **FATCA** · "doble 8 / doble W" = **W-8 / W-9** | Alta |
| "workshark", "work check", "worces" | proveedor de listas — **World-Check / Worldsys** | Media |
| "gnosis" | **Nosis** | Media-alta |
| "repat", "repair" | **RePET** | Alta |
| "P.J.M", "B.J.N", "pejote" | **PJN** (Poder Judicial de la Nación) | Alta |
| "Aune", "Une", "Audi", "audiovisual bolsa" | **Aune** y **Visual Bolsa** (sistemas de gestión) | Media |
| "rollo", "roi", "Rolf", "roce", "rost", "rosal" | **ROI** (Reporte de Operación Inusual) / **ROS** (Reporte de Operación Sospechosa) | Alta |
| "Pre Trail log", "P.R.I", "Free Lob", "Ditrate" | **Pre-Trade Log (PTL)** | Alta |
| "R.C.M / R.C.A" | Reportes sistemáticos UIF (mensual / anual) | Media |
| "coco", "balance" | **Cocos**, **Balanz** (referencias de mercado) | Media-alta |
| "Alex", "liga", "league", "Alik", "lic" | **ALyC** | Alta |
| "Camacho", "Cammy" | persona del equipo MatchFin | Media |

### 0.3 Objetivo declarado y método impuesto por Gastón

- **Pedido literal:** "contame un cuentito para alguien que no tiene ni idea… cómo empieza el día, cómo es el pulpo, hasta dónde llega". Primero **macro**, después detalle.
- **Recorte explícito:** el **funcionamiento operativo diario** de Compliance, **no** sus objetivos ni su misión.
- **Formato prohibido:** presentación. Se busca relato para poder etiquetar procesos.
- **Regla de trabajo:** "vamos a separar lo actual, lo real y lo potencial" — todo el mapeo distingue **estado actual / estado deseado**.
- **Principio rector enunciado dos veces (Gastón y Federico):** *no se digitaliza el quilombo analógico; se rediseña el flujo al pasarlo a digital.*

---

## 1. Arquitectura conceptual acordada

Federico propone la estructura y todos la adoptan. Es el esqueleto de todo el mapeo:

```
                    LÍNEA DE VIDA DEL CLIENTE (verticales secuenciales)
   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
   │  A. APERTURA │──▶│ B. PRE-      │──▶│ C. OPERATORIA│──▶│ D. POST-     │
   │  DE CUENTA   │   │ OPERATORIO   │   │              │   │ OPERATORIO   │
   └──────────────┘   └──────────────┘   └──────────────┘   └──────────────┘
   ══════════════════════════════════════════════════════════════════════════
   ║  E. NORMATIVO  (UIF / CNV / BCRA / ARCA) — atraviesa las 4 verticales  ║
   ══════════════════════════════════════════════════════════════════════════
   ║  F. PARAMETRIZACIÓN DE CUENTA Y LEGAJOS — atraviesa las 4 verticales   ║
   ══════════════════════════════════════════════════════════════════════════
```

**Definición de las capas transversales según Federico:**
- **E — Normativo:** es transversal porque siempre hay que tenerlo presente, tanto por los cambios como por lo que es fijo.
- **F — Parametrización:**  — canales habilitados, personas habilitadas a instruir, alcance del perfil transaccional. Es la capa donde cada agente ejerce su discrecionalidad.

**Nota de contexto operativo:** Compliance **no trabaja limitado al horario de mercado**. A diferencia de otras áreas de back office, no hay horario a rajatabla. Por eso el mapeo se ordena por **ciclo de vida del cliente**, no por jornada laboral.

---

## 2. VERTICAL A — Apertura de cuenta

### A.1 Primera bifurcación: tipo de sujeto

El **CUIT es el disparador** de todo el árbol documental. Identifica el tipo de cuenta y de eso se desprende qué se pide.

```
CUIT
├── Persona humana ──── simple
│                  ├─── cotitular
│                  ├─── menor de edad          ← abanico ampliado recientemente
│                  └─── sucesión
└── Persona jurídica ── S.A. capital nacional
                   ├─── S.A. capital extranjero
                   ├─── S.R.L.
                   ├─── fideicomiso
                   ├─── S.A. con beneficiarios finales persona humana
                   └─── FCI (gerente + custodio)   ← NO cubierto por el onboarding
```

### A.2 Cobertura actual del onboarding: el gap real

| Afirmación | Quién |
|---|---|
| El onboarding cubre "más de la mitad" de los casos | Federico |
| Lectura de Gastón: *falta un 40 y pico % para ser el onboarding ideal* | Gastón |
| Corrección de Federico: **no es tanto** — lo que falta es "ir al fino": fideicomisos, S.A. con capital extranjero, estructuras societarias complejas | Federico |
| Para la cartera de clientes ya cargada en MatchFin: **100% acomodado** | Federico |
| **Excepción: FCI.** Se migró la base, pero el onboarding **no está preparado para abrir fondos** | Federico |
| Lo de FCI se resolvió con un Excel volcado al Pre-Trade Log para poder hacer los controles — **no** es apertura de cuenta | Gabriel |
| Toda la información volcada hoy en el onboarding **cumple con lo normativo vigente** | Federico (respuesta afirmativa expresa) |

**Atenuante que Federico opone a su propio gap:** abrió 3 gerentes en 10 años y 50 gerentes en 1 año — el muestreo de fondos ya está cubierto. Son pocos los fondos que hay que abrir desde cero. Por eso no se le puso foco.

### A.3 Ranking de participación por volumen operado (Facimex)

Dato pedido expresamente por Gastón para dimensionar el gap contra lo vertebral:

1. **FCI** — lo que más opera
2. **Compañías de seguros / aseguradoras**
3. **Corporativo**
4. **Retail** — última instancia, y seleccionado

> **Relación crítica:** el segmento de mayor volumen (FCI) es exactamente el que **no** está cubierto por el onboarding. El gap no es marginal en términos de participación, aunque sí lo sea en cantidad de altas nuevas.
>
> Federico advierte que este ranking es específico de Facimex: , con Compliance dividido por tipo de estructura.

### A.4 Mecanismo FCI: apertura por "combos"

- Un fondo tiene **un gerente** y **uno o más custodios**.
- Se documenta un **juego de legajos por combo** (gerente × custodio).
- Si el combo ya está firmado, cualquier fondo administrado por ese gerente y custodiado por ese custodio **se abre sin salir del sector**: se le da número y se acomoda contra la documentación ya existente.
- **Excepción:** piden número de comitente de un fondo cuyo **combo** no está documentado (no es que falte el papel del fondo — falta el papel del combo).

**Propuesta de Gastón:** un botón "resolver combo" → se cargan las contrapartes → se disparan las gestiones de pedido de documentación.
**Respuesta de Federico:** sirve, *pero* obliga a tener un **gestor de pendientes**, porque el reverso es que ya estás operando con clientes no documentados.

### A.5 Práctica excepcional: número de comitente sin legajo

Punto sensible, levantado y clasificado en la reunión:

- Dar de alta un comitente **no requiere información del cliente** — el dato es en cierto modo público. Se le pone el número y el nombre, se replica en Caja de Valores, y el comitente existe.
- Esto **se replica en cualquier comitente**, no solo en fondos.
- Se puede tener número de comitente **sin perfil de inversor ni nada de eso**.
- Cuando pasa, **no se documenta** ("te muestro que me lo firmó antes de abrirlo en caja, todo a puño y letra").

**Etiquetado acordado:** proceso **excepcional y discrecional**. Alcanza con que intervenga el humano y quede registro entre las contrapartes que lo resolvieron.

### A.6 El nudo del legajo: 15 hojas

Este es el debate más largo de la vertical. Hay **tres posiciones distintas** que Gastón fuerza a explicitar:

| Posición | Quién | Argumento |
|---|---|---|
| **Reducir** | Federico | El output es un convenio de 15 hojas que casi siempre terminan firmando a puño y letra. Reducir en cantidad de hojas **y** en contenido. |
| **Mantener lo que se pide** | Pablo |  En Facimex se trabaja con empresas muy grandes: siempre se buscó pedir toda la documentación societaria aunque sea de más. |
| **Separar pedir de firmar** | Federico (refinamiento) |  No sacaría nada del legajo, porque toda la información sirve — se reduciría **lo que tiene que firmar el cliente**. |

**Precisión clave de Federico sobre qué exige realmente la norma (Res. 78):**
- Exige: legajo con los principales datos de la empresa + **firma del perfil transaccional**.
- **No** exige firmar: que sos Responsable Inscripto; la fecha de inicio de actividades en ARCA; la fecha del estatuto; la composición accionaria; la declaración FATCA (para FATCA corresponde **W-8 / W-9**, no una firma declarativa).
- Los datos impositivos sí importan operativamente (definen si se cobran o no aranceles), pero eso no los vuelve firmables.
- **Efecto perverso concreto:** el onboarding **exige taxativamente** completar campos como la fecha de inicio de actividades en ARCA. Si no está, no se llega a la instancia de firma. Y si se saltea, la ficha queda incompleta → .

**Contra-argumento estructural de Pablo (el que explica el sobre-pedido):**
> Al cliente le pedís más o menos documentación según sea de riesgo bajo / medio / alto. **Pero el riesgo bajo lo concluís recién cuando ya pediste toda la documentación.** Esa es la gran diferencia — y ahí está la oportunidad de mejora.

Esto es exactamente lo que habilita la propuesta de A.9. Pablo también aclara que Comercial lo reclama históricamente.

**Riesgo que Gastón identifica en la reducción:** o se firma por todo, o hay una firma que contempla lo que está en el extracto **y** lo que no está, porque es un todo. Sin criterio explícito de qué queda dentro y qué fuera, se rompe la cobertura. Pregunta abierta: **quién define ese criterio.**

### A.7 Firma: hológrafa vs. digital

- **El 100% del onboarding es firmado por el cliente.**
- Ambas modalidades están habilitadas — **hológrafa o digital** — y **elige el cliente**.
- El cliente carga sus propios datos en la plataforma (aunque muchas veces lo trae Comercial u otro sistema).
- El tiempo de carga es variable (se puede discontinuar y retomar un mes después); la extensión del documento es fija.

**Pedido concreto de Gabriel De Seta — validación de firma digital:**
1. La ley asimila la firma digital a la hológrafa.
2. Existen tres vías de validación: (a) página del Gobierno — se sube el archivo, devuelve firma correcta/incorrecta; (b) **Encode**, homologado para certificación digital; (c) Adobe local, descargando e instalando certificados.
3. **Pedido:** que **MatchFin valide automáticamente** los PDFs firmados digitalmente y muestre el resultado al costado del documento.
4. Válido para Argentina, Uruguay y al menos un país más.
5. Federico observa que los certificados hay que ir actualizándolos; por eso **conviene ir por la vía web del Gobierno**, que siempre está actualizada.

**Estado:** aceptado por Gastón como **instancia de control adicional dentro del proceso**.

### A.8 Distinción conceptual que Federico exige mantener

> **Riesgo LA/FT ≠ perfil transaccional ≠ riesgo crediticio.** Son tres cosas distintas y no se pueden colapsar.

- Escucha a colegas hablar del riesgo LA/FT como si fuera riesgo crediticio. **Hay que separarlos.**
- Un comitente **trabaja con su disponible**: para operar hay que fondear. Si cargás el CUIT, vas a la IF (información financiera) y te dice que el prospecto está recontra apalancado, . (Aunque sí se usan liquidez e índice de apalancamiento como indicadores.)
- **El riesgo real viene por el origen de fondos**, no por el apalancamiento.
- El **riesgo LA/FT sí se puede anticipar en grandes dimensiones** con información pública, apenas se conoce la estructura societaria. El **perfil transaccional** va en paralelo y necesita balance.
- **Confesión operativa:**  Ejemplo: una S.R.L. es riesgo medio sin muchas vueltas; una S.A. con capital nacional conocido, menos.

**Contrapunto de Mauro (limitación técnica a la propuesta escalonada):**
> La matriz de riesgo **pondera múltiples variables**. Un PEP puede terminar dando riesgo bajo. Ningún parámetro aislado define la categoría. Por lo tanto **necesitás muchos datos para poder decir "es riesgo bajo, ya no te pido más"** — y ahí volvés al problema del sobre-pedido.

**Respuesta de Pablo:** entonces la definición es cuáles son los **datos mínimos** que tienen que estar en el onboarding para determinar riesgo lo más rápido posible.

**Matiz de Federico sobre PEP:** no todo PEP pesa igual. Importa si el PEP es apoderado o si **puede inyectar capital**. No es lo mismo un PEP en una S.R.L. que un director PEP de una S.A. sin capacidad de inyectar capital. Después se va a beneficiarios finales o al extranjero. Herramientas tipo **Nosis** no dan el 100% del capital accionario pero dan panorama.

### A.9 ★ PROPUESTA CENTRAL: onboarding escalonado

Es el output conceptual más importante de la reunión. Nace del cruce entre el reclamo de Federico (aligerar) y la restricción de Pablo (no se puede saber el riesgo antes de pedir todo).

**Formulación de Gastón:** invertir el orden de los factores.

> Hoy: **perfilo → clasifico → habilito.**
> Propuesta: **habilito un nivel mínimo → el cliente opera dentro de ese nivel → cuando quiere más, le pido lo necesario para desbloquear el nivel siguiente.**

**Metáfora de la cubetera:** cajones de acrílico vacíos en fila. El agua (información) va llenando el primero; cuando rebalsa, pasa al siguiente. Con un chorrito fino llenás dos o tres y ahí te frenás. Cada fila llena = un nivel operativo desbloqueado.
- Llenar filas 1, 2 y 3 → **ya hay cuenta comitente**.
- ¿Querés más? Aparecen los requerimientos de la instancia siguiente.
- En cada desbloqueo hay **ratificación del perfilamiento**.
- **Beneficio de percepción:**  en lugar de "llename 200.000 km de formulario".

**Variable de escalonamiento — dos candidatas discutidas:**
| Variable | Postura |
|---|---|
| Por **instrumento** (dólares sí, bonos y acciones no) | Propuesta inicial de Gastón. Pablo: . |
| Por **monto** (ej. 2 palos → 20 palos) | Sugerida por Pablo, adoptada por Gastón como vara de trabajo. **Es la que queda.** |

**Adhesiones:**
- **Federico:** . Precisa la formulación correcta: **la apertura es una sola — lo que se divide en etapas es la diligencia.** Distingue: se puede tener la comitente **sin que esté operativa**.
- **Pablo:** acepta con reservas (ver A.10). Reconoce que simplifica para el cliente.
- **Gabriel:** alineado.

**Condición habilitante que Gastón identifica:** esto no se podía hacer antes porque **no había fuente de control**. Ahora con RePET (y las demás fuentes) se puede: 

**Argumento comercial:** una cuenta escalonada puede operar desde temprano dentro de lo validado, en el mismo tiempo en que hoy una apertura tradicional **puede terminar en la nada**.

### A.10 Objeciones y condiciones a la propuesta escalonada

| # | Objeción | Autor | Estado |
|---|---|---|---|
| 1 | La decisión de riesgo puede tener que tomarse **al momento de la apertura**, no después: si al cargar nombre y apellido salta PEP u OFAC, el riesgo que creías bajo pasa a medio/alto y hay que pedir más. | Pablo | **Integrada** por Gastón: es exactamente el disparador de desbloqueo. Y el volumen que pasa limpio va por la vía rápida en lugar de "comerse el proceso" por la excepción. |
| 2 | La matriz de riesgo es multivariable — un solo dato no clasifica. | Mauro | Abierta → deriva en la tarea "definir datos mínimos". |
| 3 | La norma da flexibilidad (no obliga a pedir origen de fondos en todos los casos), **pero** habla de **materialidad y significatividad**. No siempre va de la mano del monto. | Pablo | Abierta — es la restricción que condiciona la vara. |
| 4 |  — la norma permite umbrales (p. ej. por debajo de $10M no pedir nada) pero Federico lo considera inaceptable. | Federico | **Tensión no resuelta con la propuesta de escalonar por monto.** |

### A.11 Lectura automática de documentos

Levantado por Pablo como oportunidad de automatización ("un lector de PDF con IA"). Gastón lo reencuadra de inmediato:

> **No pienses en la herramienta. Decime qué relevás, para qué, y cuáles son los documentos recurrentes.**
> Modelo propuesto: el cliente carga todo en la carpeta del comitente. Después vos le preguntás al conjunto de documentos todo lo que necesitás encontrar. Si los documentos no responden alguna pregunta, **tenés claridad inmediata de qué te falta**.

**Datos que hoy se revisan a mano** (aportes de Pablo, Federico y Gabriel):
- Beneficiarios finales / accionistas
- Poderes: alcance, facultades, **vencimiento**, si es firma conjunta o indistinta
- Artículos del estatuto y **objeto social**
- **Vigencia de la sociedad** (ej. 99 años → calcular fecha de vencimiento → ¿está vigente?)
- Versiones del estatuto (v1, v2, v3), cambios accionarios, libro de registro de acciones
- Porcentaje de participación de cada accionista

**Requisito funcional que se desprende:** además de extraer los ~10 datos por default, hay que poder **buscar campos puntuales a demanda** y tener **un espacio donde volcar el resultado**.

**Argumento de viabilidad de Federico:** . Gastón: la matriz de preguntas es **finita** (10, 15, 100 preguntas — no infinitas).

**Estado:** ✅ acordado como automatización concreta. **Tarea asignada** (ver §7).

**Consecuencia encadenada (Pablo → Gabriel → Gastón):** los datos extraídos quedan registrados como input y habilitan la **actualización de legajos**: si un documento vence en 1 o 2 años, hoy hay que correr atrás del cliente. Gastón: **alertas automáticas 3 meses antes del vencimiento.** Pablo aclara que "Camacho" ya armó algo, pero **faltan los inputs registrados** — o sea, la lectura de documentos es el prerrequisito.

### A.12 Alta en Caja de Valores

- El sistema de gestión (**Aune**) **ya toma automáticamente casi todos los datos del onboarding**. Funciona bien; se editan muy pocos campos.
- **El último paso sigue siendo manual:** cargar los datos del comitente en **Caja de Valores**. ~10 minutos por comitente (dato de Mauro). Es obligatorio por normativa.
- Se presume que Facimex tiene el mismo problema.

**Vías de solución exploradas en vivo:**
- ¿Existe API de Caja de Valores? → Gastón: si la tiene Aune, la podemos tener nosotros directo.
- Mauro: Aune **tendría** un sistema para pasar info a Caja de Valores, pero en PISA **no está activo ni probado**.
- Gabriel menciona una carga masiva / código fuente aprovechable.
- Alternativa: legajo homologado que Caja de Valores reconozca.

---

## 3. VERTICAL B — Preoperatorio

### B.1 Problema raíz declarado

> ** — Federico

**Definición de información completa (los 5 campos):**
1. Qué cliente está operando
2. Qué está operando (especie)
3. Cuándo está operando
4. Volumen
5. Monto



### B.2 Cadena causal del problema

```
No existe planilla única donde ver toda la información
        ↓
Compliance está a ciegas
        ↓
Los sectores no tienen comunicación al 100% en la diaria
        ↓
Compliance se entera de que alguien operó DESPUÉS de que operó
        ↓
No hay preoperatorio: hay TAPAR una operación que no debió hacerse
```

- Frecuencia: ** (confirmado dos veces).
- Causa estructural: cuando Compliance no tiene estructura formal, **no interroga al comercial**; y el comercial no tiene criterio ni obligación de traer la consulta.
- Etiquetado de Gastón: **vicio estructural — bypass del proceso de validación.**

### B.3 El problema es bidireccional

Federico insiste: **falta información de entrada Y de salida.**

> Hoy le preguntás a un comercial cuál es el perfil transaccional de un comitente y **no lo va a saber** — porque Compliance no se lo comunicó.

**Precisión importante:** no es que Compliance carezca del dato (está dentro de la diligencia y del "conozca a su cliente"). **Es que no lo comunica.** En parte por reflejo de no divulgar información.

**La "caja negra" del dictamen (acordado explícitamente):**
- Hay información que Compliance usa para llegar a un resultado y que **no necesita estar en el proceso**.
- 
- Gastón lo formula: **existe una caja negra de información por la que Compliance no debe dar explicaciones, pero que sí cambia el dictamen.** Federico: "exacto".
- **Implicancia de diseño:** el sistema debe exponer **el output** (número, cupo, restricción, semáforo) sin exponer el insumo.

### B.4 Debate normativo — ¿está Compliance obligado a intervenir antes?

**Pablo introduce la corrección más importante de la reunión:**

> **Por normativa, Compliance NO está obligado a saber antes de que el cliente opere.** No hay obligación de que toda operación pase por un bloqueo de Compliance. Enterarse después **no es estar en falta**.

**Pero** enumera lo que sí conviene o sí es taxativo:
- **Cupo operativo:** hoy se enteran del exceso **después**. Anticiparse es sano, no obligatorio.
- **Perfil de inversor:** anticipar que el cliente quiere operar algo que no se condice con su perfil. Si es conservador, en rigor no debería poder operar instrumentos agresivos — .
- **Inversor calificado:** **taxativo**. Si dejás que un cliente no calificado opere algo reservado a calificados, **estás en falta**.
- **Salida transparente:** advertir al cliente que está fuera de su perfil y que asume el riesgo. Gastón: si firmó una cadena no restrictiva, es libre de tomar el riesgo advertido.

**Reformulación de Federico que cierra el debate:**

### B.5 ★ Modelo objetivo: el cerco invisible

Síntesis de Gastón, aceptada por todos:

```
1. PRESETEO      perfil de inversor + cupo operativo + parametrización
                 = "encorsetás la cancha donde se puede mover el cliente"
                          ↓
2. ZONA VERDE    todo lo que hace dentro de los parámetros NO genera alertas.
                 Es esperable y regular. Gastó el 50% del cupo? Mientras esté
                 dentro del 50% restante, no pasa nada.
                          ↓
3. TRANSGRESIÓN  al cruzar el cerco → alarmas en TIEMPO REAL
                          ↓
4. INTERVENCIÓN  recién ahí interviene el humano, con tiempo y con red
                          ↓
5. OVERRIDE      Compliance tiene la potestad de aceptar igual bajo su
                 responsabilidad → dispara OTRO flujo de responsabilidades
                 y de registro
```

**Por qué hoy no existe:**  La rutina volvió aceptable la ausencia de intervención humana.

### B.6 Automatización de la contingencia (aporte de Federico)

Refinamiento importante sobre **dónde** entra el humano:

> La intervención humana **no** se da ante la contingencia, sino **en el paso siguiente** a la contingencia. Las contingencias también pueden estar automatizadas.

Ejemplo desarrollado:
1. Se excede el cupo operativo → **automático**
2. Se pide documentación (siempre la misma) → **automático**
3. Ya está el balance sobre la mesa → **acá entra el humano**: 

**Formulación final de Gastón, validada:** ante la contingencia se dispara un flujo automático que recolecta todos los requerimientos; **recién con todo recolectado aparece el humano** para definir qué condiciones adicionales pone para normalizar la operación transgredida. Federico: .

**Condición técnica que Federico agrega:** que los resultados del análisis de Compliance queden **anexados y vinculados a cada operatoria de manera automática**.

### B.7 Las 4 preguntas — condición de cierre del preoperatorio

Federico define el criterio de suficiencia. Si estas cuatro están respondidas, el preoperatorio está completo:

| # | Pregunta | De dónde sale la respuesta |
|---|---|---|
| 1 | **¿Qué** puede operar? | Diligencia + cuestionario + cierre de riesgo que da el analista |
| 2 | **¿Cuánto** puede operar? | **Cupo operativo** — sale en primera instancia del balance. Cerrado, pero expandible con tolerancia |
| 3 | **¿Quién** lo puede operar? | Personas habilitadas a cursar órdenes — se define en la apertura/actualización |
| 4 | **¿De qué manera** / por qué vía? | Canales habilitados (Provider, e-mail, sistema mediado, sistema auditado) — cruce entre lo que el agente tiene habilitado y lo que el cliente pidió |

> **Control derivado:** aunque tengas el canal Reuters habilitado, **si vino por otro canal la operación no debería cruzarse.**

Estas 4 preguntas son las que Federico había anticipado al inicio como . **Requisito previo:** tener la cuenta abierta **y parametrizada**.

---

## 4. VERTICAL C — Operatoria

### C.1 Criterio de clasificación

Federico rechaza dos criterios y elige uno:

| Criterio | Veredicto |
|---|---|
| Por **instrumento** | ❌ El mercado secundario argentino es básicamente renta fija, pública o privada. No discrimina. |
| Por **plazo / duration** | ❌ No aporta al control. |
| Por **tipo de operación** | ✅ **Es el criterio correcto.** |

**Taxonomía resultante:**
```
├── Mercado primario ......... queda fuera (casi no se ve)
├── Mercado secundario ....... núcleo del análisis
├── Toma de caución
├── Operatorias MAV .......... garantizado / NO garantizado / avalado
├── Futuros
├── Colocación de fondos ..... "estás operando moneda, básicamente"
└── Circuitos por contrato X . mundo mucho más amplio; gran oportunidad
                               de cada ALyC de cara a su cliente → PARKING
```

### C.2 Ranking de criticidad

| Nivel | Operatoria | Fundamento |
|---|---|---|
| 🔴 **Alto** | **Contado con Liqui** — "cuando el cliente está sacando plata del país" | Es el foco explícito |
| 🟡 **Medio-bajo** | Renta variable |  — a mucha menor distancia que CCL |
| 🟡 **Encuadre normativo** | **MAV no garantizado** (compra o venta) | No es riesgo LA/FT pero tiene **mucho encuadre normativo**. Hay que tener a mano qué completar del cliente. Garantizado y avalado son **mucho más flexibles** |
| ⚪ **Bajo / inactivo** | Futuros | No están entrando clientes nuevos a operar futuros |
| ⚪ **No crítico** | Colocación de fondos | No se considera problemática |

### C.3 Estado deseado: silencio operativo

- **Gastón:** todo el trabajo bien cumplimentado debería dar posiciones sanas para aprobar operaciones a la escala de cada uno.
- **Federico corrige el marco:** 
- **Convergencia:** solo debería saltar **lo excepcional**.

Federico cierra: en el preoperatorio, **el "qué" es lo más difícil y lo más ambiguo**.

### C.4 ★ Zona ambigua: la negativa al cliente

El punto más delicado de la vertical. Se aborda desde el caso "el cliente pregunta por qué no lo dejan hacer CCL".

**Cómo se maneja hoy:**
- Compliance **le pasa la pelota al comercial**.
- A veces reunión cliente + comercial + Compliance para explicar.
- **Nunca por escrito**: 
- Federico sí ha comunicado negativas a clientes, **pero siempre informalmente y con el comercial enterado**.
- **No queda registro de nada de eso.**
- **Consecuencia:** 

**Riesgos identificados:**

| Riesgo | Autor |
|---|---|
| Perder o tensionar la relación con el cliente | Pablo |
| Que el cliente **inicie causa judicial** | Federico |
| Retención de fondos: si el cliente tiene posición, en cierto modo le estás reteniendo la plata | Federico |
| Contra-argumento: si el cliente quiere irse, se le devuelve la plata y punto — no debería llegarse a reclamo judicial | Pablo |

**El caso real que motiva todo (Pablo):** clientes que aparecen en medios o en causas públicas (menciona el rubro construcción y la causa de los cuadernos).  Federico agrega que en aperturas del interior .

**Gastón fuerza la definición:** 

### C.5 ★ Solución de diseño acordada: despersonalizar la negativa

Federico: 

**Los cuatro componentes:**

1. **Cláusula de derecho de admisión en el legajo de apertura** — firmada de antemano: si el cliente salta en alguno de estos controles, aplica bloqueo. 
2. **Reglas + parametrización** que hacen el corte automáticamente al detectar.
3. **Reporting accesible al cliente**, para que sepa que se le aplicó ese rigor en relación a lo que firmó.
4. **Override siempre disponible**, con argumentación, reunión y registro.

**Pantalla de transparencia (idea de Federico, integrada):** que el cliente vea en **una sola pantalla** su resumen: perfil transaccional, qué tiene permitido, si puede tomar caución y hasta qué cupo. **Que se entere antes de querer hacerlo, no cuando lo quiere hacer.**

**Marco conceptual que aporta Gastón:**  → **. El cliente sabe que está en los diarios; si el proceso lo detecta y aplica el rigor, **no hay culpa que asumir**: nosotros no pusimos al cliente en esa situación.

**Beneficio declarado:** elimina una fricción, mitiga un riesgo, y **exime a Comercial de tener que comunicar una objeción** — el proceso es el que lo hizo.

**Salvedad no resuelta de Pablo:** más allá del proceso, sigue habiendo una **decisión comercial, política y de riesgo de lavado** sobre si se lo deja operar o no. Y: .

### C.6 Fuentes externas de screening — mapa

| Fuente | Cubre | Estado / observación |
|---|---|---|
| **RePET** | Terrorismo (UIF) | ✅ Ya implementado en MatchFin. Es el precedente que habilita el modelo de reglas por fuente |
| **OFAC** | Sanciones EE.UU. | Mencionado como caso de escalamiento de riesgo |
| **World-Check / Worldsys** (proveedor de listas) | Listas de sanciones, causas de corrupción, "un millón de listados en el mundo" | Modelo pull periódico: cargás todos tus clientes y el proveedor avisa cuando alguno aparece. **No cubre PJN** |
| **PJN** | Causas judiciales, incluido fuero municipal/provincial | ⚠️ **Gap identificado.** , requiere matrícula/credenciales. **Mauro tiene credencial de PJN.** También podría accederse vía los abogados. Estuvieron reuniéndose con "Cami"/MatchFin para ver cómo sacar información |
| **Nosis** | Estructura societaria, capital accionario | No da el 100% pero  |
| **ARCA / IF** | Balances, apalancamiento | Federico: no debería mover la aguja del riesgo LA/FT |

**Instrucción de método de Gastón:** 

**Ventaja estructural que señala Federico:** todas estas fuentes son **información a la que se accede sin convenio firmado**, sobre alguien que **ni siquiera es cliente ni prospecto** — basta un CUIT. Gastón: 

### C.7 ★ Cupo operativo dinámico

Traído por Pablo desde una conversación con **Agustín Córdoba** (semana previa), quien lo quiere definir lo antes posible porque **no todas las empresas usan el mismo criterio**.

**El problema no es definir el cupo — es cómo se consume y se actualiza:**
- Si el cliente pone plata / justifica más → ¿se incrementa?
- Si retira porque vendió posiciones → ¿libera cupo?
- ¿Se acumula o se netea?

**Estado del diseño (según Gastón):** venían hacia un cupo dinámico en tiempo real, con un corte tipo "colchón" que amortigua: llega información nueva, acomoda y muestra el nuevo colchón.

**★ Contrapropuesta de Gabriel — no un resultado, un panel de control:**
> Que MatchFin **no devuelva un número**, sino un **panel donde cada Compliance Officer configure su propio criterio**:
> - acumular vs. netear según ingresos y egresos
> - configurable **cliente por cliente**
> - márgenes del 40% o 60% sobre el activo corriente según el riesgo asignado
> - extras tomables al 100%, pero con **caducidad** (ej. a los 2 meses se borra porque la liquidez se perdió)
>

**Prerrequisito duro (Pablo):** MatchFin tiene que estar **perfectamente integrado con el sistema de gestión** (Aune, Visual Bolsa, el que sea) y saber, **por cada tipo de documento y tipo de operación**, si suma o resta cupo. Gastón: .

**Vinculación normativa:** Pablo lo ata explícitamente a **prevención de lavado** — es control crítico.

**Estado actual (falla identificada):** en MatchFin **estaba previsto** el cálculo del nuevo cupo operativo, pero **hoy no está configurado** para que el dato resultante impacte en Aune. En la apertura sí funciona. Federico marca el matiz: .

---

## 5. VERTICAL D — Postoperatorio

### D.1 Sesgo diagnóstico declarado


**Consecuencia lógica:** el postoperatorio actual está inflado por ausencia de control previo. **Una alerta que llega tarde deja de ser alerta temprana.** Si Compliance permitió el excedido sabiendo, no es una alerta: es una decisión.

**Efecto esperado del modelo objetivo:** con controles previos, 

**Lo que queda genuinamente en el postoperatorio (Gabriel):**
1. **Lo estadístico** — información real de clientes que ya operaron
2. **Las contingencias eventuales** que hay que salir a resolver por algo que se dejó pasar en el PTL / pre-trade

### D.2 Estadística y comportamiento

- Volcar todo a base de datos: cuánto opera, cada cuánto, un cliente que opera en junio y opera más a fin de año.
- Gastón lo etiqueta: **calidad de clientes / comportamiento del cliente.**

### D.3 Errores humanos y fallas técnicas

Pregunta de Gastón: 

**Respuesta de Federico:** 

**Caso testigo (Pablo):** olvidarse de ingresar un nuevo cupo operativo en el sistema. Consecuencia: el exceso pasa desapercibido.

**Gastón desarma el caso hasta la causa raíz:**
```
Un humano calcula el cupo en una planilla/papel aparte
        ↓
Carga el resultado como nominal directo en el sistema
        ↓
El sistema queda desfasado cuando el input cambia
```
**Solución acordada:** cargar las **variables** en el sistema para que **calcule el cupo, lo topee y lo aplique** desde ahí mismo.

**Formulación de Gabriel que se adopta como principio:**

**Otro punto de trabajo manual identificado:** eventual actualización del perfil transaccional / perfil de inversor. También automatizable.

**Antecedente:** en PISA hay una planilla armada, "flujo de compliance".

### D.4 ★ Hub de reportes

**Inventario mencionado (parcial y con transcripción dudosa):**

| Destino | Reporte | Frecuencia |
|---|---|---|
| **UIF** | RSM/RCM | ~2 por mes |
| **UIF** | RSA/RCA | 1 por año |
| **CNV** | Múltiples formularios | Creciente. Hoy hay que reportar mercado primario, operaciones con extranjeros, saldos, operaciones y movimientos por cuentas extranjeras, pasivo, liquidez |

**Observación de alcance (Pablo) — clave:**
> A la CNV se reporta **cada vez más información, mucho más allá de Compliance**. Cada ALyC tiene que reportar varios ítems y formularios, y **no importa quién los distribuya**. Hay reportes de **contabilidad**, no solo de Compliance.

**Gabriel:** 
**Gastón:** 

**Diseño propuesto — módulo Hub de Reportes:**
```
Fuente A ─┐
Fuente B ─┼──▶ [COCTELERA] ──▶ template de formato ──▶ validación ──▶
Fuente C ─┘                                                    reporte generado
                                                                     ↓
                                                          supervisión humana
                                                                     ↓
                                                             CNV / UIF / etc.
```
- Cada reporte declara **de dónde toma sus fuentes**.
- Los reportes deben salir **casi certificados para mandar a ciegas**, con la fidelidad y profundidad normativa del organismo destinatario.
- La supervisión humana se mantiene en el iterar continuo para que los reportes queden bien afinados.

**Estrategia de implementación (Gastón):** listar el 100% de los reportes → elegir **3 esenciales**, preferentemente de sectores distintos → **prueba piloto** que deje el modelo replicable para el resto. Compliance se hace cargo de la información propia e indica dónde está la información de terceros; Gastón se ocupa de traer a los otros sectores.

### D.5 ★ Control del control

**Aporte de Gabriel, aceptado sin objeción:**
> La magnitud de reportes es tal que **no es lógico que el control sea humano**. El control debería ser **el mismo proceso a la inversa**, corrido sobre otro input.
>
> Fundamento: 

**Gastón:** hay más riesgo en la corrección humana que en dejar pasar lo automatizado, que probablemente esté correcto. Se corre un proceso de supervisión cada tanto para verificar estabilidad.

### D.6 ★ ROI / ROS — ciclos de vida

Distinción que **Gabriel exige mantener separada**:

| | **ROI — Reporte de Operación Inusual** | **ROS — Reporte de Operación Sospechosa** |
|---|---|---|
| Naturaleza | Interno. Por fuera de lo que un comercial podría llegar a hacer | Normativo |
| Disparador | Una alerta eventual que puede terminar en algo más | La inusualidad **se trasladó a un incumplimiento normativo** |
| Destino | Interno | **UIF**, con plazos que cumplir |
| Volumen | Muchos | Algunos ROI eventualmente terminan en ROS |

**Reglas particulares del ROS:**
- **Tiene ciclo de vida independiente del comitente.**
- **Puede haber ROS sin comitente**: alguien que intentó abrir cuenta para intentar una operación (operación **tentada**). La norma obliga a reportarlo aunque no se le haya abierto la cuenta.
- Tras enviarse a CNV o UIF, puede volver un **requerimiento derivado** en auditorías.
- **La UIF es muy clara** en tiempos y parámetros una vez identificado el nicho: prevención de **lavado**, de **financiamiento del terrorismo** o de **proliferación de armas** — son tres carriles distintos.

**Pedido de Gastón:** que **el sistema genere el ROI/ROS automáticamente** y llegue a supervisión; el humano decide si se eleva. De mínima, la primera instancia la hace el sistema.

### D.7 ★ Monitoreo de patrones de comportamiento

Nace de un deseo de Pablo y escala en la conversación.

**Pedido de Pablo:** más tipos de alertas e indicadores (KPIs), no solo el exceso de cupo. Ejemplo: reporte periódico de clientes que operan de forma inconsistente con su perfil de inversor, aun teniendo override.

**Aporte de Gabriel — *churning*:**
> Detectar **rotación de cartera** que solo genera comisión.
> - Si rota a la **misma duration** con papeles similares → **no estás rotando, estás generando comisión.**
> - Si rota a **duration más larga** → puede ser legítimo.
> - Caso extremo: personas humanas que arman un rulo y **se roban la comisión entre un grupo de personas**.
> - Implica **incumplimiento del código de ética**, no necesariamente un ROS.

**Restricción que Gabriel impone al diseño:**

**Requisito funcional derivado (Gastón):** hay que **cargar las características del producto** para poder definir qué comportamiento es esperable y qué no.

**Encuadre de Gabriel:**  Gastón lo adopta.

**Visión de Gastón:** un **hub de alertas** alimentable de forma independiente del sistema, con sofisticación de segunda y tercera línea de lectura, que anticipe quilombos futuros o detecte avivadas tempranas. 

**También mencionado por Gabriel:** esquemas de restricciones por **inversor calificado** — si no lo es, no puede operar determinados productos.

---

## 6. CAPA TRANSVERSAL E — Normativo

### E.1 Mapa de organismos y su naturaleza

Gabriel define los tres grandes (más ARCA y otros de menor peso). **La clave es que cada uno tiene una naturaleza distinta y por lo tanto una estrategia de tratamiento distinta:**

| Organismo | Qué regula | Forma de publicar | Tratabilidad |
|---|---|---|---|
| **BCRA** | Lo más **operativo** — qué podés y qué no podés hacer | Texto ordenado, **pero solo hasta cierta fecha** | 🟢 Parametrizable |
| **CNV** | **Guarda de documentación**, permanencia + operativo | **El más prolijo** — distribuido en títulos | 🟢 Parametrizable |
| **UIF** | Toda la **diligencia** | Resoluciones sueltas, **sin texto ordenado** | 🔴 Ambiguo — "es lo más social" |
| **ARCA** y otros | Impositivo | — | — |

**Sobre la UIF (el organismo más difícil):**
- No hay tabla de qué tenés que hacer y qué no.
- 
- **Lo ideal es siempre trabajar por sobre los márgenes de la UIF.**
- Ejemplo: hay resolución clara sobre qué es y qué no es PEP, con listado — .
- **Conclusión operativa: lo de UIF lo parametrizás vos.** Lo de BCRA y CNV es más binario.

### E.2 ★ Estrategia central: datos crudos desagregados

**Tesis de Gabriel:**

**Formalización de Gastón (validada):**
```
Matriz: cada CLIENTE es una fila · cada DATO es una columna
        ↓
El dato crudo, desagregado al máximo, arroja NEXOS entre filas
        ↓
La normativa se aplica como una CONSULTA sobre esa matriz
        ↓
Cambia la norma → cambia el parámetro, NO la estructura de datos
```

### E.3 Ejemplo trabajado: grupo económico → acceso al CCL

Gabriel lo desarrolla como caso canónico:

```
DATO CRUDO
  Cliente A: persona jurídica, composición accionaria, % de participación
  Cliente B: persona jurídica, composición accionaria, % de participación
        ↓
NEXO (derivado internamente)
  ¿Están vinculadas? Mismo grupo económico / mismos directores /
  coincidencia en participación accionaria
        ↓
PARÁMETRO NORMATIVO (BCRA — variable)
  Umbral de dominancia: ¿10%? ¿20%?  ← esto es lo que cambia
        ↓
REGLA (espíritu — estable)
  Si una sociedad del grupo liquida un activo contra dólares,
  la otra pierde el acceso al MULC. No quieren mezclar mercados.
```

**Segundo ejemplo (disposiciones transitorias CNV — permanencia):**
> El cliente tiene que tener un título cierto tiempo antes de liquidarlo contra determinada moneda.
> **Comunes denominadores estables:** `activo` + `permanencia` + `moneda`.
> Lo que cambia: qué moneda (hoy se empieza a hablar del euro), cuántos días de permanencia.

### E.4 Principio de diseño: espíritu vs. especificidad

**Gabriel:** 

**Gastón lo convierte en especificación del Hub Normativo:**
- El hub contiene la **matriz del espíritu** de cada norma (estable).
- Se le cargan las **aristas** de la normativa nueva para saber en qué rango de modificación aplica.
- Sabiendo que **la esencia es una**.
- Gastón lo llama **.

### E.5 Volatilidad como restricción de diseño

Gabriel:  Esta volatilidad es **la razón** de la estrategia E.2: si la estructura de datos es cruda y desagregada, la norma es una capa de reglas reemplazable.

**Cambios normativos en curso mencionados por Federico:**
- Cambio en la Ley de Mercado de Capitales → permite **objeto social más amplio**.
- Cambio en la forma en que un accionista puede **inyectar capital** en una empresa. 

---

## 7. Decisiones, tareas y compromisos

### 7.1 Decisiones tomadas

| # | Decisión | Estado |
|---|---|---|
| D1 | Estructura del mapeo: 4 verticales + 2 capas transversales | ✅ Consensuada |
| D2 | Se rediseña el flujo al digitalizar; no se traslada el flujo analógico | ✅ Principio rector |
| D3 | Onboarding escalonado con desbloqueo progresivo — **la apertura es una sola, lo que se escalona es la diligencia** | ✅ Consenso conceptual, falta definir la vara |
| D4 | Vara de escalonamiento: **por monto**, no por instrumento | ✅ Adoptada, pendiente de calibrar |
| D5 | Automatizar el **primer paso** de toda contingencia; el humano entra en el segundo | ✅ Acordado |
| D6 | Lectura automática de documentos con checklist finito de preguntas | ✅ Acordado |
| D7 | Validación automática de firma digital dentro de MatchFin, por vía web del Gobierno | ✅ Aceptado |
| D8 | Cláusula de derecho de admisión + bloqueo sistémico + reporting al cliente | ✅ Acordado — despersonaliza la negativa |
| D9 | Cupo operativo como **panel de control configurable**, no como número | ✅ Acordado |
| D10 | Hub de reportes con piloto de 3 reportes, abierto a sectores no-Compliance | ✅ Acordado |
| D11 | El control de los reportes es automático (proceso inverso), no humano | ✅ Acordado |
| D12 | Alertas de vencimiento documental 3 meses antes | ✅ Acordado |
| D13 | Hub normativo por espíritu de la norma + parámetros variables | ✅ Acordado |
| D14 | Método: primero necesidades y flujos, después proveedores | ✅ Regla de trabajo de Gastón |

### 7.2 Tareas asignadas ("tarea para el hogar")

| # | Tarea | Responsable | Origen |
|---|---|---|---|
| T1 | **Definir la vara y los rangos** de escalonamiento del onboarding — encontrar el patrón | **Pablo** | §A.9 |
| T2 | **Armar el cuestionario / checklist de control** sobre documentos: qué datos se buscan siempre y por default | **Pablo + Federico** (dijeron que lo arman juntos) | §A.11 |
| T3 | **Armar un Drive con el 100% de los reportes**: nombre + link a un modelo + descripción de cada uno | **Pablo + Gabriel** | §D.4 |
| T4 | **Definir el criterio de cálculo y consumo del cupo operativo** | **Agustín Córdoba** (lo pidió) + definición conjunta | §C.7 |
| T5 | **Pasar en limpio, unificar y devolver el mapeo** para revisarlo y detectar huecos | **Gastón** | Cierre |
| T6 | Documento posterior: **plan de trabajo + roadmap con módulos** e interrelaciones | **Gastón** | Cierre |

### 7.3 Compromiso de agenda

- **Reunión de continuidad al día siguiente.** Gastón suspende y reprograma otra reunión para aprovechar antes del viaje de Federico.
- Objetivo de esa reunión: ** — validar la foto, encontrar huecos, acordar el mapeo.
- Federico revisará los documentos **cuando vuelva**.

---

## 8. Tensiones y puntos no resueltos

| # | Tensión | Polos |
|---|---|---|
| ⚠️ 1 | **Escalonar por monto vs. origen de fondos siempre** | Pablo/Gastón proponen umbrales; Federico sostiene que operar sin origen de fondos , aunque la norma permita el umbral de $10M |
| ⚠️ 2 | **Reducir el legajo vs. pedir de más** | Federico quiere reducir lo firmable; Pablo defiende el sobre-pedido porque  y es lo que permite perfilar. Sin resolver: **quién define el criterio de corte** |
| ⚠️ 3 | **Datos mínimos para clasificar riesgo** | Mauro: la matriz es multivariable, un dato no alcanza. Pablo: hay que definir el set mínimo. **Sin definir** |
| ⚠️ 4 | **Materialidad / significatividad** | La norma no obliga siempre, pero exige evaluar significatividad — que no necesariamente va de la mano del monto. Condiciona T1 |
| ⚠️ 5 | **La decisión final sobre un cliente expuesto** | Automatizable el bloqueo, pero Pablo sostiene que queda una **decisión comercial, política y de riesgo de lavado** que ningún proceso resuelve |
| ⚠️ 6 | **Acceso a PJN** | Fuente crítica, no cubierta por el proveedor de listas, requiere credenciales. Mauro tiene una; sin decisión sobre la vía |
| ⚠️ 7 | **Firma parcial** | Si se firma un extracto de 5-8 hojas, ¿esa firma se extiende a lo declarado en digital? Gastón marca el riesgo; nadie lo resuelve |
| ⚠️ 8 | **Alcance del proyecto** | El hub de reportes **excede a Compliance** (llega a contabilidad y otros sectores). Gabriel lo señala, Gastón lo abre. Impacto no dimensionado |

---

## 9. Wishlist "si fuéramos la NASA"

Pregunta de cierre de Gastón: 

| Persona | Deseo |
|---|---|
| **Gabriel De Seta** | **Tener discriminados los datos crudos** (consistente con toda su línea argumental) |
| **Mauro Barnatan** | **Control de fondeo** hecho dentro de MatchFin + **acumulación de cupos** |
| **Pablo Ruiz** | Más tipos de **alertas e indicadores (KPIs)** para gestionar clientes: no solo exceso de cupo, sino reportes periódicos de operatoria inconsistente con el perfil de inversor, aun con override. *(Primero bromeó: "que la NASA vaya a buscar clientes en otros planetas.")* |
| **Federico Villegas** | No enunció un deseo explícito en el cierre, pero su posición atraviesa toda la reunión: **onboarding más liviano en lo firmable + diligencia automática por etapas** |

---

## 10. Mapa de dependencias entre iniciativas

Orden de precedencia técnica que se desprende del relevamiento:

```
[1] DATOS CRUDOS DESAGREGADOS (E.2)
     │  fundamento de todo lo demás
     ├──▶ [2] LECTURA AUTOMÁTICA DE DOCUMENTOS (A.11)
     │         └──▶ [3] ALERTAS DE VENCIMIENTO / ACTUALIZACIÓN DE LEGAJOS (A.11/D.3)
     │         └──▶ [4] ONBOARDING ESCALONADO (A.9)  ←─ requiere también fuentes externas
     │
     ├──▶ [5] HUB NORMATIVO (E.4)
     │         └──▶ [6] PARAMETRIZACIÓN DE REGLAS POR ORGANISMO
     │
     ├──▶ [7] FUENTES EXTERNAS (RePET ✅ / World-Check / PJN ⚠️ / Nosis)
     │         └──▶ [8] CLÁUSULA DE ADMISIÓN + BLOQUEO AUTOMÁTICO (C.5)
     │
     ├──▶ [9] PRE-TRADE LOG / cerco invisible (B.5)
     │         ├──▶ [10] CUPO OPERATIVO DINÁMICO — panel configurable (C.7)
     │         │          └── prerrequisito: integración total con Aune/Visual Bolsa
     │         ├──▶ [11] AUTOMATIZACIÓN DE CONTINGENCIAS (B.6)
     │         └──▶ [12] PANTALLA ÚNICA DE PERMISOS PARA EL CLIENTE (C.5)
     │
     └──▶ [13] HUB DE REPORTES (D.4)
               ├──▶ [14] CONTROL INVERSO AUTOMÁTICO (D.5)
               └──▶ [15] ROI/ROS AUTOMÁTICO (D.6)
                          └──▶ [16] MONITOREO DE PATRONES / CHURNING (D.7)
                                     └── prerrequisito: cargar características del producto
```

**Cuello de botella transversal:** la **integración MatchFin ↔ sistema de gestión (Aune / Visual Bolsa) ↔ Caja de Valores**. Aparece como prerrequisito en A.12, C.7 y D.3.
