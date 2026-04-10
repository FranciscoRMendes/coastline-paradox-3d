# 3D Version of the Coastline Paradox


> *Does a hilly city have more surface area than a flat one?*

A Jupyter notebook that answers this question by extending the classic **coastline paradox** into three dimensions — from a fractal curve, to a synthetic terrain, to real LiDAR elevation data from Telegraph Hill, San Francisco.

**Blog post:** [Telegraph Hill and the Coastline Paradox (3D Extension)](https://franciscormendes.github.io/2025/12/16/3d-coastline-paradox/)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/FranciscoRMendes/coastline-paradox-3d/blob/main/coastline_paradox.ipynb)

---

## The idea

The coastline paradox says: the measured length of a coastline depends on the length of your ruler. Use a shorter ruler and you pick up more detail — bays, inlets, rocks — and the measured length grows without bound. There is no single "true" length.

This notebook asks whether the same applies to surface area. Measure Telegraph Hill with a 256 m ruler, then a 32 m one. As the ruler shrinks and begins to resolve small ridges, gullies, and local slope variations, the measured area grows. The hill doesn't change — the measurement does.

The scale-invariant quantity that describes *how fast* length or area grows as the ruler shrinks is the **fractal dimension** D, extracted from the slope of a log-log plot:

```
L(ε) ~ ε^(1−D)     (1D coastline,  D > 1)
A(ε) ~ ε^(2−D)     (2D surface,    D > 2)
```

---

## Sections

| # | Topic | Method | Key result |
|---|-------|--------|-----------|
| 1 | **Koch curve** | Ruler method on a self-similar fractal | D ≈ 1.10 (theory: 1.26) |
| 2 | **Synthetic terrain** | Diamond-square algorithm; surface area at multiple pixel scales | D ≈ 2.3–2.5 |
| 3 | **Telegraph Hill DEM** | 500 m × 500 m USGS LiDAR at 0.25 m/px; ruler sizes 32–256 m | D ≈ 2.00084 |
| 4 | **Fractal dimension** | Log-log regression across all three cases | — |

The Telegraph Hill fractal dimension (D ≈ 2.00084) sits just above 2 — the terrain is relatively smooth at these scales — but the systematic increase in measured area as the ruler shrinks is unambiguous. The 3D coastline paradox holds, even for a modest urban hill.

---

## Implementation notes

- **Surface area computation** uses triangular tessellation (Heron's formula). Each ruler-size square is split into two triangles; all coordinates are in physical metres so cross-terms between horizontal and vertical distances are correct.
- **Synthetic surface** heights are normalised to match the pixel scale before area measurement. Without this, z ≪ x/y makes the surface trivially flat (D ≈ 2.00002).
- **Ruler sizes** for the DEM are [256, 128, 64, 32] m — all within the 500 m × 500 m DEM extent.
- Random seed is fixed (`np.random.seed(42)`) so synthetic results are reproducible.

---

## Repository structure

```
coastline_paradox.ipynb                       # Main notebook (runs on Colab)
USGS_OPR_CA_SanFrancisco_B23_05150295.tif    # Telegraph Hill DEM (~12 MB)
requirements.txt                              # Python dependencies
```

---

## Setup

```bash
pip install -r requirements.txt
jupyter notebook coastline_paradox.ipynb
```

Or open directly in [Google Colab](https://colab.research.google.com/github/FranciscoRMendes/coastline-paradox-3d/blob/main/coastline_paradox.ipynb) — no local install needed.

**Dependencies:** `numpy` · `matplotlib` · `scipy` · `rasterio` · `pyproj` · `folium`

---

## Data

`USGS_OPR_CA_SanFrancisco_B23_05150295.tif` — USGS LiDAR-derived DEM, Telegraph Hill area, San Francisco.

| Property | Value |
|----------|-------|
| Resolution | 0.25 m/pixel |
| Extent | 2000 × 2000 px (≈ 500 m × 500 m) |
| Bounds | 37.7995°N – 37.8040°N, 122.4103°W – 122.4046°W |
| CRS | UTM (projected) |
| Format | GeoTIFF, 32-bit float, LZW-compressed |

---

## License

GNU General Public License v3 — see [LICENSE](LICENSE).
