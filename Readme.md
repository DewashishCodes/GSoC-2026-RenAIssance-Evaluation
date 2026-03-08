# GSoC 2026 Evaluation: RenAIssance Project
## Test I: Optical Character Recognition of Printed Sources

This repository contains a production-grade OCR pipeline designed for the **RenAIssance Project**. It transforms high-noise, 17th-century printed scans into structured, human-readable text using a hybrid **Computer Vision + CNN-LSTM + LLM** architecture.

---

## 📊 Performance Metrics & Analysis

The pipeline was evaluated against a manual ground-truth transcription of **Covarrubias's *Tesoro de la Lengua* (1611)**.

### Quantitative Results
| Metric | Raw CNN+LSTM OCR | LLM Refined (Qwen 2.5) | Note |
| :--- | :--- | :--- | :--- |
| **CER (Character Error Rate)** | **18.50%** | 31.00% | *Metric penalty due to LLM modernization* |
| **WER (Word Error Rate)** | 64.62% | 79.23% | *Semantic alignment vs. Orthographic fidelity* |

### Key Qualitative "Wins"
Despite the metric penalty caused by "modernization drift," the LLM stage achieved significant qualitative successes that are vital for the RenAIssance project:
*   **Historical Proper Name Restoration:** Successfully corrected the misread `Arias? dorta110` to the correct historical figure `Benedicto Arias Montano`.
*   **Orthographic Correction:** Automatically identified and corrected the "Long S" (ſ) artifact, which is often misread as 'f' by standard OCR engines.
*   **Contextual Repair:** Reconstructed fragmented lines (e.g., correcting misread words like `penencion` to the logical Spanish `prevencion`).

---

## 🧠 Architectural Strategy & Innovation

### 1. The Pivot: CNN+LSTM vs. Transformer
During development, I initially tested **TrOCR (Transformer-based OCR)**. However, I identified a significant **Domain Mismatch**: the model hallucinated modern English "Fast Food Receipt" terminology due to its pre-training data. 
*   **The Solution:** I pivoted to a **Convolutional-Recurrent (CNN+LSTM)** architecture (Tesseract) using Spanish and Latin language packs. This provided much higher stability and lower hallucination rates for archaic typography.

### 2. Layout Analysis & Marginalia Removal
Fulfilling the core requirement of the test, I implemented an **OpenCV-based ROI Detector**. Using horizontal projection profiles and contour area filtering, the system automatically:
*   Isolates the main text block.
*   Ignores marginalia and decorative embellishments.
*   Segments text into clean lines for optimal OCR ingestion.

### 3. Late-Stage LLM Refinement
I utilized **Qwen-2.5-1.5B-Instruct** (via Hugging Face) as a semantic stabilizer. The research showed that while LLMs improve **Readability and Searchability**, they struggle with **Orthographic Fidelity** (preserving 17th-century spelling like *zelosos*). This insight is documented in the final section of the notebook as a roadmap for future fine-tuning.

---

## 📂 Repository Structure
*   `RenAIssance_Test_I.ipynb`: Full implementation with logic, visualization, and evaluation.
*   `RenAIssance_Test_I_Output.pdf`: Exported report with all results and graphs visible.
*   `requirements.txt`: Project dependencies for reproducibility.

## 🛠️ Setup
1.  **System:** `apt-get install poppler-utils tesseract-ocr-spa tesseract-ocr-lat`
2.  **Environment:** Google Colab T4 GPU recommended.
