# evaluar-accion — a Claude skill

[🇨🇱 Español (Chile)](README.md) · 🇺🇸 **English (US)**

A [Claude skill](https://support.claude.com/en/articles/12512180-use-skills-in-claude) that rates a single stock on a **100-point scorecard** and returns a verdict, a market traffic light, and a staged entry plan.

It was built for someone investing from Chile in US stocks (and the occasional Chilean one), with a medium-to-long-term horizon who buys in tranches. **The skill is written in Spanish and always answers in Spanish.** See [Answer in English](#answer-in-english) to change that.

> ⚠️ **This is not financial advice.** The skill builds an automated analysis from web search results, which can be incomplete, outdated, or wrong. Check the numbers before you invest. The tax notes are general and Chile-specific: confirm them with an accountant.

## What it does

1. **Collects fresh, dated data** with web search: price, YTD, 52-week range, latest quarter, EPS revisions, forward P/E vs. its own history and peers, analyst consensus, China/tariff exposure, insider activity, technicals, and upcoming catalysts.
2. **Market traffic light**: 10-year Treasury, high-yield spread, VIX, and the S&P 500's distance from its high. It adds no points; it sets the pace of entry.
3. **100-point scorecard**:

   | Block | Points |
   |---|---|
   | A. Business quality | 20 |
   | B. Earnings momentum | 15 |
   | C. Valuation | 20 |
   | D. Why it fell / thesis | 15 |
   | E. Analyst consensus | 5 |
   | F. Technicals | 10 |
   | G. Macro and geopolitical exposure | 10 |
   | H. Insider signals | 5 |

4. **Verdict**: staged-buy candidate (75–100), watchlist with triggers (60–74), wait for concrete signals (45–59), or avoid for now (< 45). Rules cap the verdict in some cases (for example, a structural decline caps it at watchlist).
5. **Entry plan**: three tranches at concrete price levels, a deadline, a maximum position size, and what would invalidate the thesis.

Give it several tickers and it scores each one, then ends with a comparison table.

## Installation

### Option 1 — Claude.ai (web and desktop app)

Code execution must be turned on. The skill relies on web search, so that has to be available in your chat too.

1. **Download the ZIP:** [`evaluar-accion.zip`](https://github.com/v-dev-cl/claude-skill-evaluar-accion/releases/latest/download/evaluar-accion.zip) (from [Releases](https://github.com/v-dev-cl/claude-skill-evaluar-accion/releases/latest)). Don't unzip it.
   > Don't use the repo's green **Code → Download ZIP** button. That archive includes the READMEs and other files, and Claude rejects it.
2. **Turn on code execution:**
   - **Free, Pro, and Max plans:** go to *Settings > Capabilities* and turn on **Code execution and file creation** (the updated Settings layout calls it **Create and edit files**).
   - **Team and Enterprise plans:** go to *Organization settings > Plugins & skills*, open the **Policy** tab, and make sure **Cloud code execution and file creation** and **Skills** are on. On Enterprise, an owner has to do this.
3. **Upload the skill:** go to *Customize > Skills*, click **+**, then **+ Create skill**, choose **Upload a skill**, and upload `evaluar-accion.zip`.
4. **Turn it on:** find `evaluar-accion` in the list and flip its toggle.

> If your Claude interface is in Spanish, the [Spanish README](README.md#opción-1--claudeai-web-y-app-de-escritorio) has the menu names in Spanish.

### Option 2 — Claude Code

**For all your projects** (personal skill):

```bash
git clone https://github.com/v-dev-cl/claude-skill-evaluar-accion.git
mkdir -p ~/.claude/skills
cp -r claude-skill-evaluar-accion/evaluar-accion ~/.claude/skills/
```

**For one repository only** (project skill, shared with your team when committed): copy the `evaluar-accion` folder into `.claude/skills/` inside that repo.

On Windows, the personal folder is `%USERPROFILE%\.claude\skills\`.

Start a new Claude Code session and it shows up in the `/` menu.

## Usage

```text
/evaluar-accion NKE
/evaluar-accion MSFT BSX
evaluate NVDA — is it worth putting on my watchlist?
```

On Claude.ai you can just ask in plain language ("evaluate NKE"). Claude loads the skill on its own when a request matches its description.

The answer includes a summary with the verdict and score, the scorecard table, why the stock fell, the market traffic light, entry levels with a deadline, catalysts, risks, and a list of sources.

## Answer in English

Open [`evaluar-accion/SKILL.md`](evaluar-accion/SKILL.md) and in the opening paragraph replace:

```text
Responder siempre en español.
```

with:

```text
Always answer in English, even though these instructions are in Spanish.
```

Then re-zip and re-upload it (see [Customize](#customize)). The scorecard logic stays the same.

## Update

- **Claude.ai:** download the latest ZIP from [Releases](https://github.com/v-dev-cl/claude-skill-evaluar-accion/releases/latest), delete the old version under *Customize > Skills*, and upload the new one.
- **Claude Code:** run `git pull` in your clone and copy the folder again.

## Customize

Everything lives in [`evaluar-accion/SKILL.md`](evaluar-accion/SKILL.md) as plain text. Common tweaks:

- **Traffic-light thresholds** (section 2) and **scorecard weights** (section 3).
- **Maximum position size** (section 5): 5% of the portfolio, or up to 10% for high-quality megacaps.
- **Tax notes** (at the end): written for Chile (article 107 and the *Global Complementario* income tax). Replace them with your own country's rules.

If you edit the skill for Claude.ai, re-zip it **with the folder included**. The ZIP must contain `evaluar-accion/SKILL.md`, and the folder name must match the `name` in the file:

```bash
zip -r evaluar-accion.zip evaluar-accion
```

## License

[MIT](LICENSE)
