# Plugins de Andrés

Marketplace privado de plugins de Claude Code.

## Instalar

```
/plugin marketplace add asmerkin/plugins-marketplace
/plugin install humanize@asmerkin
```

El repo se llama `plugins-marketplace`; el marketplace se llama `asmerkin` (así figura en `marketplace.json`) y ese nombre es el que va en `install` y `update`.

Para probar sin instalar: `claude --plugin-dir ./plugins/humanize`

## Actualizar

1. Cambiar lo que haga falta en `plugins/<plugin>/`.
2. Subir `version` en `plugins/<plugin>/.claude-plugin/plugin.json`.
3. Push.
4. En cada máquina: `/plugin marketplace update asmerkin`, o activar el auto-update en `/plugin` (repo privado: requiere `GITHUB_TOKEN`).

## Estructura

```
.claude-plugin/marketplace.json     catálogo de plugins
plugins/<plugin>/
  .claude-plugin/plugin.json        nombre y versión
  skills/<skill>/SKILL.md           skills  → <plugin>:<skill>
  agents/<agente>.md                subagentes
```

## Plugins

### `humanize`

Que la IA funcione como una persona: que entienda, razone, acompañe, empatice y se exprese como un colega.

| Capa | Qué haría un humano | Estado |
|---|---|---|
| Entender | preguntar qué querés de verdad antes de resolver lo que dijiste | pendiente |
| Razonar | pensar en voz alta, tomar posición, decir "no estoy seguro" | parcial, dentro de las skills de expresar |
| Acompañar | ir al ritmo del otro, no tirar todo de golpe | parcial, en "Para seguir" de `explain-tech` |
| Empatizar | notar frustración, cansancio o apuro, y ajustar el tono | pendiente |
| Expresar | explicar y escribir como un colega | `explain-tech`, `write-tech`, `explain-the-world` |

Idea de diseño: lo que tiene que estar siempre (entender, acompañar, empatizar) iría en un hook de `SessionStart`; lo que tiene disparador claro, como skills.

| Skill | Para qué |
|---|---|
| `explain-tech` | explicar temas técnicos en el chat, donde el lector puede repreguntar |
| `write-tech` | texto técnico que se lee solo: PRs, specs, comentarios de tickets, reportes |
| `explain-the-world` | explicar temas no técnicos en el chat: historia, economía, ciencia, salud, la vida cotidiana |

| Agente | Para qué |
|---|---|
| `voice-reviewer` | revisa en frío un texto técnico antes de publicarlo: marcas de máquina, primeras oraciones, forma, idioma. Corre con Sonnet y precarga `write-tech`, así usa las mismas reglas sin copiarlas. `write-tech` le pide a Claude que lo use; también se llama a mano con `@agent-humanize:voice-reviewer`. En el chat de claude.ai los agentes se ignoran. |

`explain-tech` y `write-tech` son solo para temas técnicos; sus descripciones mandan lo demás a `explain-the-world`.

**Pendiente:** las secciones de voz (sonar a una persona, marcas de máquina, idioma, muletillas) están repetidas en las tres skills. La idea es sacarlas a una skill propia de voz, sobre la que las otras sumen lo suyo.
