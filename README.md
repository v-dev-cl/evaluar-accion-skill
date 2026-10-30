# evaluar-accion — Skill para Claude

🇨🇱 **Español (Chile)** · [🇺🇸 English (US)](README.en-US.md)

Una [skill de Claude](https://support.claude.com/es/articles/12512180-use-skills-in-claude) que evalúa una acción con un **scorecard de 100 puntos** y entrega un veredicto, un semáforo de mercado y un plan de entrada escalonado.

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

### Opción 1 — Claude.ai (web y app de escritorio)

Necesitas tener activada la ejecución de código. La skill usa búsqueda web, así que también tiene que estar disponible en tu chat.

1. **Descarga el ZIP:** [`evaluar-accion.zip`](https://github.com/v-dev-cl/claude-skill-evaluar-accion/releases/latest/download/evaluar-accion.zip) (desde [Releases](https://github.com/v-dev-cl/claude-skill-evaluar-accion/releases/latest)). No lo descomprimas.
   > No uses el botón verde **Code → Download ZIP** del repo: ese ZIP trae el README y otros archivos, y Claude lo rechaza.
2. **Activa la ejecución de código:**
   - **Planes Free, Pro y Max:** *Configuración > Capacidades* y activa **Ejecución de código y creación de archivos** (en el diseño nuevo de Configuración se llama **Crear y editar archivos**).
   - **Planes Team y Enterprise:** *Configuración de la organización > Complementos y habilidades*, pestaña **Política**, y verifica que estén activas **Ejecución de código en la nube y creación de archivos** y **Habilidades**. En Enterprise lo tiene que hacer un owner.
3. **Sube la skill:** *Personalizar > Habilidades*, botón **+**, luego **+ Crear habilidad**, después **Cargar una habilidad**, y sube `evaluar-accion.zip`.
4. **Actívala:** busca `evaluar-accion` en la lista y prende su interruptor.

> Si tienes Claude en inglés, los nombres de los menús están en el [README en inglés](README.en-US.md#option-1--claudeai-web-and-desktop-app).

### Opción 2 — Claude Code

**Para todos tus proyectos** (skill personal):

```bash
git clone https://github.com/v-dev-cl/claude-skill-evaluar-accion.git
mkdir -p ~/.claude/skills
cp -r claude-skill-evaluar-accion/evaluar-accion ~/.claude/skills/
```

**Solo para un repositorio** (skill de proyecto, se comparte con tu equipo al hacer commit): copia la carpeta `evaluar-accion` en `.claude/skills/` dentro de ese repo.

En Windows la carpeta personal es `%USERPROFILE%\.claude\skills\`.

Abre una sesión nueva de Claude Code y la vas a ver en el menú `/`.

## Uso

```text
/evaluar-accion NKE
/evaluar-accion MSFT BSX
evalúa NVDA, ¿vale la pena tenerla en la mira?
```

En Claude.ai basta con pedirlo en lenguaje natural ("evalúa NKE"): Claude carga la skill sola cuando el pedido calza con su descripción.

La respuesta trae: resumen con veredicto y puntaje, tabla del scorecard, por qué cayó, semáforo de mercado, niveles de entrada con fecha límite, catalizadores, riesgos y la lista de fuentes.

## Actualizar

- **Claude.ai:** descarga el ZIP más reciente desde [Releases](https://github.com/v-dev-cl/claude-skill-evaluar-accion/releases/latest), borra la versión antigua en *Personalizar > Habilidades* y sube la nueva.
- **Claude Code:** `git pull` en tu clon y vuelve a copiar la carpeta.

## Personalizar

Todo vive en [`evaluar-accion/SKILL.md`](evaluar-accion/SKILL.md), en texto plano. Algunos ajustes típicos:

- **Umbrales del semáforo** (sección 2) y **pesos del scorecard** (sección 3).
- **Tamaño máximo por acción** (sección 5): 5 % del portafolio, o hasta 10 % en megacaps de alta calidad.
- **Contexto tributario** (al final): está escrito para Chile (art. 107 y Global Complementario).

Si editas la skill para Claude.ai, vuelve a comprimirla **con la carpeta incluida** (el ZIP tiene que contener `evaluar-accion/SKILL.md`, y el nombre de la carpeta tiene que coincidir con el `name` del archivo):

```bash
zip -r evaluar-accion.zip evaluar-accion
```

## Licencia

[MIT](LICENSE)
