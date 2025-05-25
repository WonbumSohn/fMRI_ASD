# 🧠 Autism Classification from fMRI using Graph Attention Networks

This project proposes a novel multi-modal deep learning framework for classifying autism spectrum disorder (ASD) using resting-state fMRI data. Unlike prior studies that focused solely on gray matter (GM), our model integrates both **gray and white matter (WM)** signals to leverage the full scope of brain activity. We developed a **Graph Attention Network (GAT)** architecture incorporating image-based functional connectivity and demographic metadata, trained and evaluated on the **ABIDE I dataset**.

---

## 🔍 Project Summary

- **Problem**: Traditional ASD classification studies exclude white matter, despite it making up ~50% of the brain and being essential to inter-regional communication.
- **Innovation**: This project integrates both GM and WM functional connectivity using a multi-modal GAT framework.
- **Approach**:
  - Constructed brain graphs with Schaefer 200 (GM) and wmsegment 128 (WM) atlases.
  - Combined ALFF, fALFF, ReHo signals as node features.
  - Applied GNNExplainer and Grad-CAM for interpretability.
  - Trained with both imaging and non-imaging (age, sex, site) data.
- **Impact**: Demonstrated improved diagnostic accuracy and interpretability, advancing clinical translation potential.

---

## 🧠 Dataset & Preprocessing

- **Source**: [ABIDE I](http://fcon_1000.projects.nitrc.org/indi/abide/) – multi-site, rs-fMRI and sMRI from 1,112 subjects (539 ASD / 573 TC).
- **Sites**: 17 (e.g., NYU, UCLA, UM, Pitt, Yale...)
- **Preprocessing**:
  - Used SPM12 and AFNI pipelines.
  - Extracted GM, WM, and GMWM BOLD signal types.
  - Performed data harmonization via **ComBat**.
  - Generated graphs with ROI-wise Pearson correlation.

---

## 🔧 Model Architecture

- **Graph Type**: Multi-modal GNN combining ROI-based and Subject-based GNNs
- **Node Features**: ALFF, fALFF, ReHo mean/std + PCC
- **Edge Definition**:
  - Brain-level: ROI-to-ROI correlations
  - Subject-level: Phenotypic similarity × embedding distance
- **GNN Blocks**:
  - ROI-based: 2×GAT + TopKPooling (128→32)
  - Subject-based: 1×GAT (embedding size = 32)
  - FC layer with Softmax for ASD/TC classification
- **Training**:
  - Leave-one-site-out CV (LOSOCV) and NYU 10-fold CV
  - Optimizer: Adam, LR=0.001, Cosine Annealing, Dropout=0.5

---

## 📈 Performance

**NYU Site Only** (10-fold CV):

- GM: 90.00% ± 3.23
- WM: 88.13% ± 3.55
- GM+WM: **92.50%** ± 2.64

---

## 🔁 Interpretability

Used **GNNExplainer** and **Grad-CAM** to identify most influential ROIs. Frequently selected nodes include:

- GM: Regions within DMN (posterior cingulate, mPFC)
- WM: Deep and temporo-frontal tracts

> Results suggest meaningful contribution of WM-based connectivity to ASD diagnosis, especially when paired with GM.

![Result](./Figures/Figure_Result_1.png)

---

## ⚖️ Comparative Models

| Model    | GM-only    | WM-only    | GM+WM      |
| -------- | ---------- | ---------- | ---------- |
| BrainGNN | 63.75%     | 70.63%     | 71.25%     |
| LG-GNN   | 80.00%     | 79.36%     | 82.50%     |
| **Ours** | **90.00%** | **88.13%** | **92.50%** |

(Results based on NYU site)

---

## 🚧 Code

> 🚧 *The full training pipeline, model weights, and preprocessing scripts will be released soon.*

---

## 📬 Contact

**Wonbum Sohn**📧 [sohnwb@gmail.com](mailto:sohnwb@gmail.com)🔗 [LinkedIn](https://www.linkedin.com/in/wonbumsohn)🌐 [www.sohnwb.com](https://www.sohnwb.com)
