# Phoenix Rules - Sistema de Reglas para Desarrollo con Claude AI

## Descripción General

Este proyecto contiene un sistema completo de reglas y plantillas para desarrollo de aplicaciones Phoenix con Elixir, diseñado específicamente para trabajar con Claude AI. El sistema `.claude` proporciona una estructura organizativa que permite a Claude crear, planificar y ejecutar proyectos de manera consistente y eficiente.

## Arquitectura del Sistema

### Estructura de Directorios

```
.claude/
├── agents/                    # Agentes especializados
│   ├── context-fetcher.md     # Recuperación inteligente de contexto
│   ├── file-creator.md        # Creación de archivos y plantillas
│   ├── git-workflow.md        # Gestión de workflows Git
│   └── test-runner.md         # Ejecución de pruebas
├── commands/                  # Comandos principales
│   ├── analyze-product.md     # Análisis de productos existentes
│   ├── create-spec.md         # Creación de especificaciones
│   ├── execute-tasks.md       # Ejecución de tareas
│   └── plan-product.md        # Planificación de productos
├── instructions/              # Instrucciones detalladas
│   ├── core/                  # Instrucciones principales
│   │   ├── analyze-product.md
│   │   ├── create-spec.md
│   │   ├── execute-task.md
│   │   ├── execute-tasks.md
│   │   └── plan-product.md
│   └── meta/                  # Meta-instrucciones
│       └── pre-flight.md      # Verificaciones previas
├── phoenix_guide/             # Guías específicas de Phoenix
│   ├── controllers.md         # Patrones para controladores
│   ├── security.md           # Prácticas de seguridad
│   ├── data_modelling/       # Modelado de datos
│   │   ├── contexts.md       # Manejo de contextos
│   │   └── ...
│   ├── deployment/           # Despliegue
│   └── howto/               # Guías específicas
└── standards/                # Estándares y buenas prácticas
    ├── best-practices.md     # Mejores prácticas generales
    ├── code-style.md         # Guía de estilo de código
    ├── tech-stack.md         # Stack tecnológico por defecto
    └── code-style/           # Estilos específicos
        ├── css-style.md      # Estilos CSS y daisyUI
        ├── html-style.md     # Estilos HTML
        └── javascript-style.md
```

## Componentes Principales

### 1. Sistema de Agentes Especializados

El sistema utiliza agentes especializados que trabajan en conjunto:

#### Context Fetcher Agent
- **Propósito**: Recuperación inteligente de documentación y contexto
- **Capacidades**:
  - Verifica si la información ya está en contexto
  - Extrae secciones específicas de archivos
  - Utiliza búsquedas grep para encontrar información relevante
  - Evita duplicación de contenido

#### File Creator Agent
- **Propósito**: Creación de archivos y directorios con plantillas consistentes
- **Capacidades**:
  - Manejo de plantillas para diferentes tipos de archivo
  - Creación de estructuras de directorios
  - Aplicación de convenciones de nomenclatura
  - Operaciones batch para múltiples archivos

#### Git Workflow Agent
- **Propósito**: Gestión completa de workflows Git
- **Capacidades**:
  - Manejo de ramas con convenciones de nomenclatura
  - Creación de commits descriptivos
  - Gestión de pull requests
  - Verificación de estado antes de operaciones

#### Test Runner Agent
- **Propósito**: Ejecución y verificación de suites de pruebas
- **Capacidades**:
  - Ejecución de pruebas completas
  - Detección y reporte de fallos
  - Integración con workflows de desarrollo

### 2. Comandos de Alto Nivel

#### Plan Product
Genera documentación completa para nuevos productos:
- **Archivos generados**:
  - `mission.md`: Visión y propósito del producto
  - `mission-lite.md`: Versión condensada para IA
  - `tech-stack.md`: Arquitectura técnica
  - `roadmap.md`: Fases de desarrollo
  - `decisions.md`: Log de decisiones

#### Create Spec
Crea especificaciones detalladas para características:
- **Proceso de 8 pasos**:
  1. Iniciación de especificación
  2. Recopilación de contexto
  3. Clarificación de requisitos
  4. Determinación de fecha
  5. Creación de carpeta de especificación
  6. Generación de `spec.md`
  7. Creación de `spec-lite.md`
  8. Especificación técnica

#### Execute Tasks
Sistema de ejecución de tareas con flujo completo:
- **Flujo de 10 pasos**:
  1. Asignación de tareas
  2. Análisis de contexto
  3. Verificación de servidor de desarrollo
  4. Gestión de ramas Git
  5. Bucle de ejecución de tareas
  6. Verificación de suite de pruebas
  7. Workflow Git completo
  8. Verificación de progreso del roadmap
  9. Notificación de finalización
  10. Resumen de completitud

### 3. Estándares y Buenas Prácticas

#### Tech Stack por Defecto
- **Framework**: Phoenix 1.8.0-rc.4
- **Lenguaje**: Elixir 1.18.1
- **Base de datos**: PostgreSQL 17+
- **ORM**: ecto_sql 3.10
- **CSS**: TailwindCSS 4.0+
- **Componentes UI**: daisyUI 5
- **Iconos**: heroicons
- **CI/CD**: GitHub Actions

#### Principios de Desarrollo
- **Simplicidad**: Implementar en las menores líneas posibles
- **Legibilidad**: Priorizar claridad sobre micro-optimizaciones
- **DRY**: No repetir código
- **Estructura de archivos**: Responsabilidad única por archivo

## Flujos de Trabajo Principales

### 1. Crear un Nuevo Producto

```bash
# Usar el comando plan-product
# Genera toda la documentación base del producto
```

**Archivos generados**:
- `.claude/product/mission.md`
- `.claude/product/mission-lite.md`
- `.claude/product/tech-stack.md`
- `.claude/product/roadmap.md`
- `.claude/product/decisions.md`

### 2. Crear una Nueva Característica

```bash
# Usar el comando create-spec
# Genera especificación completa con tareas
```

**Estructura generada**:
```
.claude/specs/YYYY-MM-DD-feature-name/
├── spec.md              # Especificación completa
├── spec-lite.md         # Resumen para IA
├── tasks.md             # Desglose de tareas
└── sub-specs/
    ├── technical-spec.md
    ├── database-schema.md
    ├── api-spec.md
    └── tests.md
```

### 3. Ejecutar Tareas de Desarrollo

```bash
# Usar el comando execute-tasks
# Ejecuta todo el flujo de desarrollo
```

**Proceso automatizado**:
1. Selección de tareas
2. Configuración de entorno
3. Gestión de Git
4. Implementación
5. Pruebas
6. Pull Request

## Integración con Phoenix

### Guías Contextuales
El sistema incluye guías completas de Phoenix que se cargan dinámicamente según el contexto:

- **Controladores**: Patrones y mejores prácticas
- **Seguridad**: Autenticación y autorización
- **Contextos**: Modelado de datos y boundaries
- **Despliegue**: Configuración de producción

### Estilos de Código
- **CSS**: Integración completa con daisyUI 5 y TailwindCSS 4
- **HTML**: Estructura semántica y accesibilidad
- **JavaScript**: Patrones modernos y mejores prácticas

## Características Avanzadas

### 1. Carga Condicional de Contexto
El sistema verifica automáticamente qué información ya está en contexto para evitar duplicación y optimizar el rendimiento.

### 2. Plantillas Inteligentes
Todas las plantillas incluyen metadatos, versionado y estructura consistente.

### 3. Validación Automática
Verificaciones integradas en cada paso del proceso para asegurar calidad.

### 4. Integración Git Completa
Gestión automática de ramas, commits y pull requests siguiendo mejores prácticas.

## Uso Práctico

### Para Desarrolladores
1. **Configuración inicial**: Ejecutar `plan-product` para nuevos proyectos
2. **Desarrollo de características**: Usar `create-spec` seguido de `execute-tasks`
3. **Análisis de código existente**: Utilizar `analyze-product`

### Para Equipos
- **Estándares consistentes**: Todos los proyectos siguen las mismas convenciones
- **Documentación automática**: Generación automática de documentación técnica
- **Workflow unificado**: Proceso estándar de desarrollo y despliegue

## Extensibilidad

El sistema está diseñado para ser extensible:
- **Nuevos agentes**: Agregar agentes especializados para funcionalidades específicas
- **Plantillas personalizadas**: Modificar o agregar nuevas plantillas
- **Estándares específicos**: Adaptar estándares para diferentes tipos de proyecto

## Beneficios Clave

1. **Consistencia**: Estructura uniforme en todos los proyectos
2. **Eficiencia**: Automatización de tareas repetitivas
3. **Calidad**: Estándares y mejores prácticas integrados
4. **Escalabilidad**: Sistema modular y extensible
5. **Colaboración**: Documentación y procesos estandarizados

Este sistema representa una solución completa para el desarrollo profesional de aplicaciones Phoenix con asistencia de IA, proporcionando estructura, consistencia y eficiencia en todo el proceso de desarrollo.