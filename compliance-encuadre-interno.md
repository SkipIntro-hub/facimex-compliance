# Modernización de Compliance — Encuadre interno

**Proyecto:** Módulo Compliance sobre plataforma MatchFin
**Alcance inicial:** Facimex (ALyC) · **Extensión prevista:** PISA / PISA
**Documento:** encuadre para presentación interna — antecede a las specs técnicas
**Base:** relevamiento del 27/08/2026 con Federico Villegas, Pablo Ruiz, Gabriel De Seta y Mauro Barnatan

---

## 1. Qué es esto y por qué ahora

MatchFin opera como el **departamento de desarrollo de IT del grupo**. Compliance es el segundo proceso que entra en modernización bajo ese esquema, después del Blotter.

**El precedente importa como argumento:**

| Blotter | Compliance |
|---|---|
| Reemplazó 3 Excels independientes de blotter (Mesa 1 Corporativa, Mesa 2 Institucional, PISA Garantizado) | Reemplaza planillas, control manual, criterio informal y comunicación por fuera de sistema |
| Se desarrolló sobre la plataforma MatchFin, con backend, API, modelo de datos y roles server-side | Se desarrolla sobre la misma plataforma, reutilizando ese núcleo |
| Roadmap de 5 fases, ~7-9 meses las primeras 4 | Mismo formato de fases y nomenclatura |
| Dejó construido el **Pre-Trade Log**, que es el auditor preoperatorio | **El PTL es el punto de partida de este módulo, no un desarrollo nuevo** |

**Activos ya construidos que este módulo capitaliza:**
- **Pre-Trade Log / Pre-Trade Auditor** (vía Blotter) — la infraestructura del control preoperatorio ya existe
- **Verificador de Sujeto Obligado (UIF)** — terminado
- **Módulo de screening RePET** v1.4 con matching Jaro-Winkler y batería de test T01–T18 — terminado
- **Integración Worldsys** para PEP y listas internacionales — en curso

O sea: no se arranca de cero. Se arranca con **el control preoperatorio y el screening ya resueltos parcialmente**, y falta todo lo que los alimenta y todo lo que los consume.

---

## 2. Principio rector (no negociable)

> **No se digitaliza el flujo analógico. Se rediseña el flujo al pasarlo a digital.**

Lo enunciaron por separado Gastón y Federico en la reunión, con las mismas palabras: 

**Consecuencia práctica para el equipo de desarrollo:** si una spec describe un proceso actual con una pantalla encima, está mal escrita. Cada spec tiene que declarar explícitamente qué del proceso actual **desaparece**.

**Corolario aportado por Gabriel De Seta, que conviene citar en la presentación:**

---

## 3. Estado de situación — el diagnóstico en cuatro hechos

Para la exposición interna, todo el relevamiento se comprime en cuatro hechos verificados. Son los que justifican la inversión.

### 3.1 Compliance no interviene antes de la operación

No existe planilla única donde ver la información completa de una operación. Los sectores no tienen comunicación al 100% en la diaria. **Compliance se entera de que alguien operó después de que operó** — con frecuencia descrita como "bastante recurrente". Lo que hoy hay no es preoperatorio: es tapar una operación que no debió hacerse.

*Matiz normativo importante, aportado por Pablo Ruiz y que hay que decir en la presentación para no sobrevender:* **por normativa no existe obligación de que toda operación pase por un bloqueo de Compliance.** Enterarse después no es estar en falta. Salvo casos taxativos — inversor calificado es el ejemplo claro. El caso a favor del pre-trade es de **gestión de riesgo y de eficiencia**, no de cumplimiento formal.

### 3.2 La asimetría de información es bidireccional

No solo falta información hacia Compliance. **Falta información desde Compliance.** Hoy le preguntás a un comercial cuál es el perfil transaccional de un comitente y no lo sabe, porque Compliance no se lo comunicó.

Con una precisión de diseño que quedó acordada: existe una **caja negra legítima** — información que Compliance usa para llegar a un dictamen y que no necesita estar expuesta en el proceso. **El sistema expone el output (cupo, restricción, semáforo), no el insumo.**

### 3.3 El onboarding no cubre el segmento de mayor volumen

El onboarding actual cubre "más de la mitad" de los casos y cumple con lo normativo vigente. Lo que falta es "ir al fino": fideicomisos, S.A. con capital extranjero, estructuras complejas — **y fondos comunes de inversión.**

**El dato que hay que poner en la lámina:** el ranking de volumen operado en Facimex es FCI → aseguradoras → corporativo → retail. **El segmento que más opera es exactamente el que el onboarding no cubre.** Se resolvió migrando la base a un Excel volcado al Pre-Trade Log para poder controlar — eso no es apertura de cuenta.

*Atenuante honesto que conviene incluir:* son pocos los fondos que hay que abrir desde cero (3 gerentes en 10 años, 50 en un año, muestreo ya cubierto). El gap es chico en cantidad de altas y grande en participación de volumen.

### 3.4 El costo del onboarding se paga por adelantado y entero

Este es el hallazgo con mayor potencial de rediseño, y sale de un intercambio entre Federico y Pablo:

- Federico: el output es un convenio de 15 hojas que casi siempre se firma a puño y letra. La norma exige mucho menos de lo que se hace firmar.
- Pablo: . Y la razón estructural: **al cliente de riesgo bajo lo concluís riesgo bajo recién cuando ya le pediste toda la documentación.**

**Ahí está la ineficiencia estructural:** se paga el costo de la diligencia máxima para descubrir que no hacía falta. Y es exactamente lo que resuelve el onboarding escalonado.

---

## 4. La rotación de eje: de verticales a capacidades

El relevamiento se ordenó por ciclo de vida del cliente. **Esa estructura no sirve para construir**, porque los mismos componentes reaparecen en varias verticales.

```
ESTRUCTURA DEL RELEVAMIENTO                ESTRUCTURA DE CONSTRUCCIÓN
(cómo Compliance narra su día)             (cómo se desarrolla)

  A. Apertura        ─┐                    ┌─ C1  Núcleo de datos
  B. Preoperatorio   ─┤                    ├─ C2  Motor de reglas
  C. Operatoria      ─┼──── rotación ───▶  ├─ C3  Ingesta documental
  D. Postoperatorio  ─┤                    ├─ C4  Onboarding escalonado
  E. Normativo       ─┤                    ├─ C5  Screening & Monitoring
  F. Parametrización ─┘                    ├─ C6  Pre-Trade Auditor
                                           ├─ C7  Hub de alertas y conducta
                                           ├─ C8  Hub de reportes
                                           ├─ C9  Capa de integraciones
                                           └─ C10 Portal de transparencia
```

**Ejemplo de por qué hace falta la rotación:** el *cupo operativo* aparece en el preoperatorio (control), en la operatoria (consumo) y en el postoperatorio (error de carga). Son tres relatos del mismo objeto. Si se especifica tres veces, se construye tres veces.

---

## 5. Arquitectura de capacidades

### C1 — Núcleo de datos desagregados
**Tesis de Gabriel De Seta, que es la columna vertebral del módulo entero:**

Cada cliente es una fila, cada dato es una columna, y **el dato crudo desagregado arroja nexos entre filas**: grupo económico, mismos directores, coincidencia de participación accionaria, combos gerente–custodio.

**Qué habilita:** todo lo demás. Es la única capacidad sin la cual ninguna otra funciona.

### C2 — Motor de reglas y parametrización
Separa lo **estable** (el espíritu de la norma) de lo **variable** (el parámetro). Ejemplo trabajado en la reunión:

| Capa | Contenido | Volatilidad |
|---|---|---|
| Regla | Si una sociedad del grupo liquida activo contra dólares, la otra pierde acceso al MULC | Estable — "no quieren mezclar mercados" |
| Parámetro | Umbral de dominancia para considerar grupo económico: ¿10%? ¿20%? | **Cambia** |
| Dato | Composición accionaria y % de participación de cada entidad | Crudo (C1) |

**Requisito no obvio:** la parametrización es **por entidad**. Facimex y PISA tienen criterios distintos sobre las mismas normas. Federico lo dijo del ranking de clientes y Gabriel del cupo. **Multi-criterio desde el diseño, no como parche posterior.**

### C3 — Ingesta documental inteligente
El cliente carga los documentos; el sistema **le hace preguntas al conjunto**. Las preguntas son un set **finito** (10, 15, 100 — no infinitas): beneficiarios finales, vigencia de la sociedad, poderes y sus vencimientos, firma conjunta o indistinta, objeto social, versiones de estatuto, cambios accionarios.

**Lo que cambia respecto de hoy:** hoy es control manual, documento por documento. Y el output no es solo "qué dice el documento" sino **qué pregunta quedó sin responder** — o sea, qué falta.

**Efecto secundario de alto valor:** los datos extraídos quedan registrados como input y habilitan la actualización de legajos con alertas de vencimiento anticipadas. Hoy hay algo armado por Camacho que **no funciona por falta de inputs registrados**. C3 es el prerrequisito que le falta.

### C4 — Onboarding escalonado
**La propuesta central del relevamiento.** Invierte el orden de los factores:

> Hoy: perfilo → clasifico → habilito.
> Propuesta: habilito un nivel mínimo → el cliente opera dentro de ese nivel → cuando quiere más, le pido lo necesario para desbloquear el siguiente.

**Precisión de Federico que le da viabilidad normativa y que hay que repetir textual en la presentación:**

Se puede tener la comitente abierta **sin que esté operativa**. Vara de escalonamiento acordada: **por monto**, no por instrumento.

Incluye también la reducción de lo firmable — separar **qué se pide** (no se toca) de **qué se imprime y se firma** (se reduce) — y la validación automática de firma digital.

### C5 — Screening & Monitoring
**La capacidad más madura.** RePET y Sujeto Obligado terminados, Worldsys en curso. Falta consolidar OFAC, PJN, Nosis y el monitoreo periódico.

**Gap identificado y no resuelto:** el proveedor de listas **no cubre PJN**, que requiere credenciales o matrícula. Mauro Barnatan tiene credencial. Sin decisión sobre la vía.

**Ventaja estructural que conviene destacar (Federico):** todas estas fuentes se consultan **con solo un CUIT**, sin convenio firmado, sobre alguien que ni siquiera es cliente ni prospecto. 

### C6 — Pre-Trade Auditor
Ya existe parcialmente vía Blotter. Implementa el modelo del **cerco invisible**:

```
PRESETEO → ZONA VERDE (sin alertas) → TRANSGRESIÓN (alerta tiempo real)
   → CONTINGENCIA AUTOMÁTICA (pide lo que corresponde) → HUMANO → OVERRIDE registrado
```

**Refinamiento de Federico sobre dónde entra el humano** — es lo que diferencia esta spec de un bloqueo tonto:
> La intervención humana **no** se da ante la contingencia sino **en el paso siguiente**. Si se excede el cupo, pedir documentación es automático porque siempre se pide la misma. El humano entra cuando el balance ya está sobre la mesa: 

Criterio de suficiencia — **las 4 preguntas**: qué puede operar, cuánto, quién y por qué vía. Si están respondidas, el preoperatorio está completo.

Incluye el **cupo operativo dinámico**, con la contrapropuesta de Gabriel: que MatchFin **no devuelva un número sino un panel de control configurable** — acumular vs. netear, por cliente, márgenes del 40/60% sobre activo corriente según riesgo, extras al 100% con caducidad a los 2 meses.

### C7 — Hub de alertas y monitoreo conductual
Segunda y tercera línea de lectura. Más allá del exceso de cupo: KPIs, operatoria inconsistente con el perfil aun con override, y **detección de patrones** — churning, rotación de cartera que solo genera comisión, rulos entre grupos de personas.

**Restricción que impone Gabriel y que condiciona la spec:**  En un FCI rotar cartera es normal; en una empresa no. **Hay que cargar las características del producto** para definir qué comportamiento es esperable.

Incluye generación automática de **ROI → ROS**, con la distinción que Gabriel exige mantener: el ROI es interno; el ROS es normativo, tiene plazos UIF, **ciclo de vida independiente del comitente** y puede existir **sin comitente** (operación tentada de alguien a quien no se le abrió la cuenta).

### C8 — Hub de reportes regulatorios
Coctelera: fuentes → template → validación → reporte que sale **casi certificado para mandar a ciegas**, con supervisión humana en el iterar.

**Dos definiciones que hay que tomar antes de especificar:**
1. **El alcance excede a Compliance.** A la CNV se reporta cada vez más información: contabilidad, mercado primario, operaciones con extranjeros, saldos y movimientos por cuentas extranjeras, pasivo, liquidez. Gabriel: 
2. **El control del control es automático.** Aporte de Gabriel aceptado sin objeción: la magnitud de reportes hace ilógico el control humano; el control debe ser **el mismo proceso a la inversa** sobre otro input. 

### C9 — Capa de integraciones
**El cuello de botella transversal del proyecto.** Aparece como prerrequisito de C3, C6 y C8.

| Integración | Estado |
|---|---|
| MatchFin ↔ Aune | El sistema de gestión ya toma automáticamente casi todos los datos del onboarding. Funciona bien |
| MatchFin ↔ Caja de Valores | **Manual, ~10 min por comitente.** Obligatorio por normativa. Se presume igual en Facimex y PISA. A explorar: API, carga masiva, o legajo homologado |
| Aune ↔ Caja de Valores | Existiría un mecanismo en Aune, **no activado ni probado** en PISA |
| MatchFin ↔ Visual Bolsa | Requisito de Pablo para el cupo: saber, por cada tipo de documento y operación, si suma o resta cupo |
| Fuentes externas | RePET ✅ · Worldsys 🔄 · PJN ⚠️ · Nosis, OFAC pendientes |

### C10 — Portal de transparencia al cliente
La capacidad que **despersonaliza la negativa comercial**. Hoy, cuando hay que negarle algo a un cliente, Compliance le pasa la pelota al comercial, nunca por escrito, sin registro, y 

Cuatro componentes acordados: cláusula de derecho de admisión firmada en la apertura → reglas que hacen el corte automático → reporting accesible al cliente → override siempre disponible, con registro.

Más la **pantalla única** que pidió Federico: que el cliente vea su perfil transaccional, qué tiene permitido, si puede tomar caución y hasta qué cupo. 

**Marco conceptual para la presentación (Gastón):** de  a **. Si el proceso detecta y aplica el rigor, no hay culpa que asumir: nosotros no pusimos al cliente en esa situación.

---

## 6. Roadmap propuesto

Mismo formato que Blotter: fases secuenciales con ID de tarea `Fn-nn`.

```
F1  NÚCLEO DE DATOS Y LEGAJO DIGITAL        C1 + C3
F2  ONBOARDING ESCALONADO                   C4 + C2(p) + firma digital
F3  SCREENING & MONITORING CONSOLIDADO      C5 + C10(p)   ◀ track paralelo
F4  PRE-TRADE AUDITOR Y CUPO DINÁMICO       C6 + C2 + C10
F5  HUB DE REPORTES Y CONDUCTA              C7 + C8        ~
    C9 INTEGRACIONES — transversal, se ataca dentro de cada fase según necesidad
```

**F3 no es estrictamente secuencial.** Es la fase más madura (RePET y Sujeto Obligado ya terminados, Worldsys en curso) y la que produce valor demostrable más rápido. Conviene correrla **en paralelo desde el inicio** como track de resultados tempranos mientras F1 construye los cimientos, que son menos visibles.

**Justificación del orden del resto:**
- **F1 primero** porque C1 es prerrequisito de todo y C3 desbloquea tanto F2 como la actualización de legajos, que hoy está trabada por falta de inputs registrados.
- **F2 antes que F4** porque el escalonamiento define los niveles de habilitación que el Pre-Trade Auditor tiene que hacer cumplir.
- **F5 último** porque su insumo es la operación ya controlada. Reportar sobre datos sucios reproduce el problema.

---

## 7. Bloqueantes: lo que Compliance debe definir antes de que se pueda especificar

Esto es lo más importante para la reunión interna, porque **no es trabajo de desarrollo**. Sin estos inputs, las specs no se pueden escribir.

| # | Definición pendiente | Responsable | Bloquea | Estado |
|---|---|---|---|---|
| **B1** | **La vara y los rangos** del escalonamiento — encontrar el patrón (ej. 2 palos → 20 palos) | Pablo Ruiz | **F2 completa** | Asignado, sin fecha |
| **B2** | **El cuestionario / checklist** de datos a extraer de documentos, por default y a demanda | Pablo + Federico (acordaron armarlo juntos) | **F1-C3** | Asignado. Existe un listado parcial, hay que revisarlo |
| **B3** | **Datos mínimos** necesarios para clasificar riesgo bajo/medio/alto lo antes posible | Sin asignar | **F2** | ⚠️ Abierto — objeción de Mauro sin resolver |
| **B4** | **Criterio de cálculo y consumo del cupo** — acumula, netea, libera al vender | Agustín Córdoba (lo pidió) | **F4** | En definición |
| **B5** | **Inventario completo de reportes** — nombre, modelo, descripción, fuentes | Pablo + Gabriel | **F5** | Asignado. Requiere abrir a sectores no-Compliance |
| **B6** | **Criterio de qué se firma y qué no** — el corte entre las 15 hojas y el extracto | ⚠️ **Sin asignar** | **F2** | Nadie lo tomó en la reunión |
| **B7** | **Características del producto** para definir comportamiento esperable | Sin asignar | **F5-C7** | Levantado por Gabriel, sin dueño |
| **B8** | **Vía de acceso a PJN** — credencial propia, abogados, o proveedor | Sin asignar | **F3** | Mauro tiene credencial |

**B3 y B6 son los dos que hay que resolver primero**, porque si no se define el criterio de corte documental y el set mínimo de datos de riesgo, **F2 entera queda sin especificar** — y F2 es la fase que produce el impacto comercial visible.

---

## 8. Tensiones a explicitar en la presentación

No conviene presentarlo como consenso pleno. Hay cuatro desacuerdos vivos, y ocultarlos hace que reaparezcan en el desarrollo:

| # | Tensión | Impacto en el roadmap |
|---|---|---|
| **T1** | **Escalonar por monto vs. exigir origen de fondos siempre.** La norma permite umbrales; Federico lo considera inaceptable:  | Choca de frente con B1. **Si no se resuelve, F2 no arranca** |
| **T2** | **Reducir el legajo vs. pedir de más.** Federico quiere reducir lo firmable; Pablo defiende el sobre-pedido porque es lo que cubre y lo que permite perfilar | Es B6. Sin dueño |
| **T3** | **Materialidad y significatividad.** La norma no obliga siempre, pero exige evaluar significatividad — que no necesariamente sigue al monto | Condiciona B1 |
| **T4** | **La decisión final sobre un cliente expuesto.** El bloqueo se automatiza, pero Pablo sostiene que queda una decisión **comercial, política y de riesgo de lavado** que ningún proceso resuelve | Define el alcance de C10: el sistema informa y bloquea, **no decide** |

---

## 9. Wishlist del equipo (cierre de la reunión)

Útil como lámina de cierre: muestra que el roadmap contiene lo que cada uno pidió.

| Persona | Pedido | Dónde entra |
|---|---|---|
| Gabriel De Seta | Tener discriminados los datos crudos | **C1 / F1** — es la fase 1 entera |
| Mauro Barnatan | Control de fondeo dentro de MatchFin + acumulación de cupos | **C6 / F4** |
| Pablo Ruiz | Más tipos de alertas e indicadores, no solo exceso de cupo | **C7 / F5** |
| Federico Villegas | Onboarding más liviano en lo firmable + diligencia automática por etapas | **C4 / F2** |
