# Coin Detection and Counting System 🪙

A highly robust, universal Computer Vision pipeline designed to detect, segment, and count overlapping and boundary-clipped coins on highly textured surfaces without relying on specific currency assumptions uploaded:CVProject.ipynb.

---

## 👥 Project Team (Team 15)

This project was designed and implemented by:

| Name | Student ID |
| :--- | :--- |
| **Ahmed H. Khalil** | `22P0226` |
| **Youssef M. Elshorbagy** | `22P0047` |
| **Karim Hatem** | `22P0153` |
| **Mostafa Foad** | `22P0077` |
| **AbdelRahman Mostafa** | `22P0053` |

---

## 🔍 Overview

Detecting metallic coins on real-world surfaces presents significant traditional Computer Vision challenges: highly reflective specular highlights break edge continuity, tight clustering causes adjacent borders to merge, and background textures (such as wood grain or table scratches) induce false positive detections.

This system solves these issues through a **Multi-Pass Standardized Pipeline** driven by the **Circular Hough Transform** uploaded:CVProject.ipynb. By decoupling image scale from physical parameters and introducing seamless border replication, the model successfully detects fully isolated, deeply occluded, and boundary-clipped coins with near-perfect accuracy.

---

## ✨ Core Features & Architecture

### 1. Aspect-Preserving Standardization & Replicated Padding
* **Consistent Parameter Scaling:** Input images of arbitrary resolutions are dynamically scaled to a standardized width ($800\text{ px}$) while preserving aspect ratios uploaded:CVProject.ipynb. This ensures physical thresholds (`minRadius`, `maxRadius`, `minDist`) behave predictably across diverse camera shots.
* **Boundary Coin Recovery:** Uses `cv2.BORDER_REPLICATE` to add seamless border padding around the canvas. Stretching the outermost pixels eliminates false, sharp border gradients while providing complete unclipped arcs for coins sitting directly on the image edge.

### 2. Multi-Pass Hough Validation
Instead of relying on a single compromised parameter set, the algorithm executes three targeted sweeps over an aggressively median-filtered canvas (`ksize=13`) that dissolves background wood grain:
* **Pass 1 (Dominant & Macro Search):** Optimized for standard and highly zoomed close-up shots (`maxRadius=180`) with strict accumulator confidence (`param2=34`).
* **Pass 2 (Standard & Tight Clusters):** Employs tight spatial allowances (`minDist=20`) to allow physically touching or packed coins to register independently.
* **Pass 3 (Deep Occlusion Sweep):** An ultra-sensitive sweep (`param2=21`) tuned to capture faint boundary fragments of coins buried behind overlapping clusters.

### 3. Global Geometric Deduplication Shield
To prevent concentric designs, strong inner coin rims, or multi-pass overlaps from generating duplicate counts, candidate circles pass through a strict spatial deduplication filter:
* Suppresses new detections sharing nearly identical centers with existing verified coins.
* Shields valid borders against stray ghost rings by enforcing a structural overlap threshold.

### 4. Currency-Independent Analysis
* Extracts precise visual features (pixel radius, HSV/BGR color profiles) directly from isolated coin masks uploaded:CVProject.ipynb.
* Dynamically classifies and groups detected objects by relative size categories and metallic hues rather than hardcoded currency templates uploaded:CVProject.ipynb.

---

## 🛠️ Requirements & Installation

Ensure you have Python 3.x installed along with the essential computer vision and data processing libraries:

```bash
pip install opencv-python numpy matplotlib
