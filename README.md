# Laboratorio SOC L1 con Splunk Enterprise

Laboratorio práctico de nivel principiante orientado al análisis de logs de autenticación, la detección de actividad sospechosa y la documentación de una investigación simulada.

## Objetivo del proyecto

Simular un caso de posibles credenciales comprometidas mediante eventos sintéticos de autenticación, utilizando Splunk Enterprise para investigar intentos fallidos seguidos de un acceso exitoso a una cuenta administrativa.

## Escenario de investigación

Se analizaron 20 eventos sintéticos de autenticación:

- 10 intentos de autenticación fallidos.
- 10 accesos exitosos.
- 7 intentos fallidos asociados a la cuenta `admin`.
- 5 fallos antes de un acceso exitoso de `admin`.
- 2 fallos adicionales posteriores al acceso exitoso.

El patrón se consideró sospechoso y justificaba una investigación prioritaria. Debido a que los datos son sintéticos, no constituye evidencia de una intrusión real.

## Herramientas y tecnologías

- **Splunk Enterprise:** búsqueda y análisis de eventos.
- **SPL (Search Processing Language):** filtrado, agregación, ordenación y análisis temporal.
- **CSV:** formato de los eventos sintéticos.
- **Markdown:** documentación técnica e informe de investigación.
- **Dashboard de Splunk:** visualización de resultados.

## Trabajo realizado

1. Preparación de un entorno local de laboratorio.
2. Importación y validación de eventos sintéticos.
3. Construcción de búsquedas SPL para analizar autenticaciones.
4. Identificación de usuarios con mayor número de fallos.
5. Reconstrucción de la cronología de la cuenta administrativa.
6. Análisis del número de fallos anteriores al acceso exitoso.
7. Documentación de hallazgos, limitaciones y recomendaciones.
8. Creación de un dashboard con cuatro paneles.

## Dashboard

El dashboard reúne las siguientes visualizaciones:

1. Autenticaciones fallidas frente a exitosas.
2. Intentos fallidos por usuario.
3. Cronología de autenticación de `admin`.
4. Fallos acumulados antes del acceso exitoso.

## Estructura del proyecto

```text
SOC-Lab/
├── data/
│   └── auth_events.csv
├── queries/
│   └── investigation_queries.spl
├── evidence/
│   └── screenshots/
├── reports/
│   ├── SOC-001-Investigation.md
│   ├── SOC-Lab-Portfolio.md
│   └── SOC-Lab-CV.md
└── README.md
```

Esta estructura describe la organización prevista del repositorio. Antes de publicarlo, se deben comprobar los archivos existentes y retirar cualquier archivo local que no se quiera compartir.

## Competencias practicadas

- Análisis de logs de autenticación.
- Búsquedas SPL y agregación de eventos.
- Detección inicial de patrones potencialmente sospechosos.
- Análisis de secuencias temporales.
- Triaje inicial y evaluación preliminar de riesgo.
- Creación de dashboards.
- Documentación técnica de una investigación simulada.

## Limitaciones

- Los eventos utilizados son sintéticos y tienen fines formativos.
- El entorno es local y no representa una infraestructura SOC de producción.
- No se ha confirmado una intrusión real.
- No se han ejecutado acciones reales de contención ni respuesta a incidentes.

## Consideraciones de seguridad y privacidad

Este proyecto está diseñado para utilizar datos sintéticos.

No se incluyen contraseñas, tokens ni credenciales de Splunk.

## Resultado

Se completó un laboratorio introductorio de investigación de autenticaciones con Splunk Enterprise, desde la carga de datos sintéticos hasta la creación de búsquedas SPL, un informe técnico y un dashboard de cuatro paneles.

**Tipo de proyecto:** laboratorio educativo de ciberseguridad SOC L1.