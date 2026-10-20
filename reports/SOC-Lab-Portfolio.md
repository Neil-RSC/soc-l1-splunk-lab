# Laboratorio SOC L1 con Splunk Enterprise

**Categoría:** Proyecto práctico de ciberseguridad  
**Nivel:** Principiante / Junior  
**Tecnología principal:** Splunk Enterprise  
**Entorno:** Windows, datos sintéticos en formato CSV  
**Caso de uso:** Detección e investigación de posibles credenciales comprometidas

## Objetivo

Diseñar y ejecutar un laboratorio práctico de análisis de eventos de autenticación para identificar patrones sospechosos, investigar intentos de acceso fallidos y documentar un posible incidente de seguridad.

## Trabajo realizado

- Preparación de un entorno local de Splunk Enterprise.
- Importación y validación de 20 eventos sintéticos de autenticación.
- Análisis de 10 intentos fallidos y 10 accesos exitosos mediante búsquedas SPL.
- Identificación de una cuenta administrativa con siete intentos fallidos.
- Investigación de la cronología: cinco fallos antes de un acceso exitoso y dos fallos posteriores.
- Revisión de la IP de origen, el equipo involucrado y los eventos asociados.
- Elaboración de un informe de investigación y recopilación de seis capturas de evidencia.
- Creación de un dashboard de Splunk con cuatro paneles para visualizar los resultados.

## Competencias técnicas practicadas

- Análisis de logs de autenticación.
- Búsquedas y agregaciones con Splunk Processing Language (SPL).
- Identificación y priorización de actividad potencialmente sospechosa.
- Análisis de secuencias temporales de eventos.
- Triaje inicial y documentación de hallazgos.
- Creación de dashboards para apoyar una investigación.

## Resultado

Se completó una investigación simulada de autenticación sospechosa, documentada mediante consultas SPL, evidencias visuales, un informe y un dashboard funcional.

**Limitación:** el laboratorio utiliza datos sintéticos y no demuestra una intrusión real ni experiencia profesional en un SOC de producción.
