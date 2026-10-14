# Assessment 1: Image Processing and Object Manipulation

## Project Overview
This repository contains the ComfyUI workflows (JSON) and final high-resolution image outputs for Assessment 1. The objective was to execute a four-stage pipeline: upscaling, seamless 16:9 outpainting, object removal (clean plate), and photorealistic object insertion.

## Dataset Structure
* **Input:** `zebra.jpg` (Original base image)
* **Outputs:** Located in the `/outputs` directory (Upscaled/Extended landscape, Clean Plate, Object Insertion).

## Installation Steps
1. Install [ComfyUI](https://github.com/comfyanonymous/ComfyUI).
2. Download `FluentlyXL-v3-inpainting.safetensors` and place it in `/models/checkpoints/`.
3. Download `4x-UltraSharp.pth` and place it in `/models/upscale_models/`.

## Training Instructions
*N/A - This task utilizes pre-trained diffusion models for zero-shot inference and generation.*

## Prediction / Execution Instructions
1. Drag and drop the provided `Redchilles Assesment 1 - (Amrut).json` into the ComfyUI interface.
2. Upload the input image into the initial `Load Image` node.
3. For inpainting/removal tasks, right-click the `Load Image` node, select "Open in Mask Editor", and paint the target region.
4. Click **Queue Prompt** to execute the pipeline.

## Model Used
* **Primary Checkpoint:** `FluentlyXL-v3-inpainting.safetensors`
* **Upscaler:** `4x-UltraSharp.pth`

## Workflow Breakdown & Image Preprocessing
* **1. Upscaling:** The base image was upscaled 4x using the `4x-UltraSharp` model prior to outpainting to prevent low-fidelity textures. It was then refined through a standard `VAE Encode` and `KSampler` at 0.25 denoise.
* **2. Outpainting:** Padded to 16:9 using `Pad Image for Outpainting` (112px padding, feathering 10) and processed through `VAE Encode (for Inpainting)` (grow_mask_by: 16) into a KSampler at 1.0 denoise.
* **3. Object Removal:** The mask was softened with a `Mask Blur` (amount: 15) and piped into `VAE Encode (for Inpainting)` (grow_mask_by: 15), then processed at 1.0 denoise to create the clean plate.
* **4. Object Insertion:** A tighter mask was drawn and softened with `Mask Blur` (amount: 4), piped into `VAE Encode (for Inpainting)` (grow_mask_by: 15), and processed at 0.85 denoise to anchor the new object.

## Evaluation Results
* **Output 1:** Seamless 16:9 outpainted savannah extension without visible horizon breaks.
* **Output 2:** Artifact-free removal of the primary object (zebra) resulting in a pristine clean background plate.
* **Output 3:** Photorealistic integration of a vintage leather trunk, anchored with accurate localized ground shadows.

## Known Limitations
* Drawing excessively large masks during the object insertion phase (at high denoise levels) can cause the inpainting model to struggle with edge blending, resulting in slight textural halos. Masks must be kept tight to the target object dimensions.
