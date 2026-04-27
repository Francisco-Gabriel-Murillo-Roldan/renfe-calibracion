# renfe-calibration
Reverse engineering applied to railway simulation. Analysis of adhesion, braking, and traction physics for 8 RENFE locomotive series through experimental testing.

# Diesel Locomotive Parametric Calibration Project for Open Rails

**Author:** Francisco Gabriel Murillo Roldán  
**Degree:** Mining Engineer  
**Methodology:** Teluroncio Method (empirical calibration through standardized testing)

## 📌 Project Overview

This repository documents a reverse engineering project applied to the **Open Rails** open-source train simulator.

A proprietary calibration method (the "Teluroncio Method") was developed to adjust the adhesion, braking, and traction physics of diesel locomotives, using **authentic technical documentation** (tractive effort curves, operator manuals, RENFE datasheets) and a rigorous **empirical testing plan** standardized on a 2% grade with 600 tons.

As a result, **8 locomotive series** (ALCO and General Motors) have been calibrated, spanning different technological generations from the 1960s to the 2000s.

## 🚂 Calibrated Locomotives

| RENFE Series | Manufacturer | Rail Power (kW) | Weight (t) | `C` (CK) | Speed (2%, 600 t) | Technology |
|--------------|--------------|-----------------|------------|----------|-------------------|------------|
| 1300 / 313 | ALCO | 900 | 84 | 0.102 | 31 km/h | Classic DC |
| 1600 / 316 | ALCO | 1120 | 109.5 | 0.15 | 21 km/h | Classic DC |
| 1800 / 318 | ALCO | 1200 | 96 | 0.122 | 42 km/h | Classic DC |
| 2100 / 321 | ALCO | 1330 | 102 | 0.102 | 31 km/h | Classic DC |
| 319.2 | GM / MACOSA | 1395 | 110 | 0.155 | 28 km/h | AC/DC, no wheel slip control |
| 319.3 | GM / MEINFESA | 1512 | 110 | 0.165 | 31 km/h | AC/DC, no wheel slip control |
| 319.4 | GM / MEINFESA | 1399 | 116 | 0.18 | 28 km/h | AC/DC, **with wheel slip control** |
| 333.3 | Vossloh / Alstom | 2237 | 120 | 0.16 | 40 km/h | **High power, freight** |
| 333.4 | Vossloh / Alstom | 2237 | 120 | 0.16 | 40 km/h | **High power, passenger** |

## 🎯 Key Technical Achievements

- **Development of a proprietary calibration method**: based on standardized testing and empirical result validation.
- **Reverse engineering of adhesion parameters**: identification of the `C` parameter's role as a "switch" for weather sensitivity and wheel slip control technology.
- **Brake calibration**: determination of the `ORTSMaxBrakeShoeForce` threshold at 20 kN per brake shoe, empirically validated to prevent wheel locking.
- **Log analysis and debugging**: using the Open Rails log as a diagnostic tool to correct braking and traction instabilities.
- **Modular configuration design**: centralization of parameters in `.inc` files, improving maintainability and scalability of the calibration.
- **Continuous dynamic brake control implementation**: achieving analog, fully independent control via `NumNotches ( 0 )`.

## 📁 Repository Structure

/renfe-calibracion
README.md # This file
  /Fichas-Tecnicas # Complete documentation for each series (8 PDFs)
  /Experimentos # Documented experiments (braking, adhesion, controls)
  /Parametros-Clave # Summary tables with final parameter values per series
  /OpenRails-Logs # Annotated "before and after" logs of corrections


## 🛠️ Methodology (Teluroncio Method)

1.  **Data extraction**: digitization of tractive effort and braking effort curves from original technical documentation.
2.  **Reference test**: locomotive + 600 tons on a 2% grade, measuring speed, effort, and power on the HUD.
3.  **Parameter adjustment**: iterative modification of `A`, `B`, `C` in the Curtius-Kniffler adhesion model until realistic behavior is achieved.
4.  **Weather validation**: repeating tests in rain (`D=0.7`) and snow (`D=0.4`) conditions to verify weather sensitivity.
5.  **Brake calibration**:
    *   Experimentation with 5 brake shoe types (`P6`, `P10`, `Composite`, `Disc_Pads`, `User_Defined`).
    *   Selection of `High_Friction_Composite` as the optimal material for modern locomotives.
    *   Determination of the 20 kN per shoe limit to avoid warnings and ensure controlled braking.
6.  **Documentation**: generation of complete technical datasheets and parameter tables for each locomotive.

## 📖 Repository Contents

| File / Folder | Description |
| :--- | :--- |
| **`/Fichas-Tecnicas/`** | Technical datasheets for 8 locomotives (ALCO 313, 318, 321; GM 319.2, 319.3, 319.4, 333.3, 333.4). |
| **`/Experimentos/`** | Documentation of key experiments: brake shoe test on 333.3, continuous dynamic brake implementation, adhesion adjustment on 319.4. |
| **`/Parametros-Clave/`** | Summary tables with final `MaxForce`, `MaxPower`, `C`, etc., values per series. |
| **`/OpenRails-Logs/`** | Annotated logs showing evolution from unstable configurations to the final calibration. |

## 📜 License

The content of this repository (documentation, analysis, experiments) is licensed under the **MIT License**.

The full license text is available in the [`LICENSE`](LICENSE) file of this repository.

## 🙏 Acknowledgments

This project was made possible by:

- The original technical documentation from ALCO, General Motors, RENFE, and Vossloh / Alstom.
- The Open Rails community (Elvas Tower and Coals to Newcastle forums).
- AI-assisted tools for technical support and documentation structuring.
- The GitHub platform for repository management and dissemination.

---
*Last updated: April 2026*
