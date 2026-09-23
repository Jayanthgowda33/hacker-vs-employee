# HACKER vs EMPLOYEE 🕶️
### One Decision Can Change Everything.

An interactive cybersecurity awareness simulation for IT professionals. You play an everyday employee — every decision you make (clicking a link, approving an MFA prompt, opening a file, joining a Wi-Fi network) visibly and immediately changes what an attacker is able to do on the other side of the screen.

This is **not** a quiz, a penetration-testing tool, or a fake hacking terminal. It's a cinematic cause-and-effect story: **ACTION → CONSEQUENCE.**

**🔗 Live demo:** `https://<your-username>.github.io/hacker-vs-employee/` *(enable GitHub Pages — see below)*

---

## ✨ What it does

| Feature | Description |
|---|---|
| **Split-Screen Simulation** | LEFT: your normal employee environment (inbox, Teams, files, Wi-Fi). RIGHT: the attacker's conceptual progress — no real commands, no exploit code, ever. |
| **Live Risk Meter** | Company risk moves through SAFE → CAUTION → HIGH → CRITICAL as your decisions accumulate — not scripted per screen. |
| **5 Real Scenarios** | A phishing email, an MFA fatigue push, a manager-impersonation chat message, a suspicious attachment, and a public Wi-Fi decision. |
| **Delayed Consequences** | Not every mistake says "WRONG" immediately. Some — like clicking a phishing link — only show their cost later, the way real incidents actually unfold. |
| **Branching State** | Your ending is computed from *accumulated* flags (credentials compromised, account compromised, sensitive access, company access) — not a tally of right/wrong answers. |
| **4 Distinct Endings** | Security Hero, Close Call, Account Compromised, and Company Breach — each reachable through different decision combinations. |
| **Rewind Attack** | The signature feature. After a compromised ending, watch the attack timeline reverse, node by node, back to **"THIS WAS THE MOMENT"** — the exact first decision that started the chain. |
| **Try Again From Here** | Replay from that exact turning point using a real state snapshot — not a full restart. |
| **Cyber Awareness Score** | A final 0–100 score with a category breakdown (Phishing Detection, MFA Awareness, Social Engineering, Suspicious Files, Account Protection). |
| **Audience Mode** | Optional — pauses before revealing outcomes, so a presenter can ask a room "What would you do?" before showing what happens. |

---

## 🧱 Tech Stack

Shipped as a **single self-contained `index.html`** — no build step, no `npm install`.

- **React 18** — via CDN (UMD build)
- **Babel Standalone** — in-browser JSX transpilation
- **`useReducer`** — the entire simulation is one state machine: risk score, compromise flags, decision log, and per-scenario snapshots all live in one reducer, so nothing runs independently of what came before
- **Vanilla CSS** — enterprise-clean employee side, darker tension-driven attacker side, no Matrix clichés

Clone it, open it, done — no dependencies to install.

---

## 🎨 Design Direction

- Dark navy employee panel (60%) vs. near-black attacker panel (40%) — a strong, elegant split, not a gimmick
- Blue = normal activity, green = safe, amber = caution, red = serious threat — color always paired with text, never color alone
- Controlled glitch and pulse effects used only to mark real state changes (a node lighting up, risk escalating) — not decoration
- Full `prefers-reduced-motion` support

---

## 🚀 Getting Started (VS Code)

1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/hacker-vs-employee.git
   cd hacker-vs-employee
   ```
2. Open the folder in VS Code.
3. Right-click `index.html` → **Open with Live Server** (install the *Live Server* extension by Ritwick Dey if needed) — or just double-click `index.html` to open it in your browser directly.

---

## 🌐 Deploy with GitHub Pages

1. Repo → **Settings → Pages**
2. Source: **Deploy from a branch** → Branch: `main`, folder: `/ (root)` → **Save**
3. Visit `https://<your-username>.github.io/hacker-vs-employee/` after ~1 minute

---

## 📂 Project Structure

```
hacker-vs-employee/
├── index.html      # Entire application — markup, styles, and React app
└── README.md
```

Everything lives inside `index.html`, organized into clearly labeled sections:

```
SCENARIO DATA     → the 5 scenarios, turning-point explanations, attacker node list
ENGINE            → risk level, ending calculation, final score breakdown
STATE + REDUCER   → the single source of truth for the whole simulation
UI PIECES         → RiskMeter, AttackerGraph, AudienceOverlay
SCENARIOS         → Phishing, MFA, Impersonation, File, Wi-Fi (employee-side components)
SIM SCREEN        → the split-screen layout + mobile tab switching
INTRO / ENDING / REWIND SCREENS
APP ROOT          → screen routing
```

---

## 🧠 How the decision engine works

Nothing in this simulation is scripted independently — every scenario reads and writes to one shared state:

- **Risk score** starts at 5 and moves up or down with every decision (e.g. approving an unfamiliar MFA request: **+30**; reporting a phishing email: **−5**). Thresholds map directly to SAFE / CAUTION / HIGH / CRITICAL.
- **Compromise flags** (`credentialsCompromised`, `accountCompromised`, `sensitiveAccessGranted`, `companyAccessReached`) accumulate across scenarios. A full company breach specifically requires **multiple** unsafe decisions compounding — not one bad click.
- **The turning point** is captured automatically: the *first* unsafe decision made in the whole playthrough, timestamped, with its own explanation — this is what powers Rewind Attack.
- **State snapshots** are captured before each scenario begins, which is what makes "Try Again From Here" a real state restoration rather than a fake replay.

This logic lives in the `ENGINE` and `STATE + REDUCER` sections of `index.html` — fully deterministic and easy to extend with new scenarios.

---

## 🔮 Extending it

- **Add a new scenario:** add an entry to `SCENARIOS`, a matching component in the `SCENARIO_COMPONENTS` map, and a `TURNING_EXPLAIN` entry.
- **Adjust difficulty:** tune the `riskDelta` values in each scenario's `onDecide` payloads.
- **Add more endings:** extend `computeEnding()` and `ENDING_META` with new flag combinations.

---

## ⚠️ Scope & Safety

This is a **defensive awareness tool only**. It does not contain, reference, or simulate real exploit code, credential-theft implementations, malware, payloads, or intrusion techniques — the attacker side is a conceptual progress visualization, never operational.

---

## 📄 License

MIT — free to use, modify, and build on.
