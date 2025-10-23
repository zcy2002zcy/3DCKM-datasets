# 🧱 3DCKM Dataset

**3DCKM (3D Channel Knowledge Map)** is a large-scale, multiband, three-dimensional channel knowledge dataset designed for **Low-Altitude Corridor (LAC)** communication research.  
It comprehensively models the interaction between wireless propagation and complex urban environments, providing multi-frequency, altitude-dependent, and environment-aligned channel data.  
3DCKM bridges the gap between traditional ground-based channel datasets and the need for accurate 3D air–ground propagation modeling in future 6G networks.

Low-Altitude Corridors (LACs) are regulated airspaces that enable the dense operation of unmanned aerial vehicles (UAVs) and are emerging as a critical component of beyond-5G and 6G networks.  
Unlike terrestrial communication, LAC links must cope with truly **three-dimensional air–ground propagation**, rapidly varying line-of-sight (LoS) conditions, and altitude-dependent blockage and scattering.  
These factors create highly variable and geometry-sensitive channel conditions, emphasizing the necessity for **3D Channel Knowledge Map (CKM)** modeling.

To address this need, **3DCKM** provides a unified framework that integrates **channel data and environmental information** across multiple frequency bands — from sub-6 GHz to millimeter-wave — under realistic urban scenarios.  
Each dataset tile captures how radio waves interact with buildings, vegetation, and terrain, offering detailed information such as path loss, delay, angle of arrival/departure (AoA/AoD), zenith angles (ZoA/ZoD), and multipath components.  
Environmental layers, including building height, base station layout, and vegetation distribution, are spatially aligned with the channel matrices to form a consistent 3D knowledge representation.

---

## 🌍 Key Features

- **Multiband Coverage** – five representative frequencies: 900 MHz, 2.6 GHz, 3.5 GHz, 5 GHz, and 41 GHz  
- **Altitude-Dependent Sampling** – 20 altitude layers (1–100 m)  
- **High Spatial Resolution** – 1 m resolution over 820 tiles (256 m × 256 m each)  
- **Environment–Channel Integration** – building, vegetation, and base-station maps aligned with channel data  
- **Comprehensive Channel Parameters** – path loss, delay, AoA/AoD, ZoA/ZoD, and multipath components

---

## 🧩 Data Description

### 🔹 `data/npz`

This directory contains six `.npz` matrices that represent **core channel propagation features**.  
Each matrix is stored in NumPy’s compressed format and corresponds to one physical characteristic of the radio link.

- **`passloss.npz`** — spatial distribution of path loss (dB), capturing large-scale attenuation caused by distance, obstruction, and scattering.  
- **`delay.npz`** — multipath delay spread or individual path delay, representing the temporal dispersion of the received signal.  
- **`aoa.npz`** — azimuth of arrival angles, describing the horizontal incident direction of multipath components at the receiver.  
- **`aod.npz`** — azimuth of departure angles, defining the horizontal emission direction at the transmitter.  
- **`zoa.npz`** — zenith of arrival, representing the vertical arrival direction (elevation angle) of incoming signals.  
- **`zod.npz`** — zenith of departure, corresponding to the elevation direction of transmitted multipath components.

All matrices share the **same spatial resolution and coordinate alignment**, meaning that each cell (x, y) refers to the **same geographic location** across all six parameters.  
This allows direct combination or stacking for **machine learning, channel prediction, or spatial correlation studies**.

---

### 🔹 `data/txt`

This folder stores **detailed multipath propagation logs**, each in plain text format.  
Every `.txt` file corresponds to a **specific simulation case, region, and transmitter–receiver configuration**.

Each record typically includes:
- Path index  
- Delay (ns)  
- Power (dB)  
- Angle of arrival and departure (azimuth and zenith)  
- Polarization or phase information (if available)

These files serve as **ground truth references** for reconstructing or validating high-resolution channel impulse responses.  
They can also be used to generate **statistical channel models**, train **neural network–based channel predictors**, or analyze **multipath geometry**.

---

### 🔹 `environment/`

This folder contains **environmental feature maps** in `.npz` format.  
They are spatially aligned with the channel data (`data/npz`) to enable consistent 3D environment–channel correlation analysis.  
Each matrix represents a physical layer of the environment influencing wireless propagation.

- **`building.npz`**  
  - Encodes **building height distribution**.  
  - `0` = open area (no building).  
  - Positive integers represent building height (in meters or relative scale).  
  - Useful for identifying shadowing regions and urban canyon effects.

- **`bs.npz`**  
  - Marks **base station (BS) locations** and indices.  
  - `0` = open area (no BS).  
  - `1, 2, 3, …` = BS index within that region (corresponds to transmitter ID in `data/`).  
  - If multiple BSs overlap, the **highest index** is displayed (representing the dominant transmitter).

- **`tree.npz`**  
  - Represents **vegetation and green areas**.  
  - `0` = no vegetation.  
  - `1` = presence of trees or green cover.  
  - Provides information for modeling additional attenuation and scattering effects caused by foliage.

---

### 🔹 Integration Between `data` and `environment`

All environmental matrices are **co-registered** with the propagation data grids — each pixel corresponds to a **1 m × 1 m** ground cell.  
Thus, for any spatial coordinate `(x, y)`, you can directly retrieve:
- Path loss, delay, and angular parameters from `data/npz`.
- Terrain and structural information from `environment/`.

This tight coupling enables **joint environment-aware learning**, such as:
- Predicting channel parameters from environment maps.
- Studying propagation variation across different heights or frequency bands.
- Generating synthetic channel knowledge maps using AI-based methods.

---


---

## 🗂️ File Naming Convention

All files follow a unified naming rule to encode spatial region, transmitter, height, and frequency information, ensuring consistent organization across the dataset.

**Format**

**Example**

**Explanation**

| Field | Meaning | Example | Notes |
|:------|:--------|:--------|:-----|
| `R001` | Region index | `R001` | The 1st 256×256 m tile |
| `T01`  | Transmitter index | `T01` | The 1st base station in that region |
| `h01`  | Height layer | `h01` | Altitude index; `01` = 1 m, …, `20` = 100 m |
| `f03`  | Frequency index | `f03` | `f01`=900 MHz, `f02`=2.6 GHz, `f03`=3.5 GHz, `f04`=5 GHz, `f05`=41 GHz |

> Example interpretation: `R001_T01_h01_f03.npz` → region 1, transmitter 1, altitude 1 m, frequency 3.5 GHz.

---

## 📊 Dataset Attributes

| Attribute | Value |
|------------|------|
| **Total Size** | ~43 GB |
| **Spatial Resolution** | 1 m |
| **Area per Tile** | 256 × 256 m |
| **Height Layers** | 20 (1–100 m) |
| **Frequencies** | 900 MHz, 2.6 GHz, 3.5 GHz, 5 GHz, 41 GHz |
| **Total Tiles** | 820 (urban Beijing) |
| **Transmitters** | ~2000 |
| **Material Types** | Concrete, Soil, Vegetation |
| **Data Format** | `.npz` (NumPy arrays), `.txt` (text logs) |

---

## 🧠 Applications

- 3D Channel Knowledge Map construction & prediction  
- AI-driven channel modeling and completion  
- Cross-frequency correlation and transfer learning  
- Low-altitude corridor network design (UAV coverage, interference)  
- Propagation behavior analysis across heights and environments

---

## 📥 Download

The dataset is hosted on **Google Drive** for public access:

🔗 **[👉 Download 3DCKM Dataset (Google Drive)](https://drive.google.com/drive/folders/1qVRvR847noCDPFYs3oZ7_vBibmiLEwc1?usp=sharing)**

> ⚠️ Please download all files before using.  
> It is recommended to use “Download all” or a stable downloader for large files.

---



