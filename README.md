# DL-vitrual-try-on

This repository contains the implementation files for our **Flux.2-based Virtual Try-On** project.
The goal of this project is to generate realistic try-on results from a **person image** and a **target garment image**, while preserving the person identity, background, pose, and garment details as much as possible.

## Project Overview

This project builds a virtual try-on workflow based on **Flux.2 / Klein**.
Given a model image and a garment image, the pipeline edits the person image so that the target garment is naturally transferred onto the person.

The main idea is a **two-input, two-stage editing pipeline**:

1. **Garment Removal**
   The original clothing in the person image is first removed to reduce interference from the original garment shape.

2. **Garment Redrawing**
   The target garment is then generated on the cleaned person image using the garment image and semantic prompts.

To improve the final result, the project also includes semantic description, garment preprocessing, mask control, LoRA-based consistency, multi-view generation, and quantitative evaluation.

## Main Methods

* **Flux.2 / Klein Virtual Try-On Pipeline**
  Used as the main image editing and generation backbone.

* **Two-Input Two-Stage Editing**
  Uses both the person image and the garment image. The original garment is removed first, then the new garment is generated.

* **Florence2 and QwenVL Prompt Enhancement**
  Extracts garment descriptions such as color, texture, sleeve type, length, and pattern to improve garment fidelity.

* **RMBG Garment Preprocessing**
  Removes the background of product garment images and crops the garment region to reduce irrelevant margins.

* **Adaptive Mask Control**
  Uses masks to restrict the editable region, reduce background/person drift, and improve boundary consistency.

* **LoRA for Style Consistency**
  Applies `f2k_consis.safetensors` to improve visual consistency and reduce style drift.

* **Multi-Perspective Generation**
  Includes workflows for generating different camera views and pose-related outputs.

* **No-Ground-Truth Evaluation**
  Since real try-on ground truth is unavailable, the evaluation uses proxy metrics for identity preservation and garment fidelity.

## Repository Structure

```text
DL-vitrual-try-on/
│
├── README.md
│
├── Final_colab.ipynb
│   └── Main Colab pipeline for the virtual try-on workflow.
│
├── evalution.ipynb
│   └── Evaluation notebook for no-ground-truth virtual try-on metrics.
│
└── code/
    ├── Baseline.json
    │   └── Basic Flux.2 image editing workflow.
    │
    ├── Baseline_Florence.json
    │   └── Baseline workflow enhanced with Florence2 garment description.
    │
    ├── flux_twoinputs.json
    │   └── Two-input workflow using both model image and garment image.
    │
    ├── multipleperspectivescamera.json
    │   └── Multi-camera / multi-perspective generation workflow.
    │
    ├── openposetoperson.json
    │   └── OpenPose-based person pose transfer workflow.
    │
    └── pose_extraction.json
        └── Pose extraction workflow.
```

## How to Use

### Colab Pipeline

Open and run:

```text
Final_colab.ipynb
```

This notebook contains the main virtual try-on pipeline.
It supports person image input, garment image input, garment preprocessing, mask control, and final try-on generation.

### RunningHub / ComfyUI Workflows

The JSON files in the `code/` folder are workflows used on RunningHub / ComfyUI.
They correspond to different experiments in the project, including baseline generation, Florence2-enhanced generation, two-input try-on, multi-perspective generation, and pose-based generation.

### Evaluation

Open and run:

```text
evalution.ipynb
```

The evaluation notebook computes several proxy metrics, including:

* Face embedding similarity
* CLIP style consistency
* VGG Gram texture distance
* HSV color histogram distance
* DINOv2 garment similarity
* Patch-level coverage
* Local feature matching for logos, text, and patterns

These metrics are used because the project does not have real ground-truth try-on images.

## Results
<img src="assets\results\1\11.jpg" width="30%">
<img src="assets\results\1\04.jpg" width="30%">
<img src="assets\results\1\11result.png" width="30%">

<img src="assets\results\2\32.jpg" width="30%">
<img src="assets\results\2\1.jpg" width="30%">
<img src="assets\results\2\32result.png" width="30%">

<img src="assets\results\3\model_1.png" width="30%">
<img src="assets\results\3\13.jpg" width="30%">
<img src="assets\results\3\13result.png" width="30%">
<img src="assets\results\3\05.jpg" width="30%">
<img src="assets\results\3\05result.png" width="30%">

<img src="assets\results\1.png" width="100%">
<img src="assets\results\2.png" width="100%">
<img src="assets\results\3.png" width="100%">
<p align="center">
  <b>Example virtual try-on results</b>
</p>


## Conclusion

This project implements a Flux.2-based virtual try-on pipeline that combines two-stage editing, semantic garment description, adaptive masking, LoRA consistency, and multi-perspective generation. The pipeline improves garment transfer quality while preserving the identity, background, and overall visual style of the original person image.
