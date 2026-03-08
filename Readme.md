# RenAIssance Project - GSoC 2026 Evaluation Test I

This repository contains my solution for **Evaluation Test I: Optical Character Recognition of printed sources** for the RenAIssance project.

## Strategy & Architecture
To meet the project's requirements, I developed a three-stage pipeline focused on historical document restoration:

1. **Layout Analysis (Computer Vision):** Utilizing OpenCV to detect the main text ROI, effectively ignoring marginalia and decorative borders.
2. **OCR Engine (CNN+LSTM):** A Convolutional-Recurrent architecture (Tesseract) was selected over modern Transformers to ensure orthographic fidelity and prevent modern-domain hallucinations.
3. **Late-Stage LLM Refinement:** Integration of **Qwen-2.5-1.5B** to perform contextual error correction (e.g., fixing the 'Long S') while preserving 17th-century Spanish orthography.

## Evaluation Results
The pipeline was evaluated against a manual ground truth transcription of Covarrubias's *Tesoro de la Lengua* (1611).
- **Raw OCR CER:** ~18.5%
- **Refinement Strategy:** The LLM stage focuses on semantic readability. While modern spelling biases in LLMs can affect character-level metrics, the pipeline significantly improves the searchability of historical texts.

## Installation & Usage
The solution is designed to run in a GPU-accelerated environment (Google Colab T4).

1. Install system dependencies:
   ```bash
   sudo apt-get install poppler-utils tesseract-ocr tesseract-ocr-spa tesseract-ocr-lat
   ```
2. Install Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run the notebook `RenAIssance_Test_I.ipynb`.

## 📂 Repository Structure
- `RenAIssance_Test_I.ipynb`: The primary implementation.
- `requirements.txt`: Python dependency list.
- `processed_images/`: Samples of cropped ROI and segmented lines.
