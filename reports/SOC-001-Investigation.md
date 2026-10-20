# Informe de investigación SOC-001
## Posible compromiso de credenciales

**Proyecto:** Laboratorio SOC L1 con Splunk Enterprise  
**Caso:** SOC-001 — Posibles credenciales comprometidas  
**Estado:** Investigación inicial documentada  
**Prioridad propuesta:** Alta para investigación  
**Plataforma:** Splunk Enterprise 10.4.3  
**Fecha de elaboración:** 18 de octubre de 2026  
**Analista:** Analista SOC L1 — Proyecto de laboratorio

---

## 1. Resumen ejecutivo

Se analizaron eventos sintéticos de autenticación mediante Splunk Enterprise con el objetivo de identificar patrones compatibles con un posible compromiso de credenciales.

El conjunto de datos contiene 20 eventos: 10 intentos fallidos y 10 accesos exitosos. El análisis identificó que la cuenta `admin` concentra siete intentos fallidos y un acceso exitoso registrado el 10 de octubre de 2026 a las 10:06:00.

La cronología de la cuenta muestra cinco intentos fallidos entre las 10:05:00 y las 10:05:40, seguidos de un acceso exitoso a las 10:06:00. Posteriormente, se registraron otros dos intentos fallidos, a las 10:12:00 y a las 10:21:00.

Los eventos examinados de la cuenta `admin` están asociados a la IP `203.0.113.50` y al equipo `DC01`.

Por tratarse de una cuenta administrativa y existir una secuencia de fallos seguida de un acceso exitoso, se propone una prioridad alta para la investigación. Sin embargo, los registros disponibles no permiten confirmar un compromiso real de credenciales. Los datos utilizados son sintéticos y la IP indicada pertenece a un rango reservado para documentación y ejemplos.

## 2. Objetivo y alcance

El objetivo de la investigación fue practicar las tareas fundamentales de un analista SOC de nivel 1:

- Validar la ingesta y la estructura de los eventos en Splunk.
- Contabilizar los intentos fallidos y los accesos exitosos.
- Identificar las cuentas con mayor número de intentos fallidos.
- Examinar la cronología de una cuenta prioritaria.
- Correlacionar los intentos fallidos con un acceso exitoso.
- Realizar una evaluación inicial de la alerta.
- Documentar los hallazgos, las evidencias y las recomendaciones.

El alcance se limita al archivo sintético `auth_events.csv` cargado en el entorno local de Splunk Enterprise.

No se analizaron sistemas de producción, no se utilizaron credenciales reales y no se ejecutaron acciones de contención ni un escalamiento operativo real.

## 3. Entorno y datos analizados

### 3.1. Entorno

- Plataforma de análisis: Splunk Enterprise 10.4.3.
- Interfaz utilizada: Search & Reporting.
- Archivo de origen: `auth_events.csv`.
- Host de ingesta en Splunk: `FoxCom`.
- Sourcetype: `csv`.
- Datos: eventos sintéticos de autenticación.

### 3.2. Validación de los datos

La búsqueda inicial devolvió 20 eventos.

El recuento mediante SPL confirmó:

| Tipo de evento | Cantidad |
|---|---:|
| Intentos fallidos (`failure`) | 10 |
| Accesos exitosos (`success`) | 10 |
| **Total** | **20** |

Durante la validación también se comprobó que los campos necesarios para la investigación estaban disponibles, incluidos `_time`, `event_time`, `user`, `src_ip`, `action`, `host`, `extracted_host` y `event_type`.

### 3.3. Consideraciones sobre las direcciones IP

La IP `203.0.113.50` pertenece a un rango reservado para documentación y ejemplos. Su presencia es coherente con el carácter sintético del laboratorio.

Las direcciones IP privadas observadas en otros eventos también forman parte del conjunto de datos de práctica. No deben interpretarse como indicadores de actividad maliciosa real sin contexto adicional.

## 4. Hallazgos de la investigación

### 4.1. Distribución de intentos fallidos por usuario

La búsqueda de fallos agrupados por cuenta produjo los siguientes resultados:

| Usuario | Intentos fallidos | IP de origen | Equipo asociado |
|---|---:|---|---|
| `admin` | 7 | `203.0.113.50` | `DC01` |
| `backup` | 2 | `192.168.1.10` | `FILE01` |
| `maria` | 1 | `192.168.1.20` | `WS-01` |
| **Total** | **10** | | |

La cuenta `admin` concentra el 70 % de los intentos fallidos del conjunto analizado. Por su carácter administrativo y por el patrón temporal observado, se seleccionó como cuenta prioritaria para continuar la investigación.

Los fallos de las cuentas `backup` y `maria` se registran como hallazgos secundarios. Por sí solos, no permiten concluir que exista actividad maliciosa.

### 4.2. Cronología de la cuenta admin

La búsqueda de todos los eventos asociados a `admin` devolvió ocho registros:

| Fecha y hora | Acción | IP de origen | Equipo |
|---|---|---|---|
| 10/10/2026 10:05:00 | `failure` | `203.0.113.50` | `DC01` |
| 10/10/2026 10:05:10 | `failure` | `203.0.113.50` | `DC01` |
| 10/10/2026 10:05:20 | `failure` | `203.0.113.50` | `DC01` |
| 10/10/2026 10:05:30 | `failure` | `203.0.113.50` | `DC01` |
| 10/10/2026 10:05:40 | `failure` | `203.0.113.50` | `DC01` |
| 10/10/2026 10:06:00 | `success` | `203.0.113.50` | `DC01` |
| 10/10/2026 10:12:00 | `failure` | `203.0.113.50` | `DC01` |
| 10/10/2026 10:21:00 | `failure` | `203.0.113.50` | `DC01` |

La secuencia muestra cinco fallos consecutivos antes del acceso exitoso, con un intervalo de un minuto entre el primer fallo y el éxito. Se registraron otros dos fallos después de ese acceso.

### 4.3. Resumen de accesos exitosos de admin

La consulta de resumen de accesos exitosos devolvió una fila con los siguientes valores:

- Accesos exitosos: 1.
- Primer acceso exitoso: 10 de octubre de 2026, 10:06:00.
- Último acceso exitoso: 10 de octubre de 2026, 10:06:00.
- IP de origen: `203.0.113.50`.
- Equipo asociado: `DC01`.

Las horas del primer y último acceso coinciden porque solo se encontró un evento exitoso de `admin` en los datos cargados y con los filtros aplicados.

### 4.4. Interpretación técnica

La combinación de varios fallos de autenticación seguidos de un acceso exitoso constituye un patrón que merece investigación. Entre las posibles explicaciones se encuentran errores de autenticación legítimos, intentos repetidos de acceso o un posible uso no autorizado de credenciales.

Los registros analizados no permiten determinar cuál de estas explicaciones es correcta. Por tanto, el hallazgo se clasifica como sospechoso, con compromiso no confirmado.

## 5. Consultas SPL utilizadas

### 5.1. Recuento de fallos y accesos exitosos

```spl
source="auth_events.csv" host="FoxCom" sourcetype="csv"
| stats count(eval(action="failure")) AS fallos
        count(eval(action="success")) AS exitosos
```

**Objetivo:** comprobar la cantidad de eventos fallidos y exitosos presentes en los datos cargados.

**Resultado observado:** 10 fallos y 10 accesos exitosos.

### 5.2. Fallos agrupados por usuario

```spl
source="auth_events.csv" host="FoxCom" sourcetype="csv" action="failure"
| stats count AS intentos_fallidos
        values(src_ip) AS ips_origen
        values(extracted_host) AS equipos
        by user
| sort - intentos_fallidos
```

**Objetivo:** identificar las cuentas con mayor cantidad de intentos fallidos y mostrar las IP y los equipos asociados.

**Resultado observado:** `admin` registró siete fallos, `backup` dos y `maria` uno.

### 5.3. Cronología de admin

```spl
source="auth_events.csv" host="FoxCom" sourcetype="csv" user="admin"
| table _time user action src_ip extracted_host event_type
| sort _time
```

**Objetivo:** examinar los eventos de la cuenta `admin` en orden cronológico.

**Resultado observado:** cinco fallos antes de un acceso exitoso y dos fallos posteriores.

### 5.4. Resumen de accesos exitosos de admin

```spl
source="auth_events.csv" host="FoxCom" sourcetype="csv" user="admin" action="success"
| stats count AS accesos_exitosos
        earliest(_time) AS primer_acceso
        latest(_time) AS ultimo_acceso
        values(src_ip) AS ips_origen
        values(extracted_host) AS equipos
| convert ctime(primer_acceso) ctime(ultimo_acceso)
```

**Objetivo:** contar los accesos exitosos de la cuenta y obtener las horas del primero y del último, junto con las IP y los equipos asociados.

**Resultado observado:** un acceso exitoso a las 10:06:00, desde `203.0.113.50`, asociado a `DC01`.

### 5.5. Recuento acumulado de fallos antes del acceso exitoso

```spl
source="auth_events.csv" host="FoxCom" sourcetype="csv" user="admin"
| eval orden=if(action="failure",1,2)
| sort 0 _time
| streamstats count(eval(action="failure")) AS fallos_acumulados
| where action="success"
| table _time user src_ip extracted_host fallos_acumulados
```

**Objetivo:** ordenar los eventos de `admin`, contar los fallos acumulados y mostrar el valor correspondiente al evento exitoso.

**Resultado observado:** `fallos_acumulados = 5` en el acceso exitoso de las 10:06:00.

## 6. Triaje inicial de la alerta

### 6.1. Ficha de evaluación

| Campo | Evaluación |
|---|---|
| Identificador | SOC-001 |
| Cuenta prioritaria | `admin` |
| Equipo asociado | `DC01` |
| IP observada | `203.0.113.50` |
| Fallos de la cuenta | 7 |
| Accesos exitosos de la cuenta | 1 |
| Patrón observado | Cinco fallos, un éxito y dos fallos posteriores |
| Severidad propuesta | Alta para investigación |
| Estado del compromiso | No confirmado |

### 6.2. Justificación de la prioridad

Se propone una prioridad alta para la investigación porque el patrón afecta a una cuenta administrativa y contiene un acceso exitoso precedido de varios fallos.

Esta valoración representa una decisión inicial de triaje, no una severidad universal. En un entorno real, la clasificación definitiva dependería de los criterios internos de la organización, la criticidad del activo, el contexto de autenticación y las evidencias adicionales.

La confianza en que haya ocurrido un compromiso real continúa siendo limitada porque solo se dispone de eventos sintéticos y no se ha validado la legitimidad del acceso.

## 7. Preguntas de investigación y respuestas del analista SOC L1

### 7.1. ¿Qué usuario fue atacado?

La cuenta prioritaria para la investigación es `admin`, asociada al equipo `DC01`.

Los datos muestran siete intentos fallidos y un acceso exitoso. La secuencia es sospechosa, pero no permite confirmar por sí sola que la cuenta haya sido atacada o comprometida.

### 7.2. ¿Desde qué dirección IP se originaron los intentos?

Los eventos examinados de la cuenta `admin`, tanto fallidos como exitosos, muestran la IP `203.0.113.50`.

Esta dirección pertenece a un rango reservado para documentación y ejemplos. Su presencia corresponde al contexto sintético del laboratorio y no identifica a un atacante real.

### 7.3. ¿Cuántos intentos fallidos ocurrieron?

Se registraron siete intentos fallidos de autenticación asociados a `admin` dentro del conjunto analizado.

Cinco se produjeron entre las 10:05:00 y las 10:05:40, antes del acceso exitoso de las 10:06:00. Los otros dos ocurrieron a las 10:12:00 y a las 10:21:00.

### 7.4. ¿Hubo un inicio de sesión exitoso posterior?

Sí. Se identificó un acceso exitoso de `admin` el 10 de octubre de 2026 a las 10:06:00, desde la IP `203.0.113.50` y asociado al equipo `DC01`.

El acceso se produjo después de cinco fallos consecutivos. Este patrón justifica una revisión adicional, pero no demuestra que se hayan utilizado credenciales robadas.

### 7.5. ¿Qué nivel de severidad asignarías?

Se propone una severidad alta para la investigación inicial, debido al carácter administrativo de la cuenta y a la secuencia de fallos seguida de un acceso exitoso.

La valoración es provisional y deberá revisarse cuando se disponga de más contexto. No debe interpretarse como una confirmación de compromiso ni sustituye la matriz de severidad que pudiera utilizar una organización real.

### 7.6. ¿Qué evidencias registrarías y cuándo escalarías el caso?

**Evidencias que deben registrarse:**

- El recuento de los 20 eventos: diez fallos y diez accesos exitosos.
- La distribución de los fallos por usuario.
- La cronología completa de los ocho eventos asociados a `admin`.
- El acceso exitoso de las 10:06:00 y los cinco fallos anteriores.
- La IP, el equipo y los campos relevantes de los eventos.
- Las consultas SPL utilizadas y sus resultados.
- Capturas de pantalla de las búsquedas, con su contexto y fecha de obtención.
- La conclusión provisional, las limitaciones de los datos y las acciones recomendadas.

**Criterios de escalamiento:**

En un entorno real, escalaría el caso al analista SOC L2 o al responsable de respuesta a incidentes si no se puede justificar el acceso exitoso, si se identifica uso no autorizado de la cuenta, si aparecen cambios de privilegios o acciones administrativas sospechosas, o si las políticas internas exigen escalar este tipo de alerta.

Mientras se verifica el contexto, conservaría las evidencias y registraría las acciones realizadas. Las medidas de contención deberían seguir los procedimientos autorizados de la organización.

En este laboratorio no se realizó un escalamiento operativo ni se ejecutaron acciones de contención, porque el escenario se basa en datos sintéticos.

## 8. Recomendaciones

1. Verificar la legitimidad del acceso exitoso de `admin`.
2. Confirmar si el origen y el equipo corresponden al contexto esperado.
3. Revisar otros eventos disponibles para identificar actividad posterior al acceso.
4. Comprobar si existen cambios de privilegios u otras acciones administrativas relacionadas.
5. Documentar cualquier evidencia adicional y actualizar la valoración de severidad.
6. Escalar el caso según el procedimiento correspondiente si la actividad no puede justificarse.
7. En un entorno real, seguir los procedimientos autorizados antes de bloquear cuentas, restablecer credenciales o aplicar otras medidas de contención.

Estas acciones son recomendaciones propuestas. No se ejecutaron en sistemas reales ni forman parte de una respuesta operativa realizada.

## 9. Limitaciones

- Los eventos analizados son sintéticos y se utilizan con fines educativos.
- La IP `203.0.113.50` está reservada para documentación y ejemplos.
- Los registros disponibles no permiten confirmar la identidad de un posible atacante ni demostrar un compromiso real.
- La investigación se limita a los eventos importados en Splunk.
- No se dispone de contexto externo que permita validar la legitimidad del acceso.
- No se realizaron acciones de contención ni un escalamiento real.

## 10. Conclusión

El laboratorio permitió practicar la validación de eventos, el análisis de autenticación con SPL, la identificación de cuentas con mayor número de fallos, la correlación temporal y el triaje inicial de una alerta.

El hallazgo principal es la secuencia de cinco fallos de autenticación de `admin` antes de un acceso exitoso a las 10:06:00, seguida de dos fallos adicionales. En total, la cuenta presenta siete fallos y un éxito en los eventos examinados.

Se recomienda una investigación prioritaria debido al carácter administrativo de la cuenta. Sin embargo, el compromiso permanece sin confirmar y cualquier conclusión definitiva requeriría más contexto y evidencias.

### Dashboard de investigación

Se creó un dashboard en Splunk denominado **SOC - Investigación de autenticaciones**, compuesto por cuatro paneles: comparación de autenticaciones fallidas y exitosas, fallos por usuario, cronología de autenticación de admin y fallos acumulados antes del acceso exitoso.

El dashboard facilita la revisión visual de los eventos y complementa las búsquedas SPL y las evidencias documentadas. Los resultados corresponden a datos sintéticos utilizados con fines formativos. El patrón observado justifica una investigación prioritaria, pero no demuestra por sí solo que se haya producido una intrusión real.

## 11. Evidencias pendientes de adjuntar

- [x] `01_validacion_eventos.png` — validación de los 20 eventos.
- [x] `02_fallos_por_usuario.png` — distribución de los intentos fallidos por usuario.
- [x] `03_cronologia_admin.png` — cronología de autenticación de admin.
- [x] `04_accesos_exitosos_admin.png` — resumen del acceso exitoso de admin.
- [x] `05_fallos_antes_del_exito.png` — cinco fallos previos al acceso exitoso.
- [x] `06_dashboard_soc.png` — dashboard SOC con los cuatro paneles de investigación.

## 12. Nota de alcance profesional

Este documento corresponde a un proyecto educativo de laboratorio SOC L1 realizado con Splunk Enterprise y eventos sintéticos.

Demuestra práctica con búsquedas SPL, validación de datos, análisis de autenticación, documentación de evidencias y triaje inicial. No representa experiencia de respuesta a incidentes en producción ni una investigación de un compromiso real.
