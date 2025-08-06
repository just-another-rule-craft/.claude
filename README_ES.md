# Phoenix Rules - Sistema de Desarrollo Impulsado por IA para Aplicaciones Phoenix

## Descripción General

Este proyecto contiene una arquitectura de sistema dual integral para el desarrollo de aplicaciones Phoenix con Elixir, diseñada para trabajar perfectamente con **Claude AI** y **GitHub Copilot**. El sistema combina el poder de dos asistentes de IA a través de estructuras de directorio complementarias que permiten un desarrollo Phoenix consistente, eficiente y de alta calidad.

## 🏗️ Arquitectura de Sistema Dual

### `.claude/` - Sistema de Desarrollo con Claude AI
**Propósito**: Gestión completa del ciclo de vida del proyecto con Claude AI

### `.github/` - Integración con GitHub Copilot
**Propósito**: Calidad de código, patrones y desarrollo colaborativo con GitHub Copilot

## 📁 Resumen de Estructura de Directorios

### `.claude/` - Sistema Claude AI
```
.claude/
├── agents/                    # Agentes de IA especializados
│   ├── context-fetcher.md     # Recuperación inteligente de contexto
│   ├── file-creator.md        # Creación de archivos y plantillas
│   ├── git-workflow.md        # Gestión de workflows Git
│   ├── refactor-agent.md      # Especialista en refactorización
│   └── test-runner.md         # Ejecución y verificación de pruebas
├── commands/                  # Comandos de desarrollo de alto nivel
│   ├── analyze-product.md     # Análisis y evaluación de productos
│   ├── create-spec.md         # Creación de especificaciones
│   ├── execute-tasks.md       # Workflows de ejecución de tareas
│   ├── plan-product.md        # Planificación y arquitectura de productos
│   ├── implement-phoenix-feature.md # Implementación de características
│   ├── debug-phoenix-issue.md # Workflows de depuración
│   ├── api-dev.md            # Desarrollo de APIs
│   ├── deploy-monitor.md     # Despliegue y monitoreo
│   └── security-audit.md     # Auditoría de seguridad
├── instructions/              # Instrucciones detalladas de procesos
│   ├── core/                  # Instrucciones de desarrollo central
│   └── meta/                  # Instrucciones de meta-procesos
├── phoenix_guide/             # Experiencia del framework Phoenix
│   ├── controllers.md         # Patrones de controladores
│   ├── live_view.md          # Desarrollo LiveView
│   ├── security.md           # Mejores prácticas de seguridad
│   ├── data_modelling/       # Modelado de datos y contextos
│   ├── deployment/           # Despliegue en producción
│   └── testing/              # Estrategias de testing
├── standards/                 # Estándares de desarrollo
│   ├── best-practices.md     # Mejores prácticas generales
│   ├── code-style.md         # Guías de estilo de código
│   ├── tech-stack.md         # Especificaciones tecnológicas
│   └── claude-code-optimization.md # Integración Claude Code
└── CLAUDE.md                 # Configuración principal de Claude
```

### `.github/` - Sistema GitHub Copilot
```
.github/
├── copilot-instructions.md   # Configuración principal de GitHub Copilot
├── coding-guidelines/        # Guías de calidad de código
│   ├── phoenix-patterns.md  # Patrones específicos de Phoenix
│   ├── elixir-style.md      # Estándares de codificación Elixir
│   ├── testing-standards.md # Enfoques de testing
│   ├── security-practices.md# Guías de seguridad
│   └── performance-optimization.md # Patrones de rendimiento
├── ISSUE_TEMPLATE/          # Plantillas de issues de GitHub
│   ├── bug_report.yml       # Plantilla de reporte de bugs
│   └── feature_request.yml  # Plantilla de solicitud de características
├── pull_request_template.md # Plantilla de PR para revisión de código
└── dependabot.yml          # Gestión de dependencias
```

## 🎯 Componentes del Sistema

### 1. Sistema Claude AI (`.claude/`)

#### Ecosistema de Agentes Especializados
El sistema Claude utiliza agentes especializados que trabajan juntos para la gestión completa del proyecto:

**Context Fetcher Agent**
- **Propósito**: Recuperación inteligente de documentación y contexto
- **Capacidades**:
  - Verifica si la información ya está en contexto
  - Extrae secciones específicas de archivos
  - Utiliza búsquedas semánticas para encontrar información relevante
  - Evita duplicación de contenido

**File Creator Agent**
- **Propósito**: Creación de archivos y directorios con plantillas consistentes
- **Capacidades**:
  - Manejo de plantillas para diferentes tipos de archivo
  - Creación de estructuras de directorios
  - Aplicación de convenciones de nomenclatura
  - Operaciones batch para múltiples archivos

**Git Workflow Agent**
- **Propósito**: Gestión completa de workflows Git
- **Capacidades**:
  - Manejo de ramas con convenciones de nomenclatura
  - Creación de commits descriptivos
  - Gestión de pull requests
  - Verificación de estado antes de operaciones

**Test Runner Agent**
- **Propósito**: Ejecución y verificación de suites de pruebas
- **Capacidades**:
  - Ejecución de pruebas completas
  - Detección y reporte de fallos
  - Integración con workflows de desarrollo

**Refactor Agent**
- **Propósito**: Refactorización y optimización de código
- **Capacidades**:
  - Reconocimiento de patrones y mejoras
  - Optimización de estructura de código
  - Modernización de código legacy

#### Comandos de Desarrollo de Alto Nivel

**Plan Product** (`plan-product`)
- Genera documentación completa para nuevos productos
- Crea misión, arquitectura técnica y roadmap
- Establece fundación de proyecto y registro de decisiones

**Create Spec** (`create-spec`)
- Crea especificaciones detalladas para características
- Proceso de especificación de 8 pasos con documentación completa
- Genera especificaciones técnicas y desgloses de tareas

**Execute Tasks** (`execute-tasks`)
- Workflow completo de ejecución de tareas
- Proceso automatizado de desarrollo de 10 pasos
- Integra testing, workflow Git y despliegue

**Implement Phoenix Feature** (`implement-phoenix-feature`)
- Workflow de desarrollo enfocado en características
- Patrones de implementación específicos de Phoenix
- Testing y validación automatizados

### 2. Sistema GitHub Copilot (`.github/`)

#### Calidad de Código y Colaboración

**Instrucciones Copilot**
- **Propósito**: Configuración principal para integración GitHub Copilot
- **Características**:
  - Patrones de código específicos de Phoenix
  - Guías de desarrollo LiveView
  - Sugerencias conscientes del contexto
  - Consejos de optimización de rendimiento

**Guías de Codificación**
- **Patrones Phoenix**: Mejores prácticas específicas del framework
- **Estilo Elixir**: Convenciones e idiomas del lenguaje
- **Estándares de Testing**: Enfoques integrales de testing
- **Prácticas de Seguridad**: Desarrollo security-first
- **Optimización de Rendimiento**: Codificación consciente del rendimiento

**Integración GitHub**
- **Plantillas de Issues**: Reportes estructurados de bugs y solicitudes de características
- **Plantilla Pull Request**: Lista de verificación integral para revisión de código
- **Dependabot**: Gestión automatizada de dependencias
- **Sin CI/CD**: Enfocado en patrones de desarrollo, no en despliegue

## 🔧 Stack Tecnológico

#### Framework y Lenguaje
- **Framework**: Phoenix 1.8.0-rc.4
- **Lenguaje**: Elixir 1.18.1
- **Base de datos**: PostgreSQL 17+
- **ORM**: ecto_sql 3.10

#### Frontend y Estilos
- **CSS**: TailwindCSS 4.0+
- **Componentes UI**: daisyUI 5
- **Iconos**: heroicons

#### Principios de Desarrollo
- **Simplicidad**: Implementar en las menores líneas posibles
- **Legibilidad**: Priorizar claridad sobre micro-optimizaciones
- **DRY**: No repetir código
- **Estructura de archivos**: Responsabilidad única por archivo
- **AI-First**: Diseñado para desarrollo asistido por IA

## 🚀 Flujos de Trabajo Principales

### 1. Inicialización de Proyecto (Claude AI)

**Comando**: `plan-product`
```bash
# Crea fundación completa del proyecto
# Genera misión, arquitectura y roadmap
```

**Estructura generada**:
```
.claude/product/
├── mission.md           # Visión y objetivos del producto
├── mission-lite.md      # Resumen optimizado para IA
├── tech-stack.md        # Arquitectura técnica
├── roadmap.md          # Fases de desarrollo
└── decisions.md        # Registro de decisiones
```

### 2. Desarrollo de Características (Claude AI)

**Comando**: `create-spec` + `execute-tasks`
```bash
# Paso 1: Crear especificación
# Paso 2: Ejecutar implementación
```

**Estructura generada**:
```
.claude/specs/YYYY-MM-DD-nombre-caracteristica/
├── spec.md              # Especificación completa
├── spec-lite.md         # Resumen para IA
├── tasks.md             # Desglose de tareas
└── sub-specs/
    ├── technical-spec.md
    ├── database-schema.md
    ├── api-spec.md
    └── tests.md
```

### 3. Calidad de Código (GitHub Copilot)

**Integración Automática**:
- Sugerencias de código en tiempo real basadas en patrones Phoenix
- Recomendaciones security-first
- Consejos de optimización de rendimiento
- Sugerencias de patrones de testing

### 4. Desarrollo Colaborativo

**Integración GitHub**:
- Reporte estructurado de issues
- Revisiones integrales de PR
- Actualizaciones automatizadas de dependencias
- Patrones de código consistentes en equipo

## 🧠 Características de Integración IA

### Capacidades Claude AI
- **Planificación de Proyectos**: Generación completa de arquitectura de producto y roadmap
- **Creación de Especificaciones**: Especificaciones detalladas de características con desgloses de tareas
- **Implementación**: Workflows completos de desarrollo con testing automatizado
- **Gestión de Contexto**: Recuperación inteligente de información y conciencia de contexto
- **Integración Git**: Gestión automatizada de ramas, commits y pull requests

### Capacidades GitHub Copilot
- **Completado de Código**: Sugerencias de código conscientes de Phoenix y Elixir
- **Reconocimiento de Patrones**: Patrones específicos del framework y mejores prácticas
- **Conciencia de Seguridad**: Recomendaciones de código security-first
- **Optimización de Rendimiento**: Sugerencias de código conscientes del rendimiento
- **Integración de Testing**: Patrones de desarrollo guiado por pruebas

### Workflow Dual IA
1. **Fase de Planificación**: Claude AI crea especificaciones y estructura del proyecto
2. **Fase de Implementación**: GitHub Copilot asiste con completado de código y patrones
3. **Fase de Revisión**: Ambos sistemas aseguran calidad y consistencia
4. **Fase de Despliegue**: Claude AI gestiona el workflow completo

## 📋 Uso Práctico

### Para Desarrolladores Individuales
1. **Configuración de Proyecto**: Usar `plan-product` de Claude AI para nuevos proyectos
2. **Desarrollo de Características**: `create-spec` + `execute-tasks` con Claude AI
3. **Implementación de Código**: GitHub Copilot para asistencia en tiempo real
4. **Revisión de Código**: Verificaciones integradas de calidad y patrones

### Para Equipos de Desarrollo
- **Estándares Consistentes**: Guías de codificación unificadas en ambos sistemas IA
- **Documentación Automatizada**: Generación automática de especificaciones técnicas
- **Workflow Estandarizado**: Procesos consistentes de desarrollo y revisión
- **Compartir Conocimiento**: Patrones y mejores prácticas centralizados

### Integración con Herramientas de Desarrollo
- **VS Code**: Integración nativa de GitHub Copilot
- **Claude Code**: Integración directa con workflows de Claude AI
- **Git**: Gestión automatizada de workflows
- **Testing**: Ejecución y validación integradas de pruebas
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
## 🔧 Extensibilidad del Sistema

### Agregar Nuevos Agentes Claude AI
```bash
# Crear nuevos agentes especializados en .claude/agents/
# Seguir la estructura de plantilla de agentes
# Integrar con workflows de comandos existentes
```

### Extender Guías GitHub Copilot
```bash
# Agregar nuevas guías de codificación en .github/coding-guidelines/
# Actualizar copilot-instructions.md con nuevos patrones
# Mantener consistencia con estándares Claude AI
```

### Plantillas de Proyecto Personalizadas
- **Patrones Phoenix**: Agregar nuevos patrones específicos del framework
- **Estándares de Industria**: Adaptar para dominios o industrias específicas
- **Convenciones de Equipo**: Personalizar para requisitos específicos del equipo

## 🎯 Beneficios Clave

### Para Desarrollo Asistido por IA
1. **Poder IA Dual**: Combina la planificación de Claude AI con la codificación de GitHub Copilot
2. **Conciencia de Contexto**: Ambos sistemas entienden patrones Phoenix y Elixir
3. **Consistencia**: Estándares unificados en diferentes interacciones IA
4. **Eficiencia**: Workflows automatizados reducen tareas repetitivas
5. **Calidad**: Mejores prácticas y validación integradas

### Para Equipos de Desarrollo
1. **Estandarización**: Patrones consistentes en todos los proyectos
2. **Onboarding**: Nuevos desarrolladores pueden aprovechar patrones existentes
3. **Preservación de Conocimiento**: Experiencia capturada en formatos reutilizables
4. **Colaboración**: Workflows estructurados para coordinación de equipo
5. **Escalabilidad**: Sistema modular crece con las necesidades del equipo

### Para Desarrollo Phoenix
1. **Experiencia Framework**: Conocimiento profundo de Phoenix y Elixir integrado
2. **Patrones Modernos**: Últimos patrones y mejores prácticas Phoenix 1.8+
3. **Security First**: Consideraciones de seguridad integradas en todo
4. **Consciente del Rendimiento**: Patrones y optimizaciones de rendimiento
5. **Cultura de Testing**: Estrategias y patrones integrales de testing

## 🤝 Comunidad y Contribución

Este sistema dual-IA representa un nuevo paradigma en desarrollo de software, combinando las fortalezas de diferentes asistentes IA para desarrollo integral de aplicaciones Phoenix. El sistema está diseñado para evolucionar con el ecosistema Phoenix y las capacidades IA.

### Contribuir
- **Mejoras de Patrones**: Enviar mejores patrones y prácticas Phoenix
- **Integración IA**: Mejorar integración Claude AI y GitHub Copilot
- **Documentación**: Mejorar guías y ejemplos
- **Integración de Herramientas**: Agregar soporte para nuevas herramientas de desarrollo

## 📚 Recursos de Aprendizaje

### Primeros Pasos
1. **Configuración Claude AI**: Configurar directorio `.claude/` para tu proyecto
2. **Configuración GitHub Copilot**: Habilitar GitHub Copilot con guías `.github/`
3. **Desarrollo Phoenix**: Seguir workflows integrados de desarrollo
4. **Mejores Prácticas**: Estudiar patrones y estándares integrados

### Uso Avanzado
- **Agentes Personalizados**: Crear agentes Claude AI especializados para tareas específicas
- **Bibliotecas de Patrones**: Construir bibliotecas de patrones reutilizables para tu dominio
- **Integración de Equipo**: Adaptar el sistema para workflows específicos del equipo
- **Mejora Continua**: Evolucionar patrones basados en experiencia del proyecto

Este sistema representa una solución completa para el desarrollo profesional de aplicaciones Phoenix con asistencia de IA dual, proporcionando estructura, consistencia y eficiencia en todo el proceso de desarrollo.

## Inspiración y Créditos

Este proyecto se inspiró en el excelente trabajo realizado en [agent-os](https://github.com/buildermethods/agent-os/tree/main) por el creador en Builder Methods. Su enfoque innovador hacia los flujos de trabajo de desarrollo impulsados por IA proporcionó los conceptos fundamentales que hemos adaptado y extendido para el desarrollo con Phoenix.

## Licencia

Este proyecto está licenciado bajo la Licencia MIT - consulta el archivo [LICENSE](LICENSE) para más detalles.

### Resumen de la Licencia MIT

Se concede permiso, de forma gratuita, a cualquier persona que obtenga una copia de este software y los archivos de documentación asociados, para utilizar el Software sin restricciones, incluyendo sin limitación los derechos de usar, copiar, modificar, fusionar, publicar, distribuir, sublicenciar y/o vender copias del Software.

**Este es software completamente de código abierto - eres libre de usarlo, modificarlo y distribuirlo como consideres conveniente.**