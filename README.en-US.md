# evaluar-accion — a skill for Claude, ChatGPT, Gemini, and more

[🇨🇱 Español (Chile)](README.md) · 🇺🇸 **English (US)**

A skill that rates a single stock on a **100-point scorecard** and returns a verdict, a market traffic light, and a staged entry plan.

It follows the open [Agent Skills](https://agentskills.io/) standard (a folder with a `SKILL.md` file), so it works in Claude, ChatGPT, Gemini, Codex, Gemini CLI, and other compatible agents.

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
5. **Entry plan keyed to the verdict**: for a buy candidate, three tranches at concrete levels (the first at the current price) and a deadline; for a watchlist name, the levels are re-evaluation triggers; for wait or avoid, only the signals that would prompt a re-check. Always with a maximum position size and what would invalidate the thesis.

Data rules: every figure is dated, a figure that cannot be found scores 0 and is marked "sin dato" (three or more cap the verdict), and if the company reports within 10 days the skill says so and suggests waiting for the report.

Give it several tickers and it scores each one, then ends with a comparison table.

## Installation

> **Required in every assistant: web search turned on.** The skill never uses prices or figures from memory. Without web search it tells you so and doesn't run the evaluation.

Most options use the same ZIP: [`evaluar-accion.zip`](https://github.com/v-dev-cl/evaluar-accion-skill/releases/latest/download/evaluar-accion.zip) (from [Releases](https://github.com/v-dev-cl/evaluar-accion-skill/releases/latest)). Don't unzip it.

> Don't use the repo's green **Code → Download ZIP** button. That archive includes the READMEs and other files, and the assistants reject it.

The ChatGPT and Gemini steps come from each vendor's official docs (October 2026). Their menus change often, so the labels may differ slightly.

### Option 1 — Claude.ai (web and desktop app)

1. **Turn on code execution:**
   - **Free, Pro, and Max plans:** go to *Settings > Capabilities* and turn on **Code execution and file creation** (the updated Settings layout calls it **Create and edit files**).
   - **Team and Enterprise plans:** go to *Organization settings > Plugins & skills*, open the **Policy** tab, and make sure **Cloud code execution and file creation** and **Skills** are on. On Enterprise, an owner has to do this.
2. **Upload the skill:** go to *Customize > Skills*, click **+**, then **+ Create skill**, choose **Upload a skill**, and upload `evaluar-accion.zip`.
3. **Turn it on:** find `evaluar-accion` in the list and flip its toggle.

> If your Claude interface is in Spanish, the [Spanish README](README.md#opción-1--claudeai-web-y-app-de-escritorio) has the menu names in Spanish.

### Option 2 — Claude Code

**For all your projects** (personal skill):

```bash
git clone https://github.com/v-dev-cl/evaluar-accion-skill.git
mkdir -p ~/.claude/skills
cp -r evaluar-accion-skill/evaluar-accion ~/.claude/skills/
```

**For one repository only** (project skill, shared with your team when committed): copy the `evaluar-accion` folder into `.claude/skills/` inside that repo.

On Windows, the personal folder is `%USERPROFILE%\.claude\skills\`.

Start a new Claude Code session and it shows up in the `/` menu.

### Option 3 — ChatGPT

**Plans:** Skills is confirmed on Business, Enterprise, Edu, and Healthcare. On Enterprise and Edu, an admin has to turn on **Enable skill uploading**. Availability on Plus and Pro is still unclear. If you don't see **Skills**, use the fallback below.

1. In the sidebar, open **Plugins** and go to the **Skills** tab.
2. Click **Create**, then **Upload from your computer**.
3. Upload `evaluar-accion.zip`. ChatGPT scans it first. Most skills are available as soon as the scan finishes, but some get flagged for review.

**Fallback without Skills (for example, on Plus):** create a **Project**, open its instructions, and paste the full contents of [`evaluar-accion/SKILL.md`](evaluar-accion/SKILL.md). Chats inside that project follow the instructions.

### Option 4 — Gemini (web app and Mac app)

**Accounts:** personal Google accounts (18+). Work and school accounts come later. Uploading works on [gemini.google.com](https://gemini.google.com) and in the Mac app, not on mobile.

1. In the sidebar, click **Customize** (if you don't see it, click **More** first), then select **Skills** at the top.
2. Click **Create**, then **Upload**.
3. Choose `evaluar-accion.zip` (the `evaluar-accion` folder works too) and click **Open**.
4. Click **Upload**, review the skill, and click **Create**.

### Option 5 — Codex CLI, Gemini CLI, and other agents

Codex CLI and Gemini CLI both read the same personal folder, `~/.agents/skills/`:

```bash
git clone https://github.com/v-dev-cl/evaluar-accion-skill.git
mkdir -p ~/.agents/skills
cp -r evaluar-accion-skill/evaluar-accion ~/.agents/skills/
```

For one repository only, copy it into `.agents/skills/` inside that repo.

Other [Agent Skills](https://agentskills.io/)-compatible agents (Cursor, GitHub Copilot, and others) work the same way: copy the `evaluar-accion` folder into that tool's skills folder. Check its docs for the location.

## Usage

```text
/evaluar-accion NKE
/evaluar-accion MSFT BSX
evaluate NVDA — is it worth putting on my watchlist?
```

In every assistant you can just ask in plain language ("evaluate NKE"). The assistant loads the skill on its own when a request matches its description. To call it directly:

| Assistant | How to call it |
|---|---|
| Claude Code | `/evaluar-accion NKE` |
| ChatGPT | `@evaluar-accion` |
| Gemini | type `/` (or `@`) and pick the skill |
| Codex | `$evaluar-accion NKE` |

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

- **Claude.ai, ChatGPT, and Gemini:** download the latest ZIP from [Releases](https://github.com/v-dev-cl/evaluar-accion-skill/releases/latest), delete the old version, and upload the new one. Gemini also lets you replace a skill from its options menu.
- **Claude Code, Codex CLI, and Gemini CLI:** run `git pull` in your clone and copy the folder again.

## Customize

Everything lives in [`evaluar-accion/SKILL.md`](evaluar-accion/SKILL.md) as plain text. Common tweaks:

- **Traffic-light thresholds** (section 2) and **scorecard weights** (section 3).
- **Maximum position size** (section 5): 5% of the portfolio, or up to 10% for high-quality megacaps.
- **Traffic-light thresholds** are dated (October 2026) with a note to review them every 12 months.
- **Tax notes** (section 7): written for Chile. Chilean shares with *presencia bursátil*: 10% single tax until 2026-12-31 (Law 21.420) and tax-free again from 2027-01-01 (article 107, Law 21.850); foreign shares: *Global Complementario* income tax plus 15% withholding on US dividends under the Chile–US treaty. Confirm with an accountant and replace them with your own country's rules.

If you edit the skill to upload it to Claude.ai, ChatGPT, or Gemini, re-zip it **with the folder included**. The ZIP must contain `evaluar-accion/SKILL.md`, and the folder name must match the `name` in the file:

```bash
zip -rX evaluar-accion.zip evaluar-accion
```

## Publishing a release (maintainers)

Every PR runs `validate` (frontmatter per the Agent Skills spec, blocks summing to 100, both READMEs carrying the download link). To publish:

1. Bump `metadata.version` in `evaluar-accion/SKILL.md` and merge to `main`.
2. Tag that version: `git tag v1.2.0 && git push origin v1.2.0`.
3. The `release` workflow builds `evaluar-accion.zip` (folder at the ZIP root) and creates the release with the ZIP attached. The `releases/latest/download/evaluar-accion.zip` link in this README only follows releases: **no tag, no update** for Claude.ai, ChatGPT, or Gemini users.

## License

[MIT](LICENSE)
