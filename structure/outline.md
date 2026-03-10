# Estructura de Tesis: Evaluación Comparativa de Ejecución de Código MCP vs Llamadas Directas en Sistemas Multi-Agente

## Mapeo de Capítulos

### 1. Portada
- Título: "Evaluación Comparativa de Ejecución de Código MCP vs Llamadas Directas en Sistemas Multi-Agente"
- Autor, fecha, universidad, programa

### 2. Resumen / Abstract
- Resumen en español (~250 palabras): problema, método, resultados clave, conclusión
- Abstract en inglés (~250 palabras): misma estructura

### 3. Tabla de Contenidos
- Autogenerada por LaTeX

### 4. Capítulo 1: Introducción
- **1.1 Contexto**: Auge de LLMs y sistemas multi-agente, MCP como protocolo estándar
- **1.2 Planteamiento del Problema**: Consumo de tokens en MCP vs ejecución directa de código, trade-offs de rendimiento
- **1.3 Motivación**: Costos operativos, latencia, necesidad de guías de decisión
- **1.4 Objetivos**: General (evaluar comparativamente) y específicos (medir tokens, latencia, precisión, construir modelo de decisión)
- **1.5 Hipótesis**: H1-H4 sobre eficiencia de tokens, latencia, precisión y punto de cruce
- **1.6 Alcance y Limitaciones**

### 5. Capítulo 2: Revisión de Literatura
- **2.1 Protocolo MCP**: Origen, especificación, adopción (Anthropic, AWS Strands)
- **2.2 Ejecución de Código en LLMs**: Patrones de Anthropic, Cloudflare Workers, E2B
- **2.3 Sistemas Multi-Agente**: Topologías (orquestador, enjambre, pipeline), frameworks existentes
- **2.4 Benchmarks Existentes**: Evaluaciones de tool-use, benchmarks de agentes
- **2.5 Sandboxing y Seguridad**: Docker, gVisor, restricciones de red

### 6. Capítulo 3: Marco Teórico
- **3.1 Arquitectura MCP**: Cliente-servidor, transporte stdio/SSE, ciclo de vida de herramientas
- **3.2 Mecánica de Tool Calling**: Flujo de tokens, serialización JSON, overhead de protocolo
- **3.3 Ejecución de Código en Sandbox**: Docker como sandbox, montaje de volúmenes, restricciones de seguridad
- **3.4 Topologías Multi-Agente**: Patrones de orquestación, delegación de tareas
- **3.5 Economía de Tokens**: Modelos de costo, métricas de eficiencia

### 7. Capítulo 4: Diseño Experimental
- **4.1 Matriz Factorial 2×2**:
  - A1: MCP directo (1 herramienta)
  - A2: MCP directo (multi-herramienta)
  - A3: MCP con ejecución de código (1 herramienta)
  - A4: MCP con ejecución de código (multi-herramienta)
- **4.2 Suite de Tareas**:
  - T1: Tarea simple (1 herramienta)
  - T2: Tarea con concatenación (multi-herramienta secuencial)
  - T3: Tarea compleja (30+ herramientas)
- **4.3 Servidores MCP Utilizados**: Descripción de los 6 servidores (3 directos + 3 ejecución de código)
- **4.4 Métricas**: Tokens (input/output/total), latencia, precisión, pasos de ejecución
- **4.5 Variables de Control**: Modelo LLM, temperatura, repeticiones

### 8. Capítulo 5: Metodología
- **5.1 Implementación del Framework**: OrchAIstra, arquitectura de agentes
- **5.2 Sandbox Docker**: Configuración, flags de seguridad, executor.py
- **5.3 Servidores MCP**: Implementación detallada de cada servidor
- **5.4 Runner de Experimentos**: Pipeline de ejecución, recolección de métricas
- **5.5 Sistema de Evaluación**: Grading por LLM, rúbricas, scoring

### 9. Capítulo 6: Resultados
- **6.1 Resultados por Arquitectura**: Tablas comparativas A1-A4
- **6.2 Análisis de Tokens**: Gráficas de consumo, desglose input/output
- **6.3 Análisis de Latencia**: Tiempos de ejecución, overhead de Docker
- **6.4 Análisis de Precisión**: Tasas de éxito, calidad de respuestas
- **6.5 Análisis Estadístico**: Tests de significancia, intervalos de confianza

### 10. Capítulo 7: Modelo de Decisión
- **7.1 Feature Engineering**: Características de la tarea (complejidad, herramientas requeridas)
- **7.2 Entrenamiento del Clasificador**: Modelo predictivo para modo óptimo
- **7.3 Validación**: Cross-validation, métricas del modelo
- **7.4 Árbol de Decisión Interpretable**: Reglas para elegir MCP directo vs ejecución de código

### 11. Capítulo 8: Discusión
- **8.1 Interpretación de Resultados**: Relación con hipótesis H1-H4
- **8.2 Comparación con Literatura**: Hallazgos vs trabajos previos
- **8.3 Análisis de Modos de Fallo**: Cuándo falla cada enfoque y por qué
- **8.4 Implicaciones Prácticas**: Guías para desarrolladores

### 12. Capítulo 9: Conclusiones
- **9.1 Resumen de Hallazgos**
- **9.2 Contribuciones**: Framework, dataset, modelo de decisión
- **9.3 Trabajo Futuro**: Más modelos, más tareas, optimización de prompts

### 13. Referencias
- Formato natbib/plainnat, citas en texto con autor-año

### 14. Apéndice
- **A.1 Código Fuente**: Listings de servidores MCP clave
- **A.2 Tablas Completas de Resultados**
- **A.3 Esquemas de Servidores MCP**
- **A.4 Prompts Utilizados**
