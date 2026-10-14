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
1. Drag and drop the provided `Assessment1_Workflow.json` into the ComfyUI interface.
2. Upload the input image into the initial `Load Image` node.
3. For inpainting/removal tasks, right-click the `Load Image` node, select "Open in Mask Editor", and paint the target region.
4. Click **Queue Prompt** to execute the pipeline.

## Model Used
* **Primary Checkpoint:** `FluentlyXL-v3-inpainting.safetensors`
* **Upscaler:** `4x-UltraSharp.pth`

## Image Preprocessing
* **Resolution Scaling:** The base image was upscaled 4x using UltraSharp *prior* to outpainting. This provided a native high-resolution canvas, preventing the SDXL architecture from hallucinating or producing low-fidelity textures.
* **Latent Masking:** Mask edges were softened using a `Mask Blur` node and applied directly in the latent space (`Set Latent Noise Mask`) rather than pixel space to eliminate hard rectangular seams.

## Evaluation Results
* **Output 1:** Seamless 16:9 outpainted savannah extension without visible horizon breaks.
* **Output 2:** Artifact-free removal of the primary object (zebra) resulting in a pristine clean background plate.
* **Output 3:** Photorealistic integration of a vintage leather trunk, anchored with accurate localized ground shadows (executed at 0.85 denoise).

## Known Limitations
* Drawing excessively large masks during the object insertion phase (at high denoise levels) can cause the inpainting model to struggle with edge blending, resulting in slight textural halos. Masks must be kept tight to the target object dimensions.
