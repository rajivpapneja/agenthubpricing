# AgentHub Pricing Tool

Sales pricing configurator for [Synapt AgentHub](https://www.prodapt.com) — Prodapt's enterprise AI agent catalog and control plane.

Used by sales teams to model CAPEX/OPEX quotes live with customers, covering platform licensing, agent packaging, SI delivery, integrations, infra sizing, and LLM token consumption.

---

## Features

- **9-step wizard** with live CAPEX/OPEX cost panel that updates on every input
- **T&M + Fixed Price** modes with per-role negotiated day rate override
- **Token consumption model** with governance overhead buffer (prevents 20–35% underestimation)
- **Infra sizing** for AWS, Azure, GCP, and On-prem
- **Excel export** — 6-sheet workbook (Summary, CAPEX, OPEX, Token Model, Infra Sizing, Rate Card)
- **Downloadable desktop app** — Mac `.dmg` + Windows `.exe` installer via Electron

---

## Quick Start (web)

```bash
pip3 install -r requirements.txt
python3 -m streamlit run app/main.py
# Open http://localhost:8501
```

---

## Build Desktop App

### Mac (.dmg)
```bash
./build.sh
# Output: dist-electron/AgentHub Pricing-1.0.0.dmg
```

### Windows (.exe)
```powershell
.\build.ps1
# Output: dist-electron\AgentHub Pricing Setup 1.0.0.exe
```

### Dev mode (Electron wrapping local Streamlit)
```bash
# Terminal 1 — start Streamlit
python3 -m streamlit run app/main.py

# Terminal 2 — launch Electron
cd electron && NODE_ENV=development ./node_modules/.bin/electron main.js
```

---

## Project Structure

```
agenthub-pricing/
├── app/
│   ├── main.py                  # Streamlit 9-step wizard
│   └── assets/logo.png          # AgentHub logo
├── pricing_engine/
│   ├── models.py                # Pydantic input/output models
│   ├── rate_loader.py           # YAML rate card loader
│   └── calculators/
│       ├── platform.py          # Platform tier licensing
│       ├── agents.py            # Agent catalog & packaging
│       ├── integrations.py      # SoR + LLM integration costs
│       ├── infra.py             # Cloud/on-prem infra sizing
│       ├── tokens.py            # LLM token consumption model
│       ├── delivery.py          # SI delivery cost (T&M / Fixed)
│       └── support.py           # Support tier pricing
├── export/
│   └── excel_exporter.py        # 6-sheet openpyxl workbook
├── config/
│   └── rate_cards.yaml          # All default rates (editable)
├── electron/
│   ├── main.js                  # Electron main process
│   ├── loading.html             # Branded splash screen
│   ├── preload.js               # Context bridge
│   └── package.json             # electron-builder config
├── agenthub.spec                # PyInstaller bundling spec
├── build.sh                     # Mac build script
├── build.ps1                    # Windows build script
└── requirements.txt
```

---

## Customising Rate Cards

All default pricing lives in [`config/rate_cards.yaml`](config/rate_cards.yaml):

- Platform tier annual license & setup fees
- Delivery role day rates (standard + floor for T&M)
- LLM model pricing per 1M tokens
- Infra instance profiles per cloud
- Support tier percentages
- Agent packaging cost per readiness level

Sales reps can override any value per deal directly in the UI.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| UI | Streamlit |
| Models | Pydantic v2 |
| Charts | Plotly |
| Excel | openpyxl |
| Config | PyYAML |
| Desktop | Electron + electron-builder |
| Bundler | PyInstaller |

---

© 2026 Prodapt. All rights reserved.
