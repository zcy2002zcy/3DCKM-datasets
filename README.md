# 🧱 3DCKM Dataset

**3DCKM (3D Construction Knowledge Model)** is a large-scale 3D channel modeling and environmental dataset designed for wireless communication research.  
It provides spatially aligned channel matrices, multipath information, and environmental maps for 3D propagation modeling, AI-based prediction, and digital twin applications.  
This dataset is freely available for research and educational use.

---

## 📦 Dataset Overview

| Item | Description |
|------|--------------|
| **Name** | 3DCKM Dataset |
| **Version** | 1.0 |
| **Total Size** | ~43 GB |
| **Format** | `.npz`, `.txt` |
| **License** | CC BY-NC 4.0 (Non-Commercial Use) |
| **Host Platform** | Google Drive |
| **Author** | zcy2002zcy |

---

## 📥 Download

The dataset is hosted on **Google Drive** for public access:

🔗 **[👉 Download 3DCKM Dataset (Google Drive)](https://drive.google.com/drive/folders/1qVRvR847noCDPFYs3oZ7_vBibmiLEwc1?usp=sharing)**

> ⚠️ Please download all files before using.  
> It is recommended to use “Download all” or a stable downloader for large files.

---

## 📂 Directory Structure


---

## 🧩 Data Description

### 🔹 `data/npz`
This folder stores six `.npz` files representing key wireless channel characteristics:
- **passloss** — path loss matrix  
- **zod / zoa** — zenith of departure and arrival  
- **aoa / aod** — azimuth of arrival and departure  
- **delay** — multipath delay spread or per-path delay  

All matrices share the same spatial dimensions, making them directly comparable for machine learning or propagation analysis.

### 🔹 `data/txt`
Contains detailed **multipath propagation information** for each simulation or measurement case.

### 🔹 `environment/`
This folder contains `.npz` matrices describing the physical environment that affects radio propagation:
- **building.npz** — 0 means open space; other values represent building height.  
- **bs.npz** — 0 means open space; numbers (1, 2, 3, …) represent base stations corresponding to entries in the `data` folder. If multiple stations overlap, the highest index is shown.  
- **tree.npz** — 0 means open space; 1 indicates vegetation or green area.

---

## 🧪 Example Usage

```python
import numpy as np
import matplotlib.pyplot as plt

# Load path loss matrix
pl = np.load('data/npz/passloss.npz')['arr_0']

# Load building height map
building = np.load('environment/building.npz')['arr_0']

# Visualize path loss
plt.imshow(pl, cmap='hot')
plt.title("Pathloss Heatmap")
plt.colorbar(label="Loss (dB)")
plt.show()

# Compute mean path loss in open areas
mean_open = pl[building == 0].mean()
print("Mean Pathloss in Open Area:", mean_open)
@dataset{3dckm_2025,
  title={3DCKM: 3D Construction Knowledge Model Dataset},
  author={zcy2002zcy},
  year={2025},
  url={https://drive.google.com/drive/folders/1qVRvR847noCDPFYs3oZ7_vBibmiLEwc1?usp=sharing}
}
