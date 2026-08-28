# Análisis Quirúrgico de Integración: Módulo Compliance e Infraestructura MatchFin

Este documento analiza cómo se encastra el nuevo **Módulo de Compliance** dentro del ecosistema de módulos ya desarrollados en la plataforma **MatchFin** (Onboarding, Credit Management y Blotter), identificando las brechas técnicas actuales (gaps) y definiendo con precisión qué debe contener la especificación funcional para los desarrolladores.

---

## 1. El Rompecabezas MatchFin: Injerencia de Compliance

El siguiente mapa conceptual ilustra la relación y gobernanza del nuevo módulo de Compliance sobre la infraestructura existente:

```mermaid
graph TD
    subgraph Módulo Onboarding (Existente)
        OB_Form[Formulario y Legajo Digital]
        OB_States[Flujo de Estados: Carga -> Revisión -> Firmada -> Abierta]
        OB_Alerts[Sistema de Avisos Manuales]
    end

    subgraph Módulo Credit Management (Existente)
        CR_Vuelco[Vuelco de Balances y Ventas]
        CR_Quant[Análisis Quant: Cupo de Referencia]
        CR_Rel[Entidades Relacionadas y Nosis]
    end

    subgraph Módulo Blotter & PTL (Existente)
        PTA[Pre-Trade Auditor]
        PTA_Checks[4 Capas: Canal, Perfil, Cupo, Pago]
        PTA_Override[Fila de Excepciones / Handoff a Humano]
    end

    subgraph Módulo Compliance (NUEVO - Núcleo Regulador)
        C1_DB[(C1: Núcleo de Datos Desagregados CUIT)]
        C2_Rules[C2: Motor de Reglas & Parámetros]
        C3_IA[C3: Ingesta con IA / Extracción Checklist]
        C4_Esc[C4: Diligencia Escalonada por Montos]
        C5_Scr[C5: Verificador de Screening Consolidado]
    end

    %% Relaciones
    OB_Form -->|Alimenta| C1_DB
    C3_IA -->|Auto-completa campos| OB_Form
    C4_Esc -->|Modifica reglas de validación y estados| OB_States
    C1_DB -->|Automatiza vencimientos| OB_Alerts

    CR_Vuelco -->|Aporta datos crudos| C1_DB
    CR_Quant -->|Aporta base del cupo| C2_Rules
    C2_Rules -->|Define límites reales en PTL| PTA_Checks
    
    PTA -->|Dispara transgresión y handoff| PTA_Override
    PTA_Override -->|Requiere Override Registrado| C2_Rules
```

---

## 2. Puntos de Impacto en los Módulos Existentes

### A. Módulo Onboarding (Apertura de Cuenta)
*   **Gobernanza del Semáforo de Secciones:** Actualmente, el manual de Onboarding exige que *todas* las secciones aplicables estén en verde (completas) para poder enviar la solicitud (pág. 2). 
    *   **Injerencia de Compliance:** Se debe modificar este comportamiento para admitir el **Onboarding Escalonado (C4)**. Los requisitos de obligatoriedad (campos amarillos/verdes) ahora dependerán dinámicamente del **Nivel de Diligencia (Nivel 1, 2 o 3)** calculado en base al monto proyectado.
*   **Validación de Identidad y Bloqueos Prontos:** La validación automática por ARCA al crear la solicitud (pág. 6) debe interactuar directamente con el **Motor de Screening (C5)** en la Fase 3, de modo que si salta una alerta de fallecido, menor o coincidencia crítica en RePET/Worldsys, el onboarding se asigne automáticamente al estado de *Revisión de Carga* bloqueando el flujo rápido comercial.
*   **Automatización del Sistema de Avisos:** El "Sistema de avisos" (pág. 17) para vencimiento de documentos hoy es manual. Con la implementación del **checklist con IA (C3)**, el sistema extraerá las fechas de vencimiento de poderes (`POD-03`) y de sociedades (`VIG-02`), y automatizará el disparo de avisos 3 meses antes, sin intervención humana.

### B. Módulo Credit Management (Riesgo Financiero)
*   **Vuelco de Balances y Ratios Quant:** El módulo de Credit Management realiza un vuelco automático de balances (pág. 10) y ejecuta un *Análisis Quant* (pág. 16) para estimar un cupo operativo.
    *   **Injerencia de Compliance:** Este cupo quant y los ratios calculados (como Deuda/EBITDA, Liquidez, etc.) serán tomados por el **Motor de Reglas de Compliance (C2)** como "Datos Crudos". El motor aplicará las reglas específicas de la ALyC (ej. netear/acumular, márgenes de seguridad según matriz de riesgo) para devolver el **Cupo Operativo Real** que se inyectará en el Blotter.
*   **Centralización del CUIT:** Se unificará la base de datos de empresas relacionadas y estructura societaria (pág. 13) con la capacidad de nexos de **C1** para poder controlar normativas BCRA de grupos económicos.

### C. Módulo Blotter (Pre-Trade Auditor)
*   **El Pre-Trade Auditor (PTL):** En el flujo del Blotter (ver Mapa Mental), la orden pasa por el Pre-Trade Auditor antes de confirmarse.
    *   **Injerencia de Compliance:** Compliance proveerá el backend lógico para las **4 capas de validación** (Canal, Perfil de Inversor, Cupo y Pago):
        1.  *Canal:* Cruza los canales registrados en Onboarding (pág. 10) con la vía de ingreso de la orden.
        2.  *Perfil:* Cruza el perfil resultante del test automático/manual (pág. 12) con el nivel de riesgo del activo.
        3.  *Cupo/Pago:* Valida el consumo del cupo dinámico calculado por el motor de Compliance.
    *   **Gestión de Excepciones (Override):** Si el PTL arroja `NOK`, el sistema ejecuta la regla R5 (Contingencia auto -> recolección -> analista). En lugar de detener la mesa de operaciones indefinidamente, el comercial puede visualizar la causa de rechazo gracias al **Portal de Transparencia (C10)**, reduciendo la asimetría de información.

---

## 3. Quirúrgicamente: Lo que hay por hacer (Gaps)

Para instrumentar este acople, los desarrolladores deben intervenir la base de datos y la lógica de negocio existentes. Estos son los gaps específicos:

| Componente | Qué existe hoy | Qué falta desarrollar (Brecha) | Injerencia de Specs |
|---|---|---|---|
| **Estructura de Onboarding** | Tabla de obligatoriedad de campos estática por tipo de sujeto (Humana/Jurídica). | Relacionar obligatoriedad a una variable de estado: `nivel_diligencia_id` (1, 2, 3). | Spec **F2-01** / **F2-02** |
| **Flujo de Firmas** | Firma electrónica completa (Signatura) o manuscrita del legajo de 15 hojas. | Mapeo de campos para generar un **Extracto de Legajo** (reducido a 3-5 páginas) que sea legalmente vinculante. | Spec **F2-04** / **F2-05** |
| **Ingesta de Documentos** | Carga de archivos PDF en sección *Documentación*. El analista lee e ingresa fechas manualmente. | Microservicio de lectura OCR/IA que responda el checklist de 10 preguntas societarias (VIG, POD, ACC) y escriba en `C1`. | Spec **F1-06** *(Ejemplo)* |
| **Enlace PTL - Cupos** | Cálculo de *Línea Quant* en Credit Management sin impacto directo en ejecución de operaciones. | Endpoint que comunique el cupo dinámico de Compliance al ruteador del Blotter en milisegundos. | Spec **F4-04** / **F4-05** |
| **Base de Datos de Nexos** | Módulos de ONs, Nosis y Deuda por CUIT aislado. | Modelo de datos jerárquico de nexos societarios (`grupo_economico_id`) para herencia de restricciones cambiantes. | Spec **F1-02** |

---

## 4. Detalles requeridos en las Specs para los Devs

Para que las specs de este módulo sean procesables por el equipo de MatchFin, cada documento (`Fn-nn`) deberá incluir los siguientes detalles de acople:

### 1. Cambios en el Modelo de Datos (Esquema de BD)
No basta con decir "se guarda el nivel". Se debe especificar la migración de las tablas existentes de Onboarding:
*   **Modificación en `onboarding_solicitud`:**
    *   Agregar campo: `nivel_diligencia_id` (FK a nueva tabla `diligencia_nivel`).
    *   Agregar campo: `cupo_operativo_acumulado` (Decimal).
*   **Nueva tabla `diligencia_nivel`:**
    *   `id`, `entidad_id`, `nombre`, `monto_limite`, `requiere_origen_fondos` (Boolean), `campos_obligatorios[]` (JSON con lista de selectores de campos de legajo).

### 2. Mapeo de Lógica del Motor de Reglas (C2)
La spec debe detallar cómo el motor de reglas de Compliance evalúa la orden del Blotter.
*   **Input del pre-trade:** `comitente_id`, `especie`, `monto_operacion`, `canal_ingreso`, `usuario_firmante`.
*   **Pseudocódigo de validación para devs:**
    ```
    // 1. Validar Canal
    SI canal_ingreso NO ESTÁ EN comitente.canales_habilitados ENTONCES retornar NOK("Canal no autorizado")

    // 2. Validar Perfil de Inversor
    SI especie.riesgo > comitente.perfil_inversion.limite_riesgo ENTONCES
       SI comitente.asume_riesgo == FALSO ENTONCES retornar NOK("Instrumento excede perfil de inversor")

    // 3. Validar Cupo
    Decimal cupo_disponible = comitente.cupo_limite - comitente.consumo_actual
    SI monto_operacion > cupo_disponible ENTONCES
       Disparar_Contingencia_Exceso(comitente_id, monto_operacion)
       retornar NOK("Exceso de cupo operativo")
    ```

### 3. Definición del Handoff y Contingencias (R5)
Las specs deben detallar qué API se dispara ante un fallo preoperatorio:
*   **Método:** `POST /api/v1/compliance/contingencias`
*   **Payload:** `{ comitente_id, transgresion_tipo, monto }`
*   **Acción automática:** El sistema busca en `C1` qué documento falta para desbloquear el siguiente nivel, envía un email automático pidiendo ese balance/declaración, y pone la orden en estado `Pendiente Compliance` en la pantalla única de la mesa de operaciones (PTL).
