# AS_U3_EXAMEN_PRACTICO

# INFORME FINAL DE AUDITORÍA DE SISTEMAS

## CARÁTULA

![image](https://github.com/user-attachments/assets/7e0801d9-e7e2-4fc4-b6af-1abaaf94c956)


**Entidad Auditada:** [Universidad privada de tacna]  
**Ubicación:** [Tacna]  
**Período auditado:** [27/06/2025]  
**Equipo Auditor:** [Salinas Condori Erick Javier]  
**Fecha del informe:** [27/06/2025]  

Enlace repositorio: https://github.com/Salinas-Condori-Erick/AS_U3_EXAMEN_PRACTICO 

## ÍNDICE

1. [Resumen Ejecutivo](#1-resumen-ejecutivo)  
2. [Antecedentes](#2-antecedentes)  
3. [Objetivos de la Auditoría](#3-objetivos-de-la-auditoría)  
4. [Alcance de la Auditoría](#4-alcance-de-la-auditoría)  
5. [Normativa y Criterios de Evaluación](#5-normativa-y-criterios-de-evaluación)  
6. [Metodología y Enfoque](#6-metodología-y-enfoque)  
7. [Hallazgos y Observaciones](#7-hallazgos-y-observaciones)  
8. [Análisis de Riesgos](#8-análisis-de-riesgos)  
9. [Recomendaciones](#9-recomendaciones)  
10. [Conclusiones](#10-conclusiones)  
11. [Plan de Acción y Seguimiento](#11-plan-de-acción-y-seguimiento)  
12. [Anexos](#12-anexos)  



## 1. RESUMEN EJECUTIVO

El presente informe recoge los resultados de la auditoría al proyecto de despliegue de la plataforma WordPress sobre máquinas virtuales gestionadas con Vagrant y provisionadas mediante Chef. El objetivo principal ha sido evaluar la seguridad, disponibilidad y eficiencia operativa de la infraestructura creada, así como el cumplimiento de buenas prácticas en gestión de configuraciones y control de cambios.
Durante el proceso, se identificaron tres hallazgos críticos relacionados con la correcta parametrización de redes privadas (variables de entorno no cargadas) y el uso de plugins desactualizados, que podrían comprometer la reproducibilidad y la estabilidad del entorno. Asimismo, se detectaron oportunidades de mejora en el endurecimiento de los roles de usuario y en la aplicación de parches automatizados.
Como conclusión, la arquitectura base cumple con los requerimientos funcionales, pero requiere ajustes en el proceso de provisión (actualizar o eliminar dependencias rotas) y fortalecer las políticas de acceso y gestión de secretos. Se recomiendan:

Eliminar la dependencia del plugin vagrant-env y codificar valores críticos en el Vagrantfile o migrar a un gestor de secretos seguro.

Implementar escaneos periódicos de vulnerabilidades (por ejemplo con Trivy) en las VMs antes y después de provisiones.

Definir un calendario de actualizaciones automáticas para ChefDK y componentes de base (Ubuntu, MySQL, Apache, Nginx).



## 2. ANTECEDENTES

La auditoría se realizó sobre el proyecto “Despliegue de WordPress usando Vagrant y Chef


## 3. OBJETIVOS DE LA AUDITORÍA

Evaluar la seguridad, la estabilidad y la conformidad del proceso de despliegue automatizado de la plataforma WordPress sobre Vagrant y Chef, garantizando que la infraestructura cumpla con estándares de mejores prácticas en gestión de configuración, control de acceso e integridad de datos.

Objetivos específicos

Seguridad de la información: Verificar que no existan credenciales embebidas o dependencias vulnerables en los cookbooks y Vagrantfile.

Continuidad del negocio: Comprobar que el proceso de reprovisión de las VMs sea rápido, reproducible y permita restaurar el servicio ante fallos.

Gestión de cambios y configuración: Evaluar la coherencia entre el código de Chef y la infraestructura resultante, revisando la idempotencia de las recetas y la trazabilidad de versiones.

Cumplimiento normativo: Contrastar la solución con estándares de seguridad como OWASP y recomendaciones de hardening para Ubuntu y servidores web.

Integridad y disponibilidad de datos: Asegurar que el servicio de base de datos MySQL se configura con respaldos automáticos y mecanismos de recuperación ante fallos.


## 4. ALCANCE DE LA AUDITORÍA

# Ámbitos evaluados

Tecnológico: Infraestructura de VMs, redes privadas, provisión con Chef, Vagrantfile.

Organizacional: Procesos de despliegue, control de versiones, roles y permisos de usuarios.

Normativo: Buenas prácticas de hardening y cumplimiento de requisitos básicos de seguridad web.

# Sistemas y procesos incluidos

Máquinas virtuales database, wordpress y proxy.

Repositorio de Git con Vagrantfile y cookbooks.

Provisión de software (MySQL, Apache, WordPress, Nginx) y configuración de redes.

# Unidades o áreas auditadas

Equipo de TI / DevOps responsable del desarrollo y despliegue.

Coordinación con el área de infraestructura de la entidad.

# Periodo auditado

27/06/2025 (inclusión de las últimas actualizaciones de la plataforma y recetas Chef).


## 5. NORMATIVA Y CRITERIOS DE EVALUACIÓN

Lista de normas, marcos de referencia o políticas aplicadas como base para la auditoría. Ejemplos:

- COBIT 2019  
- ISO/IEC 27001:2022  
- Ley de Protección de Datos Personales [local]  
- Políticas internas de TI de la entidad


## 6. METODOLOGÍA Y ENFOQUE

La auditoría se llevó a cabo siguiendo una metodología basada en estándares reconocidos de auditoría de sistemas, combinando técnicas de análisis técnico, revisión documental y verificación práctica. El proceso se dividió en las siguientes fases:

1. **Planificación**  
   - Revisión del alcance, objetivos y recursos disponibles.  
   - Identificación de las máquinas virtuales y componentes involucrados en el despliegue.

2. **Recolección de información**  
   - Análisis del `Vagrantfile`, cookbooks Chef, archivos de configuración (`.env`) y scripts de provisión.  
   - Inspección de las máquinas virtuales desplegadas, sus servicios y configuraciones.

3. **Evaluación técnica**  
   - Verificación del cumplimiento de buenas prácticas en provisión de infraestructura.  
   - Pruebas manuales y automáticas de conectividad, servicios, seguridad básica y funcionamiento del entorno.  
   - Validación de los roles, permisos y configuraciones de red.

4. **Identificación de hallazgos y análisis de riesgos**  
   - Categorización de hallazgos según su impacto (crítico, alto, medio, bajo).  
   - Análisis de las causas y posibles consecuencias de cada hallazgo.

5. **Informe y recomendaciones**  
   - Documentación de los resultados obtenidos.  
   - Propuesta de mejoras concretas en configuración, seguridad y automatización.

El enfoque de la auditoría fue **preventivo, técnico y correctivo**, orientado a identificar vulnerabilidades potenciales en un entorno de desarrollo y pruebas, antes de su implementación en entornos de producción.

Se aplicó un enfoque **basado en riesgos**, priorizando los aspectos que pudieran comprometer la:

- **Disponibilidad** del entorno de WordPress.
- **Seguridad** de las máquinas virtuales, servicios web y base de datos.
- **Fiabilidad** de los procesos de aprovisionamiento con Chef.
- **Gestión adecuada de configuraciones y secretos** (como usuarios, contraseñas e IPs).

Asimismo, se utilizó un enfoque **estructurado y sistemático**, siguiendo principios de:

- **Idempotencia** en la automatización con Chef.
- **Reproducibilidad** del entorno con Vagrant.
- **Modularidad y mantenimiento** en el uso de cookbooks y scripts.

## 7. HALLAZGOS Y OBSERVACIONES

Los hallazgos realizados son los siguientes:

Durante el proceso de auditoría técnica y funcional, se identificaron diversos hallazgos relacionados con la configuración del entorno, la automatización del despliegue y la seguridad de la infraestructura. A continuación se detallan los principales:

# Evidencias de despliegue:

![image](https://github.com/user-attachments/assets/79a66994-8960-43a4-9dd5-2390bbb13e7d)

![image](https://github.com/user-attachments/assets/6e255710-5ec9-4ad5-bc32-62ec736a1f98)

![image](https://github.com/user-attachments/assets/5bbfc436-cde4-4483-b7d0-b168e18115f9)

![image](https://github.com/user-attachments/assets/ba4e95bb-e49b-4730-ab55-e3c8027a1e11)

Se ejecutará una VM usando Vagrant y ejecutará las pruebas unitarias.

![image](https://github.com/user-attachments/assets/b308094b-51e5-4b6d-b053-278682e99965)

Pruebas unitarias usando docker

![image](https://github.com/user-attachments/assets/81d2ad9f-688b-4e12-be00-e8478fb9208e)

Pruebas de integración e infraestructura

![image](https://github.com/user-attachments/assets/2f377246-e4af-43c9-8129-997e8962ee5d)


## 8. ANÁLISIS DE RIESGOS

Evaluación del impacto y probabilidad de los riesgos identificados, asociados a los hallazgos encontrados.

| Hallazgo | Riesgo asociado                                                            | Impacto | Probabilidad | Nivel de Riesgo |
|----------|-----------------------------------------------------------------------------|---------|--------------|-----------------|
| 1        | Falla en la carga de variables de entorno impide levantar las VMs          | Alto    | Alta         | Alto            |
| 2        | IPs no válidas pueden provocar errores de red en Vagrant                   | Medio   | Alta         | Alto            |
| 3        | Hostnames duplicados pueden causar conflictos en la red interna            | Medio   | Media        | Medio           |
| 6        | Credenciales visibles en texto plano representan un riesgo de filtración  | Alto    | Media        | Alto            |
| 7        | Ausencia de logs post-provisión dificulta trazabilidad y auditoría         | Medio   | Media        | Medio           |



## 9. RECOMENDACIONES

Propuestas técnicas y organizativas para mitigar los riesgos y subsanar los hallazgos:

- **Hallazgo 1:** Eliminar o comentar la línea `config.env.enable` y definir las variables directamente en el `Vagrantfile` o usar `.envrc` con `direnv`.
- **Hallazgo 2:** Asignar manualmente las IPs privadas en el `Vagrantfile` para evitar errores si no se cargan las variables.
- **Hallazgo 3:** Cambiar el `hostname` de la VM proxy a `proxy.epnewman.edu.pe` para evitar duplicidad.
- **Hallazgo 6:** Reemplazar las credenciales visibles por variables de entorno seguras o usarlas desde un archivo cifrado.
- **Hallazgo 7:** Implementar registros automáticos de ejecución (`logs/setup.log`) tras cada provisión para verificar el estado y facilitar la trazabilidad.


## 10. CONCLUSIONES

La auditoría realizada al entorno de despliegue de WordPress con Vagrant y Chef permitió identificar configuraciones incorrectas, errores en la carga de variables y riesgos de seguridad relacionados con credenciales expuestas.  
A pesar de estos hallazgos, la arquitectura general del sistema está bien estructurada y es funcional una vez corregidas las configuraciones críticas.  
Se concluye que el entorno auditado es **viable y operativo**, pero requiere ajustes puntuales en su automatización y seguridad para alcanzar niveles aceptables de robustez y control.

## 11. PLAN DE ACCIÓN Y SEGUIMIENTO

Propuesta de plan de acción acordado con la entidad auditada:

| Hallazgo | Recomendación                                                                 | Responsable       | Fecha Comprometida |
|----------|--------------------------------------------------------------------------------|-------------------|---------------------|
| 1        | Comentar línea `config.env.enable` y definir las variables directamente        | Equipo DevOps     | 01/07/2025          |
| 2        | Asignar manualmente las IPs privadas en el `Vagrantfile`                       | Responsable Infra | 02/07/2025          |
| 3        | Modificar el hostname de la VM proxy a `proxy.epnewman.edu.pe`                | Administrador VM  | 02/07/2025          |
| 6        | Reemplazar credenciales visibles por variables seguras o archivo cifrado      | Seguridad TI      | 03/07/2025          |
| 7        | Implementar generación automática de logs de provisión en cada VM              | Equipo DevOps     | 04/07/2025          |


## 12. ANEXOS

Incluir documentos de respaldo como:

- Capturas de pantalla  del anexo Evidencias 

![image](https://github.com/user-attachments/assets/ac078fa1-14d3-4dac-80a0-6a86fd6f0950)

![image](https://github.com/user-attachments/assets/d7ea44ee-8001-4369-8d80-11deaedc98b7)

![image](https://github.com/user-attachments/assets/0e8fa463-9f27-4c7c-9f85-7d3ce2d0d8a8)

![image](https://github.com/user-attachments/assets/e31452ab-1c45-4316-84f6-0d84f794ac99)

![image](https://github.com/user-attachments/assets/89af5d68-e3e7-47fb-ae49-28f6f4e1c09f)

![image](https://github.com/user-attachments/assets/49e4c702-e8ba-452d-b908-bbc54b2a60d3)

![image](https://github.com/user-attachments/assets/34e9337d-51df-450a-93ae-93946ec7595d)



