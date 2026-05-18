# Atmospheric-mmWave-Energy-Harvesting-with-Rectenna-Validation-
Sameeksha R | B.Tech ECE, VIT Chennai · B.S. Electronic Systems, IIT Madras
Feasibility study, parametric simulation, and power management validation for RF energy harvesting across the 57–325 GHz mmWave spectrum.


Overview
This repository contains the complete simulation codebase for the project:
"Atmospheric mmWave Energy Harvesting with Rectenna Validation"
The work conducts a physics-based feasibility study of harvesting ambient/dedicated RF energy from atmospheric mmWave sources, with analysis of:

Absorption loss using HITRAN spectroscopic data + Beer-Lambert propagation
Humidity sensitivity across the full RH range (5–95%)
Frequency–distance heat maps identifying viable harvest zones
End-to-end power management chain validation for ultra-low-power IoT


Key Findings
FrequencyRangeUse CaseVerdict60 GHz> 80 mLong-range IoT, O₂ window✓ Stable, humidity-independent94 GHz30–80 mW-band mid-range✓ Good window140 GHz20–50 mD-band✓ Moderate183 GHz< 20 mShort-range, H₂O line◑ High power, humidity-sensitive325 GHz< 5 mVery short✗ Impractical beyond 5 m

Harvestable power: 50–500 nW at 5–20 m range (verified via rectenna model)
Humidity variation: 5–10× power swing across 20–95% RH at 183 GHz
IoT viability: Duty-cycled sensor node (~3 µW avg) sustained by 183 GHz harvest at ≤ 10 m


Repository Structure
mmwave_harvesting/
├── python/
│   ├── feasibility_mapping.py      # Beer-Lambert + HITRAN absorption, feasibility table
│   ├── humidity_sensitivity.py     # Parametric RH sweep, seasonal comparison
│   ├── heatmap_power.py            # 2D frequency–distance heat maps
│   ├── power_management.py         # Rectenna, MPPT, storage, IoT load simulation
│   └── run_all.py                  # ← Run this to generate all outputs
├── matlab/
│   ├── feasibility_mapping.m       # Absorption spectrum + power vs distance
│   ├── humidity_heatmap.m          # Humidity sensitivity + heat maps
│   └── power_management.m         # Rectenna efficiency + storage simulation
├── outputs/                        # Generated figures (auto-created)
└── README.md

Physics Model
Atmospheric Absorption
Total absorption coefficient (dB/km) is computed as:
α(f) = α_dry(f) + Σ_k [ L_k(f) × scale_k ]
Where:

α_dry = 0.005 × (f/100)² — non-resonant dry-air continuum (Van Vleck–Weisskopf)
L_k(f) = Lorentzian line profile (pressure-broadened, collisional regime)
scale_k = humidity scaling for H₂O lines (ρ_w / ρ_std from Magnus formula)

Spectral lines sourced from HITRAN database, cross-referenced with ITU-R P.676-12.
Beer-Lambert Propagation
L_atm(dB) = α(dB/km) × d(km)
Total Link Budget
P_rx(dBm) = P_tx + G_tx + G_rx - FSPL - L_atm
FSPL(dB)  = 20·log10(4πdf/c)
Power Management Chain
P_harvest(nW) = P_rx(mW) × η_rect(P_rx)       [Schottky Gaussian model]
P_MPPT(nW)   = P_harvest × η_MPPT             [fractional OCV, η = 88%]
ΔV_cap       = (P_net × Δt) / (C × V_cap)     [supercapacitor dynamics]

Getting Started
Python
Requirements:
bashpip install numpy matplotlib
Run everything:
bashcd python/
python run_all.py
Run individual modules:
bashpython feasibility_mapping.py    # absorption spectrum + power vs distance
python humidity_sensitivity.py   # RH parametric sweep
python heatmap_power.py          # 2D heat maps (takes ~30 s)
python power_management.py       # rectenna + storage validation
MATLAB
Open MATLAB, navigate to matlab/, then run:
matlabfeasibility_mapping    % absorption spectrum
humidity_heatmap       % humidity sensitivity + heat maps
power_management       % rectenna + storage simulation
All outputs are saved to ../outputs/.

Tested on MATLAB R2021b+. Uses only base MATLAB (no toolboxes required).


Generated Outputs
FileDescriptionabsorption_spectrum.png57–325 GHz α(f) with harvest window overlaypower_vs_distance.pngP_rx vs distance for 5 harvest frequencieshumidity_power_sensitivity.pngPower & harvestable nW vs RH (5–95%)183ghz_humidity_distances.png183 GHz sensitivity at multiple distancesseasonal_rh_comparison.pngWinter/Summer/Monsoon harvest potentialheatmap_full.png2D harvest map (57–325 GHz, 1–100 m)heatmap_shortrange.pngZoomed 1–30 m map + viable zone maskheatmap_rh_comparison.pngSide-by-side RH=20/60/90% mapsrectenna_efficiency.pngSchottky efficiency curve + DC outputpower_budget.pngRF→Rectenna→MPPT→Load chain budgetstorage_simulation.pngSupercapacitor V(t) + power balance(MATLAB versions prefix with matlab_)

IoT Load Reference
Simulated duty-cycled sensor node (STM32 @ low-power mode):
PhasePowerDurationSense50 µW10 msProcess200 µW5 msTransmit (BLE)2000 µW2 msSleep (RTC)1 µW1000 msAverage~3 µW1017 ms cycle

Related Work

Publication (Under Review): Hybrid OFDMA-mmWave Framework Design for Industrial Robotic Communication — MIMO-OFDMA, 5G NR channel modelling
Research Collaboration: NAIST Japan — Underwater Hybrid Optical-Acoustic Wireless Communication


References

ITU-R P.676-12 — Attenuation by Atmospheric Gases
HITRAN 2020 Database — Harvard-Smithsonian Center for Astrophysics
Rosenkranz, P.W. — Water Vapor Microwave Continuum Absorption, Radio Science, 1998
Popović, Z. et al. — Microwave Energy Harvesting, IEEE Transactions on Microwave Theory and Techniques, 2013
Visser, H.J. & Vullers, R.J.M. — RF Energy Harvesting and Transport for Wireless Sensor Networks, Proceedings of the IEEE, 2013
