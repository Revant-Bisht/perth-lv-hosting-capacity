# Perth LV Feeder Rooftop-Solar Hosting Capacity & Community Battery Study

Power-flow, Monte-Carlo hosting-capacity, and short-circuit study of a 55-customer residential low-voltage (LV) feeder under Perth solar conditions, built in Python with `pandapower`.

**Author:** Revant Bisht - BEng (Hons) Electrical & Electronic Engineering, University of Western Australia  
**Portfolio Links:** [revant-bisht.github.io](https://revant-bisht.github.io/) · [Technical Blog](https://revantblogs.vercel.app/)
**Status:** Work in progress (target `v1.0`: Dec 2026)

---

## 1. Overview

High residential rooftop PV penetration on Western Australia's South West Interconnected System (SWIS) causes reverse power flow during midday minimum-load periods, pushing LV street voltages past statutory limits and overloading distribution assets.

This repository models a Perth-parameterised LV residential feeder to quantify:
1. **Baseline hosting capacity** before steady-state voltage or thermal limits are breached.
2. **Hosting-capacity gains** from four mitigations: fixed power factor, **AS/NZS 4777.2:2020** Volt-VAr / Volt-Watt inverter response, off-load transformer tap adjustment, and a community-scale battery.
3. **Fault-level adequacy** under **IEC 60909** short-circuit analysis at the distribution transformer LV busbar and remote feeder extremities.

---

## 2. Hosting-Capacity definition
**Hosting capacity** is defined in this study as the maximum rooftop PV penetration (% of customers with a 5 kW system, reported alongside aggregate kW per customer) at which **fewer than 5% of stochastic Monte-Carlo allocations** violate any of the following steady-state constraints under a worst-case midday snapshot:

* **Upper voltage limit:** $V \le 1.10\text{ p.u.}$ ($253.0\text{ V}$ phase-to-neutral on a $230\text{ V}$ base)
* **Lower voltage limit:** $V \ge 0.94\text{ p.u.}$ ($216.2\text{ V}$)
* **Thermal limits:** Cable and distribution transformer loading $\le 100\%$ of rated ampacity / kVA

---

## 3. Scope

### In Scope (`v1.0`)
* **Hand-calculation benchmark:** 3-bus per-unit load flow solved by hand and cross-checked against `pandapower` ($<1\%$ voltage and power-flow error).
* **LV Feeder network model:** IEEE European LV Test Feeder (906 buses, 55 residential loads) re-parameterised to a $22\text{ kV}/400\text{ V}$ ($50\text{ Hz}$) Australian distribution feeder.
* **Representative-day time-series load flow:** 2–3 representative operating days (e.g., high-PV/low-load mild day, summer peak, winter day) combining Perth PVGIS irradiance with Ausgrid residential load profiles.
* **Snapshot Monte-Carlo hosting capacity:** Stochastic PV placement from $0\%$ to $100\%$ penetration at a worst-case midday snapshot (minimum daytime load, peak solar output), recording the first binding constraint.
* **Mitigation comparison:** Fixed power factor, AS/NZS 4777.2:2020 Volt-VAr and Volt-Watt curves, transformer tap adjustment, and a community-scale battery sized and sited for midday export absorption.
* **Short-circuit / protection check:** IEC 60909 three-phase fault currents ($I_k''$) at the LV busbar and feeder end compared against typical LV HRC fuse curves (single-phase-to-earth faults included only if valid zero-sequence line parameters are available).
* **Technical report:** 12–20 page study report (`report/`) documenting assumptions, methodology, results, and limitations.

### Out of Scope / Stretch Goals
* Full three-phase unbalanced load flow and voltage unbalance factor (VUF) assessment.
* Full-year / seasonal Monte-Carlo time-series simulation.
* Linear-programming optimal battery dispatch.
* DIgSILENT PowerFactory replication of the 3-bus benchmark.

---

## 4. Assumptions & Limitations

* **Nominal voltage & statutory limits:** Western Power's existing Technical Rules specify $240/415\text{ V} \pm 6\%$ ($225.6\text{–}254.4\text{ V}$), while **AS 61000.3.100** / **AS IEC 60038** specify $230/400\text{ V} +10\%/-6\%$ ($216.2\text{–}253.0\text{ V}$), toward which WA's Technical Rules have been migrating. This study adopts **$230/400\text{ V}$ ($216.2\text{–}253.0\text{ V}$)** as the primary limit because $253.0\text{ V}$ is marginally more conservative than $254.4\text{ V}$ and aligns directly with AS/NZS 4777.2:2020 inverter control setpoints.
* **Load vs. solar data sources:** The Ausgrid Solar Home dataset supplies **Sydney residential load shapes only** (used as a proxy for Australian household demand); **PVGIS** supplies **Perth** solar irradiance and PV output profiles.
* **Balanced baseline:** Baseline load flow and Monte-Carlo runs assume balanced three-phase loading. Single-phase customer clustering can produce higher local voltage rise than a balanced model predicts.
* **Snapshot Monte-Carlo:** Hosting-capacity curves evaluate a worst-case coincident minimum-load / peak-irradiance snapshot rather than full-year time series; time-of-day battery state-of-charge effects are evaluated separately in the representative-day simulations.

---

## 5. Validation Criteria

* [ ] 3-bus `pandapower` load flow matches hand per-unit calculations within $<1\%$ across bus voltage magnitudes, angles, and branch flows.
* [ ] 100% power-flow convergence across all representative-day timesteps and Monte-Carlo trials.
* [ ] All four mitigation cases compared on an identical feeder topology, seed set, and constraint definition.
* [ ] IEC 60909 prospective fault currents verified against standard LV fuse time-current characteristics.

---

## 6. Key Results

*Results will be populated upon completion of `v1.0`.*

| Scenario | Hosting Capacity (% Customers) | Hosting Capacity (kW / Customer) | First Binding Constraint | Max Feeder Voltage (V) |
|---|---|---|---|---|
| Baseline (Unity PF) | TBD | TBD | TBD | TBD |
| Fixed PF (0.95 inductive) | TBD | TBD | TBD | TBD |
| AS/NZS 4777.2 Volt-VAr + Volt-Watt | TBD | TBD | TBD | TBD |
| Transformer Off-Load Tap (−2.5% / −5%) | TBD | TBD | TBD | TBD |
| Community-Scale Battery (TBD kWh) | TBD | TBD | TBD | TBD |
---

## 7. Repository Structure

```text
perth-lv-hosting-capacity/
├── README.md               # Project scope, assumptions, setup, and results
├── LOG.md                  # Working log of progress and decisions
├── requirements.txt        # Pinned Python 3.11 dependencies
├── LICENSE                 # MIT License
├── data/
│   ├── raw/                # Unmodified PVGIS and Ausgrid source files
│   └── processed/          # Cleaned 30-minute load and PV profiles
├── notebooks/              # Validation and exploratory Jupyter notebooks
├── src/                    # Simulation modules (network, Monte-Carlo, mitigations)
│   └── __init__.py
├── figures/                # Exported plots for the report
└── report/                 # Technical study report and appendices
```

## 8. Environment setup and How to reproduce

Requires **Python 3.11** (for full compatibility with `pandapower` and `numba` on Apple Silicon macOS).

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

### How to Run
```bash
# 1. Run 3-bus hand-calculation validation (notebook)
jupyter notebook notebooks/01_three_bus_validation.ipynb

# 2. Run representative-day time-series and Monte-Carlo studies (planned)
# python -m src.run_timeseries
# python -m src.run_hosting_capacity
# python -m src.run_short_circuit
```

---

## 9. References
1. AS 61000.3.100 — Electromagnetic compatibility (EMC): Steady state voltage limits in public electricity systems.AS/NZS 4777.
2. 2:2020 — Grid connection of energy systems via inverters, Part 2: Inverter requirements.
3. Western Power — Technical Rules (LV nominal voltage $240/415\text{ V} \pm 6\%$ and ERA Technical Rules alignment proposals).
4. IEC 60909-0 — Short-circuit currents in three-phase AC systems.
5. IEEE PES Distribution System Analysis Subcommittee — European Low Voltage Test Feeder (bundled via pandapower.networks).
6. PVGIS (Joint Research Centre, European Commission) — Solar radiation and PV performance API for Perth, WA.
7. Ausgrid — Solar Home Electricity Data (300 residential customer 30-minute demand profiles).