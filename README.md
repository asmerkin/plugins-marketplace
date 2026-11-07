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
| Acompañar | ir al ritmo del otro, no tirar todo de golpe | parcial, en "Para seguir" de `teach` |
| Empatizar | notar frustración, cansancio o apuro, y ajustar el tono | pendiente |
| Expresar | explicar y escribir como un colega | `teach`, `explain-in-writing` |

Idea de diseño: lo que tiene que estar siempre (entender, acompañar, empatizar) iría en un hook de `SessionStart`; lo que tiene disparador claro, como skills.

| Skill | Para qué |
|---|---|
| `teach` | explicar en la conversación, donde el lector puede repreguntar |
| `explain-in-writing` | texto que se lee solo: PRs, specs, comentarios de tickets, reportes |
