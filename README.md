# Perth LV Feeder Rooftop-Solar Hosting Capacity & Community-Battery Study

**Status:** In Progress — Project Charter & Environment Setup (Target Completion: **Day 28 / 14 Dec 2026**, Tag `v1.0`)  
**Author:** Revant Bisht — BEng (Hons) Electrical & Electronic Engineering, University of Western Australia  
**Portfolio Links:** [revant-bisht.github.io](https://revant-bisht.github.io/) · [Technical Blog](https://revantblogs.vercel.app/)

---

## 1. Problem Statement

Western Australia's main grid —the **South West Interconnected System (SWIS)** - experiences some of the highest residential rooftop solar PV penetrations in the world. AEMO forecasts SWIS distributed PV capacity growing from approximately **3 GW to 6 GW**, alongside residential and community battery storage expanding from **0.55 GW to 2.3 GW**.

On sunny, low-demand afternoons, reverse power flow from exported residential solar drives low-voltage (LV) street feeder voltages above the statutory upper limit of **253 V** ($230\text{ V} + 10\%$ under **AS 61000.3.100**). This causes residential inverters to trip offline, creates thermal bottlenecks at distribution transformers and service mains, and introduces minimum-demand security challenges across the network—issues actively managed every day by **Western Power**, **Horizon Power**, and **AEMO WA**.

---

## 2. Core Research Questions

1. **Baseline Hosting Capacity:** How much rooftop solar PV can a typical 55-house Perth-style low-voltage street feeder ($22\text{ kV}/415\text{ V}$ distribution transformer, $230/400\text{ V}$, $50\text{ Hz}$) host before breaching statutory voltage limits (**AS 61000.3.100**) or thermal equipment ratings?
2. **DER & Network Mitigations:** By how much does the feeder's hosting capacity increase when applying:
   * **(a)** Fixed power factor operation,
   * **(b)** Autonomous inverter **Volt-VAr** and **Volt-Watt** response modes compliant with **AS/NZS 4777.2:2020**,
   * **(c)** A utility-scale community battery (modelled on Western Power's **PowerBank** trial) sized and sited for midday solar soaking, and
   * **(d)** Off-load distribution transformer tap changes?
3. **Fault-Level & Protection Adequacy:** Under **IEC 60909** short-circuit analysis, what are the prospective fault currents at the $415\text{ V}$ transformer terminals versus the far end of the LV feeder, and will standard LV feeder fuses still clear a far-end fault within required disconnection times?

---

## 3. Project Scope & Schedule Control

* **Planned Effort:** **41.0 hours** across Days 00-28 (16 Nov 2026 – 14 Dec 2026) at 2.5 hours/day.
* **Hard Deadline:** **Monday, 14 December 2026 (Day 28 — Release Tag `v1.0`)**.

### In-Scope (Minimum Viable Product — Mandatory by Day 28)
1. **Theoretical Verification (ELEC3016 Bridge):** Solve a 3-bus per-unit power flow network by hand using **ELEC3016 (Power and Machines)** transformer and line impedance theory, and verify that `pandapower` Newton-Raphson load flow matches within $<1\%$.
2. **LV Feeder Network Model:** Re-parameterise the **IEEE European LV Test Feeder** (906 buses, 55 residential loads) to Australian/Perth distribution standards ($22\text{ kV}/415\text{ V}$ Dyn11 transformer, $230/400\text{ V}$, $50\text{ Hz}$).
3. **Quasi-Static Time-Series (QSTS) Load Flow:** Simulate 24-hour and seasonal feeder operation using **Perth solar irradiance** data (via the **PVGIS API**) combined with real half-hourly Australian residential load and PV profiles from the **Ausgrid Solar Home Dataset** (300 homes).
4. **Monte-Carlo Hosting-Capacity Engine:** Stochastically allocate rooftop PV systems across random customer connection points from $0\%$ to $100\%$ penetration; record the first limiting constraint (overvoltage $>1.10\text{ p.u.}$, voltage unbalance, cable ampacity, or transformer kVA overload) and plot the hosting-capacity probability distribution.
5. **Mitigation Comparison:** Quantify and compare hosting-capacity gains across fixed PF, **AS/NZS 4777.2:2020** Volt-VAr/Volt-Watt curves, community battery absorption, and transformer tap adjustments.
6. **Protection Check:** Execute **IEC 60909** fault calculations at the LV busbar and remote feeder extremities against typical LVHRC fuse curves.

### Out-of-Scope (Stretch Goals — Only Attempted if Ahead of Schedule)
* Full 3-phase unbalanced sensitivity study across single-phase house service drops.
* Linear-programming (LP) optimal battery dispatch scheduling.
* Lightweight machine-learning add-on classifying the root cause of voltage violations (linking back to existing portfolio ML work).
* Rebuilding the 3-bus verification benchmark inside **DIgSILENT PowerFactory** (pending Windows/UWA lab access).

---

## 4. Deliverables

1. **Consultant-Style Technical Study Report (`report/`):** A **12-20 page engineering report** structured after industry grid-connection studies (DIgSILENT / GHD / Aurecon style), covering Scope, Network Assumptions, Methodology, Load-Flow & Hosting-Capacity Results, Mitigation Trade-Offs, Protection Check, Recommendations, and Appendices.
2. **Reproducible Codebase & Jupyter Notebooks (`src/` & `notebooks/`):** Clean, modular Python simulation engine and validation notebooks with exported publication-grade figures (`figures/`).
3. **Long-Form Technical Blog Post:** *"How much rooftop solar can a Perth street take?"* — published on [revantblogs.vercel.app](https://revantblogs.vercel.app/) and replacing the placeholder Smart Grid card on [revant-bisht.github.io](https://revant-bisht.github.io/projects.html).

---

## 5. Skills Highlighted & Industry Alignment

*Copied from `Projects.md` (§3) as per plan:*

| Skill category | Market rank / demand signal | How the project demonstrates it |
|---|---|---|
| **Technical Report Writing** | **#1 overall skill** | 12–20 page consultant-style study report with clear engineering recommendations |
| **Power-System Analysis** | **#6 overall skill** | Per-unit hand verification, QSTS load flow, voltage regulation, and thermal loading |
| **Power-System Software** | **+23 pts** *(largest jump from grad to junior ads)* | `pandapower` network modelling, time-series simulation, and IEC 60909 fault studies |
| **Protection & Fault Levels** | **+15 pts** | Transformer vs. feeder-end short-circuit current verification against LV fuse curves |
| **Python & Data Tooling** | **+13 pts** | Automated Monte-Carlo engine, `pandas` time-series pipelines, `matplotlib`, and Git |
| **Renewables, Batteries & DER** | **52% of power listings** | Rooftop PV hosting capacity, inverter control modes, and community battery sizing |
| **AS/NZS Standards** | **#8 overall skill** | **AS/NZS 4777.2:2020** (inverter grid connection) & **AS 61000.3.100** (steady-state voltage limits) |

**Target Employers:** Western Power, Horizon Power, Synergy, AEMO, DIgSILENT Pacific, GHD Energy, Aurecon, CEC/AEMO Grid-Connection Graduate Program, Transgrid, and Powerlink.

---

## 6. Repository Structure

```text
perth-lv-hosting-capacity/
├── README.md               # Project charter, methodology, and headline results
├── LOG.md                  # Daily engineering log (hours, milestones, blockers)
├── requirements.txt        # Pinned Python 3.11 package dependencies
├── LICENSE                 # MIT License
├── data/
│   ├── raw/                # Unmodified PVGIS solar irradiance & Ausgrid load datasets
│   └── processed/          # Cleaned, time-aligned 30-min profiles generated by code
├── notebooks/              # Step-by-step validation & exploratory Jupyter notebooks
├── src/                    # Reusable Python modules (network builder, Monte-Carlo, Volt-VAr)
│   └── __init__.py
├── figures/                # Study-grade plots and voltage heatmaps for report & portfolio
└── report/                 # 12–20 page consultant-style technical report & appendices
```

## 7. Environment Setup & How to Reproduce

This project uses **Python 3.11** (for full compatibility with `pandapower`, `scipy`, and `numba` on Apple Silicon macOS).

```bash
# 1. Clone the repository
git clone https://github.com/Revant-Bisht/perth-lv-hosting-capacity.git
cd perth-lv-hosting-capacity

# 2. Create and activate a Python 3.11 virtual environment
python3.11 -m venv .venv
source .venv/bin/activate

# 3. Install pinned dependencies
python -m pip install --upgrade pip setuptools wheel
python -m pip install -r requirements.txt

# 4. Verify pandapower installation
python -c "import pandapower as pp; print('pandapower version:', pp.__version__)"
```

---

## 8. Headline Results (To be populated by Day 28)

> *Resume / Portfolio Summary Target:*  

> *"Modelled a 55-house Perth-style LV feeder in Python (`pandapower`) with real solar and load data. Found that the baseline hosting capacity of **`[TBD]%`** rooftop-PV penetration rises to **`[TBD]%`** with AS/NZS 4777.2 Volt-VAr control and a **`[TBD]` kWh** community battery. Verified load flow against hand per-unit calculations (`<1%` error) and authored a consultant-style study report."*