## IIoT-Based Precision Fertigation System with Dual-Path Fuzzy Control and Leachate Management for Hydroponic Melon Cultivation

> Published in: *2026 International Conference on Information Technology and Computing (ICITCOM)*

---

## Authors & Affiliations

| # | Name | Affiliation |
|---|------|-------------|
| 1 | Cisa Valentino Cahya Ramadhani | Department of Information Technology, Faculty of Informatics, Telkom University Surabaya Campus, Indonesia |
| 2 | Helmy Widyantara | Department of Information Technology, Faculty of Informatics, Telkom University Surabaya Campus, Indonesia |
| 3 | Muhammad Adib Kamali | Department of Information Technology, Faculty of Informatics, Telkom University Surabaya Campus, Indonesia |
| 4 | Michael Angello Qadosy Riyadi | Department of Information Technology, Faculty of Informatics, Telkom University Surabaya Campus, Indonesia |

---

## Background

Hydroponic melon cultivation demands simultaneous and continuous regulation of electrical conductivity (EC), substrate moisture (VWC), and drainage leachate — yet prior automated approaches address each in isolation. IoT-based EC threshold control, Fuzzy-PID regulation, Mamdani fuzzy irrigation, and threshold-based leachate recirculation have each been demonstrated separately, but no unified real-time platform integrates all three within a single IIoT edge system for melon cultivation, leaving a clear gap in closed-loop precision fertigation design.

---

## Objectives

1. Design and bench-validate an IIoT-based precision fertigation system on a single Raspberry Pi 4 Model B edge controller.
2. Implement a growth-stage-aware nutrient dosing algorithm (HST) with individually calibrated EC-slope coefficients per nutrient (N, P, K).
3. Deploy a dual-path Mamdani FIS for simultaneous fertigation dose (GDF) and irrigation duration (WD) control.
4. Integrate a rule-based leachate recycling/discharge protocol within the same platform.

---

## Methods

The system is organized into four functional layers: **sensing**, **communication**, **edge control**, and **actuation**.

- **Sensing:** Three NPK 7-in-1 sensors (RS485/Modbus RTU at 1 Hz) at the mixing tank, growing medium, and leachate tank, measuring EC, pH, VWC, temperature, and N/P/K concentrations.
- **Calibration:** Each stock solution (N, P, K) individually calibrated via rolling-window CV filtering (w = 5, CV < 1%) and OLS regression to obtain EC-slope coefficients (b_N, b_P, b_K).
- **Nutrient Dosing (HST):** Per-nutrient pump volumes computed from EC deficit and N:P:K ratios per growth phase (Vegetative DAT 1–30, Flowering DAT 31–50, Ripening DAT 51–85).
- **Dual-Path Mamdani FIS:**
  - *Fertigation path:* EC error → Global Dose Factor (GDF, range 0–1.5)
  - *Irrigation path:* VWC → Watering Duration (WD, 0–270 s)
  - Defuzzification via centroid of area (COA); MIN antecedent, MAX aggregation.
- **Leachate Management:** ΔEC threshold (0.5 mS/cm) determines recirculation (≤20% batch) or discharge via Pump-Drain.
- **Edge Stack:** Raspberry Pi 4 Model B, Python 3.11, pymodbus, scikit-fuzzy, Flask, SQLite; five DC pumps via Remote I/O Modbus TCP relay.

---

## Results & Findings

| Test Aspect | Target | Result |
|-------------|--------|--------|
| Sensor communication | 100% success | 100% (3/3 units, 100 reads) |
| Actuator control | 100% success | 100% (5/5 units, 30 trials) |
| Network latency | < 100 ms | 15–16 ms |
| Stock calibration R² | > 0.95 | 0.9777–0.9879 |
| Dosing volume error | < ±5% | ±3.2% per pump |
| EC mixing accuracy | < ±5% | ±1.9% |
| Fuzzy GDF output | Proportional | 0.00–1.30 |
| Fuzzy WD output | Proportional | 0–270 s |
| Leachate decision | 100% correct | 3/3 correct |

OLS calibration yielded slopes b_N = 13.33, b_P = 6.44, b_K = 9.31 µS/mL with R² of 0.9872, 0.9777, and 0.9879 respectively. ~62% of intermediate sensor readings were rejected by the CV filter, confirming its necessity. All leachate decisions (recycle, recycle+correction, purge) matched rule-defined expectations across all three boundary test cases.

---

## Conclusion

The system demonstrates feasibility of co-implementing growth-stage nutrient dosing, dual-path fuzzy control, and leachate management on a single low-cost Raspberry Pi 4 IIoT platform. All bench-level functional targets were met: EC mixing within 1.9%, dosing error within 3.2%, FIS proportional response across all membership functions, and 100% communication reliability at 15–16 ms latency. The primary contribution is systems integration — unifying previously isolated subsystems — with the rolling-window CV calibration procedure as an additional procedural contribution. Validation is bench-level only; future work should conduct full-season greenhouse trials, add closed-loop pump calibration, incorporate pH as a fuzzy input, and benchmark against PID and fixed-schedule baselines.

---

## Citation

### BibTeX — Paper

```bibtex
@inproceedings{ramadhani2026iiot,
  title     = {IIoT-Based Precision Fertigation System with Dual-Path Fuzzy 
               Control and Leachate Management for Hydroponic Melon Cultivation},
  author    = {Ramadhani, Cisa Valentino Cahya and Widyantara, Helmy and 
               Kamali, Muhammad Adib and Riyadi, Michael Angello Qadosy},
  booktitle = {2026 International Conference on Information Technology 
               and Computing (ICITCOM)},
  year      = {2026},
  publisher = {IEEE}
}
```

### BibTeX — GitHub Repository

```bibtex
@misc{ramadhani2026iiot_github,
  author       = {Ramadhani, Cisa Valentino Cahya and Widyantara, Helmy and 
                  Kamali, Muhammad Adib and Riyadi, Michael Angello Qadosy},
  title        = {IIoT-Based Precision Fertigation System with Dual-Path Fuzzy 
                  Control and Leachate Management for Hydroponic Melon Cultivation},
  year         = {2026},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/[username]/iiot-hydroponic-melon-fertigation-fuzzy-leachate-ICITCOM2026}},
  note         = {Source code and bench validation for IIoT precision fertigation 
                  system with Mamdani FIS and leachate management}
}
```
