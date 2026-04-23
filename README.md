# APTS Client

Repositorio publico de distribucion para integrar proyectos con APTS sin necesidad de publicar un paquete npm.

Este repo expone un cliente HTTP minimo y dos assets de apoyo para que otros repositorios puedan descargar, copiar o versionar localmente los archivos que necesitan.

## Archivos disponibles

- `apts-client.js`: cliente Node.js para llamar a la API de APTS.
- `apts_skills.json`: contrato JSON de las skills expuestas por APTS.
- `apts-agent-guidelines.md`: guia base para `AGENTS.md` o `.github/copilot-instructions.md`.

## Descarga directa

URLs utiles:

- Raw GitHub:
  - `https://raw.githubusercontent.com/LITAT-AGC/apts-client/main/apts-client.js`
  - `https://raw.githubusercontent.com/LITAT-AGC/apts-client/main/apts_skills.json`
  - `https://raw.githubusercontent.com/LITAT-AGC/apts-client/main/apts-agent-guidelines.md`
- jsDelivr:
  - `https://cdn.jsdelivr.net/gh/LITAT-AGC/apts-client@main/apts-client.js`
  - `https://cdn.jsdelivr.net/gh/LITAT-AGC/apts-client@main/apts_skills.json`
  - `https://cdn.jsdelivr.net/gh/LITAT-AGC/apts-client@main/apts-agent-guidelines.md`

Ejemplo con PowerShell:

```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/LITAT-AGC/apts-client/main/apts-client.js" -OutFile "./scripts/apts-client.js"
```

Ejemplo con curl:

```bash
curl -L https://raw.githubusercontent.com/LITAT-AGC/apts-client/main/apts-client.js -o ./scripts/apts-client.js
```

## Requisitos del cliente

- Node.js 18 o superior.
- Un repositorio Git local si vas a usar `resolveGitIdentity()`.
- Variables de entorno:

```env
APTS_BASE_URL=http://localhost:46100/api
APTS_API_KEY=replace-with-your-api-key
```

## Uso rapido

```js
const {
  registerTask,
  resolveGitIdentity,
  logAgentProgress,
} = require('./apts-client');

async function main() {
  const identity = resolveGitIdentity();

  const task = await registerTask({
    project_url: identity.project_url,
    title: 'Integrar APTS en el proyecto',
    agent_name: identity.agent_name,
    agent_email: identity.agent_email,
  });

  await logAgentProgress(task.task_id, {
    project_url: identity.project_url,
    agent_name: identity.agent_name,
    branch: identity.branch,
    message: 'Integracion inicial completada.',
  });
}

main().catch(console.error);
```

## Flujo recomendado

1. Copiar `apts-client.js` al proyecto cliente.
2. Copiar `apts_skills.json` si tu runtime soporta function calling o tools con schema.
3. Copiar `apts-agent-guidelines.md` a `AGENTS.md` o `.github/copilot-instructions.md`.
4. Configurar `APTS_BASE_URL` y `APTS_API_KEY`.
5. Validar con `registerTask`, `logAgentProgress` y `heartbeat`.

## Fuente

La fuente original de estos archivos vive en el repositorio del servicio APTS:

- `https://github.com/LITAT-AGC/agentic-tracker`

Este repo existe solo como punto publico de distribucion ligera.