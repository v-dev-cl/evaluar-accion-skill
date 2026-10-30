# evaluar-accion — Skill para Claude, ChatGPT, Gemini y más

🇨🇱 **Español (Chile)** · [🇺🇸 English (US)](README.en-US.md)

Una skill que evalúa una acción con un **scorecard de 100 puntos** y entrega un veredicto, un semáforo de mercado y un plan de entrada escalonado.

Sigue el estándar abierto [Agent Skills](https://agentskills.io/) (una carpeta con un archivo `SKILL.md`), así que funciona en Claude, ChatGPT, Gemini, Codex, Gemini CLI y otros agentes compatibles.

Está pensada para quien invierte desde Chile en acciones de EE.UU. (y a veces chilenas), con horizonte de mediano/largo plazo y compra por tramos. **Siempre responde en español.**

> ⚠️ **No es asesoría financiera.** La skill arma un análisis automático con datos de búsquedas web, que pueden venir incompletos, desactualizados o derechamente equivocados. Revisa las cifras antes de invertir. Los comentarios tributarios son generales: confírmalos con un contador.

## Qué hace

1. **Recolecta datos frescos y con fecha** vía búsqueda web: precio, YTD, rango de 52 semanas, último trimestre, revisiones de EPS, P/E vs historia y pares, consenso de analistas, exposición a China/aranceles, insiders, técnico y próximos catalizadores.
2. **Semáforo de mercado**: bono a 10 años, spread high yield, VIX y S&P 500 vs su máximo. No suma puntos; define el ritmo de entrada.
3. **Scorecard de 100 puntos**:

   | Bloque | Puntos |
   |---|---|
   | A. Calidad del negocio | 20 |
   | B. Momento de resultados | 15 |
   | C. Valuación | 20 |
   | D. Por qué cayó / tesis | 15 |
   | E. Consenso de analistas | 5 |
   | F. Técnico | 10 |
   | G. Exposición macro y geopolítica | 10 |
   | H. Señales internas | 5 |

4. **Veredicto**: candidata a compra escalonada (75–100), watchlist con gatillos (60–74), esperar señales (45–59) o evitar por ahora (< 45), con reglas que topan el veredicto (por ejemplo, si la caída es estructural el máximo es watchlist).
5. **Plan de entrada**: tres tramos con niveles concretos, una fecha límite, el tamaño máximo de la posición y qué invalidaría la tesis.

Si le pasas varios tickers, los evalúa uno por uno y cierra con una tabla comparativa.

## Instalación

> **Requisito en cualquier asistente: búsqueda web activada.** La skill no usa precios ni cifras de memoria; sin búsqueda web te va a avisar y no va a evaluar.

Casi todas las opciones usan el mismo ZIP: [`evaluar-accion.zip`](https://github.com/v-dev-cl/evaluar-accion-skill/releases/latest/download/evaluar-accion.zip) (desde [Releases](https://github.com/v-dev-cl/evaluar-accion-skill/releases/latest)). No lo descomprimas.

> No uses el botón verde **Code → Download ZIP** del repo: ese ZIP trae el README y otros archivos, y los asistentes lo rechazan.

Los pasos para ChatGPT y Gemini vienen de la documentación oficial de cada proveedor (octubre de 2026). Los menús cambian seguido, así que pueden variar un poco.

### Opción 1 — Claude.ai (web y app de escritorio)

1. **Activa la ejecución de código:**
   - **Planes Free, Pro y Max:** *Configuración > Capacidades* y activa **Ejecución de código y creación de archivos** (en el diseño nuevo de Configuración se llama **Crear y editar archivos**).
   - **Planes Team y Enterprise:** *Configuración de la organización > Complementos y habilidades*, pestaña **Política**, y verifica que estén activas **Ejecución de código en la nube y creación de archivos** y **Habilidades**. En Enterprise lo tiene que hacer un owner.
2. **Sube la skill:** *Personalizar > Habilidades*, botón **+**, luego **+ Crear habilidad**, después **Cargar una habilidad**, y sube `evaluar-accion.zip`.
3. **Actívala:** busca `evaluar-accion` en la lista y prende su interruptor.

> Si tienes Claude en inglés, los nombres de los menús están en el [README en inglés](README.en-US.md#option-1--claudeai-web-and-desktop-app).

### Opción 2 — Claude Code

**Para todos tus proyectos** (skill personal):

```bash
git clone https://github.com/v-dev-cl/evaluar-accion-skill.git
mkdir -p ~/.claude/skills
cp -r evaluar-accion-skill/evaluar-accion ~/.claude/skills/
```

**Solo para un repositorio** (skill de proyecto, se comparte con tu equipo al hacer commit): copia la carpeta `evaluar-accion` en `.claude/skills/` dentro de ese repo.

En Windows la carpeta personal es `%USERPROFILE%\.claude\skills\`.

Abre una sesión nueva de Claude Code y la vas a ver en el menú `/`.

### Opción 3 — ChatGPT

**Planes:** Skills está confirmado en Business, Enterprise, Edu y Healthcare. En Enterprise y Edu, un admin tiene que activar **Enable skill uploading**. En Plus y Pro la disponibilidad todavía no es clara: si no ves **Skills**, usa la alternativa de más abajo.

1. En la barra lateral abre **Plugins** y entra a la pestaña **Skills**.
2. Haz clic en **Create** y luego en **Upload from your computer**.
3. Sube `evaluar-accion.zip`. ChatGPT la revisa antes de habilitarla; casi siempre queda disponible apenas termina, pero puede quedar marcada para revisión.

> No encontré la versión oficial en español de estos menús. Si tienes ChatGPT en español, busca los equivalentes.

**Alternativa sin Skills (por ejemplo, Plus):** crea un **Proyecto**, abre sus instrucciones y pega el contenido completo de [`evaluar-accion/SKILL.md`](evaluar-accion/SKILL.md). Los chats dentro de ese proyecto van a seguir las instrucciones.

### Opción 4 — Gemini (app web y app para Mac)

**Cuentas:** cuentas personales de Google (mayores de 18). Las de trabajo y educación llegan después. La carga funciona en [gemini.google.com](https://gemini.google.com) y en la app para Mac, no en el celular.

1. En la barra lateral haz clic en **Personalizar** (si no lo ves, primero en **Más**) y arriba selecciona **Habilidades**.
2. Haz clic en **Crear** y luego en **Subir**.
3. Elige `evaluar-accion.zip` (también sirve la carpeta `evaluar-accion`) y haz clic en **Abrir**.
4. Haz clic en **Subir**, revisa la habilidad y haz clic en **Crear**.

### Opción 5 — Codex CLI, Gemini CLI y otros agentes

Codex CLI y Gemini CLI leen la misma carpeta personal, `~/.agents/skills/`:

```bash
git clone https://github.com/v-dev-cl/evaluar-accion-skill.git
mkdir -p ~/.agents/skills
cp -r evaluar-accion-skill/evaluar-accion ~/.agents/skills/
```

Para un solo repositorio, cópiala en `.agents/skills/` dentro de ese repo.

Otros agentes compatibles con [Agent Skills](https://agentskills.io/) (Cursor, GitHub Copilot, etc.) funcionan igual: copia la carpeta `evaluar-accion` en la carpeta de skills de esa herramienta. Revisa su documentación para saber cuál es.

## Uso

```text
/evaluar-accion NKE
/evaluar-accion MSFT BSX
evalúa NVDA, ¿vale la pena tenerla en la mira?
```

En todos los asistentes basta con pedirlo en lenguaje natural ("evalúa NKE"): la skill se carga sola cuando el pedido calza con su descripción. Para llamarla directamente:

| Asistente | Cómo llamarla |
|---|---|
| Claude Code | `/evaluar-accion NKE` |
| ChatGPT | `@evaluar-accion` |
| Gemini | escribe `/` (o `@`) y elige la habilidad |
| Codex | `$evaluar-accion NKE` |

La respuesta trae: resumen con veredicto y puntaje, tabla del scorecard, por qué cayó, semáforo de mercado, niveles de entrada con fecha límite, catalizadores, riesgos y la lista de fuentes.

## Actualizar

- **Claude.ai, ChatGPT y Gemini:** descarga el ZIP más reciente desde [Releases](https://github.com/v-dev-cl/evaluar-accion-skill/releases/latest), borra la versión antigua y sube la nueva. En Gemini también puedes reemplazar la habilidad desde su menú de opciones.
- **Claude Code, Codex CLI y Gemini CLI:** `git pull` en tu clon y vuelve a copiar la carpeta.

## Personalizar

Todo vive en [`evaluar-accion/SKILL.md`](evaluar-accion/SKILL.md), en texto plano. Algunos ajustes típicos:

- **Umbrales del semáforo** (sección 2) y **pesos del scorecard** (sección 3).
- **Tamaño máximo por acción** (sección 5): 5 % del portafolio, o hasta 10 % en megacaps de alta calidad.
- **Contexto tributario** (al final): está escrito para Chile (art. 107 y Global Complementario).

Si editas la skill para subirla a Claude.ai, ChatGPT o Gemini, vuelve a comprimirla **con la carpeta incluida** (el ZIP tiene que contener `evaluar-accion/SKILL.md`, y el nombre de la carpeta tiene que coincidir con el `name` del archivo):

```bash
zip -r evaluar-accion.zip evaluar-accion
```

## Licencia

[MIT](LICENSE)
