| System | Exec Mode | Descripcion | Type |
| --- | --- | --- | --- |
| Agente unico | Llamadas de MCP | Agente unico llamada a mcp server directo | A1 |
| Agente unico | Code Mode | Code execution mcp | A2 |
| Multi Agente | Llamadas de MCP | Multiples agentes llamando a un mcp server | A3 |
| Multi Agente | Code Mode | Code execution mcp | A4 |

## MCP Servers

En total tenemos 8 mcp servers

- dev tool kit - toolkit para developers
- nutrition - mcp de nutricion de que comer etc.
- morse - mcp de codigo morse
- nasa - mcp de nasa
- cocktails - mcp de hacer bebidas de cocktail
- alphavantage
- lastfm
- postgres

## Tasks

La idea es que para cada uno de estos mcp’s vamos a tener un markdown que tiene la siguiente estructura. 

Con los objetivos a cumplir

```markdown
- Overview
	- Task naming convention
- Description
- lista de tasks
- grading critera (grading instructions)
- Notes
```

Un llm al final revisa estos tasks y califica el output

## Tipos de tasks

Existen 3 tipos de tasks:

- Single tool
- Chained tool task
- Task with a lot of tools

## Tabla posible de resultados

| Run  | Agent | Task_id | Metrics | Servers |
| --- | --- | --- | --- | --- |
| id_1 | A1 | DevToolKit | JSON | devtoolkit,postgres |
| id_2 | A1 | DevToolKit | JSON | devtoolkit |
| id_3 | A2 | postgres | JSON | postgres,nasa |

## Corrida:

- vamos a correr todos los tasks con cada tipo de agente, con diferentes numero de servidores a disposicion
- Al final de la ejecucion un LLM va a calificar que tan bien le fue con respecto a los markdowns de tasks
