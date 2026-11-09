# Prior-Corrected Scribble-Conditioned Graph Convolutional Segmentation of Radicular Cysts in CBCT: An Information-Matched Benchmark on a Public Dataset

This repository contains the official implementation, experimental folds, and trained weights for the scribble-conditioned graph convolutional network (GCN) pipeline designed to segment radicular cysts in Cone-Beam Computed Tomography (CBCT) sections, as presented in the accompanying paper.

## Overview

Delineating periapical lesions on CBCT scans is historically slow and observer-dependent. This framework provides an interactive, scribble-conditioned segmentation solution optimized to run efficiently on standard CPU setups without requiring a GPU. 

### Core Pipeline Stages
1. **Stage 1 (Image Pre-processing):** Point-spread-function (PSF)-based Wiener deconvolution followed by a 6-layer Sub-Pixel Convolutional Neural Network (SPCNN) acting as a learned smoothing filter.
2. **Stage 2 (Graph Segmentation):** Conversion of the filtered image into a Simple Linear Iterative Clustering (SLIC) Region-Adjacency Graph (RAG) with N=400 nodes. An inductive, dual-branch Graph Convolutional Network (combining ChebNet and GraphSAGE layers) processes appearance, geometry, and scribble-derived distance descriptors as input node features.
3. **Stage 3 (Pixel-Level Refinement):** An analytical, parameter-free class-prior correction step that neutralizes the over-segmentation bias introduced during weighted training, followed by a Convolutional Conditional Random Field (ConvCRF) for precise pixel-level boundary matching.
4. **Optional Branch:** A Faster R-CNN object detector (ResNet-50-FPN) that acts on the enhanced image alone to flag unmarked lesions potentially missed by the clinician.

---

## Dataset & Benchmarking

Experiments were conducted using the publicly available **DOLCHID** dataset (Dental Odontogenic Lesion CBCT and Histopathology Integrated Dataset), utilizing all 54 histology-confirmed radicular cyst cases. 

* **Validation Scheme:** Patient-level, size-stratified 4-fold cross-validation.
* **Performance Summary:** 
  * **Dice Similarity Coefficient:** 0.805 (95% CI 0.783–0.830)
  * **95th-Percentile Hausdorff Distance (HD95):** 3.68 mm
  * **Lesion-Area Bias:** −23.2 mm² (with high clinical area agreement: ICC = 0.763)
  * **Inference Speed:** ~513.8 ms per 256 × 256 section on a standard 2-core CPU configuration.

---
