# Skills y agentes personales

Marketplace de plugins de Claude Code.

## Instalar

```
/plugin marketplace add asmerkin/plugins-marketplace
/plugin install personal@asmerkin
```

Para probar en local, desde este directorio: `/plugin marketplace add ./`

## Actualizar

1. Cambiar lo que haga falta en `plugins/<plugin>/`.
2. Subir `version` en `plugins/<plugin>/.claude-plugin/plugin.json`.
3. Push.
4. En cada máquina: `/plugin marketplace update asmerkin`, o activar el auto-update en `/plugin`.

## Estructura

```
.claude-plugin/marketplace.json     catálogo de plugins
plugins/personal/
  .claude-plugin/plugin.json        nombre y versión
  skills/<skill>/SKILL.md           skills  → personal:<skill>
  agents/<agente>.md                subagentes
```

## Plugins

| Plugin | Contenido |
|---|---|
| `personal` | skills: `teach` (explicar en el chat), `explain-in-writing` (texto que se lee solo: PRs, specs, tickets) |
