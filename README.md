# Case Analyser

A small tool that takes a case-study PDF, extracts its text (with OCR fallback for scanned/image-based PDFs), and sends it to an AI model with a structured prompt to produce a judge-style case analysis. The output is saved as a text file, and can be pasted into Claude to generate a presentation-ready PPT.

## What it does

1. Extracts text from a case-study PDF using `pdfplumber`.
2. If the PDF is scanned/image-based and direct extraction fails, falls back to OCR using `pytesseract` + `pdf2image`.
3. Sends the extracted text to Gemini with a detailed prompt that:
   - Explains the case in simple terms
   - Separates facts, calculations, and recommendations
   - Identifies the strongest insight in the case
4. Saves the AI's analysis to `AI_Case_Study_Analysis.txt`.
5. That output can be copy-pasted into Claude with a prompt to turn it into a PPT (see below).

## Setup

Install dependencies (first cell of the notebook does this):

```bash
pip install pdfplumber pytesseract pdf2image
pip install -U google-genai
```

You'll also need:
- **Tesseract OCR** installed on your system (required by `pytesseract` for scanned PDFs)
- **Poppler** installed on your system (required by `pdf2image`)
- A **Gemini API key** — set as an environment variable, not hardcoded in the notebook (see Security note below)

## How to run

1. Place your case-study PDF in the **same folder** as the notebook.
2. Update the filename in the notebook:
   ```python
   pdf_path = "CASE STUDY.pdf"  # change this to your PDF's actual filename
   ```
3. Run all cells top to bottom.
4. The analysis will print in the notebook and save to `AI_Case_Study_Analysis.txt` in the same folder.

## Turning the analysis into a PPT

Once you have `AI_Case_Study_Analysis.txt`, open Claude and use a prompt like:

> "Here is my case study analysis. Please turn this into a clean, presentation-ready PPT with slides for: case overview, problem statement, key insights, analysis/calculations, and recommendations. [paste analysis text here]"

Claude can generate the deck directly from that.

## Security note

⚠️ The current notebook has the Gemini API key **hardcoded directly in the code** (cell 5). Before sharing, uploading, or committing this notebook anywhere (GitHub, cloud storage, etc.):

1. **Regenerate/revoke the current key** in your Google AI Studio account, since it's already been exposed.
2. Replace the hardcoded key with an environment variable read from a `.env` file or your system's environment, e.g.:
   ```python
   import os
   api_key = os.environ.get("GEMINI_API_KEY")
   ```
3. Never commit `.env` files or API keys to version control.

## Known limitations

- OCR accuracy depends on scan quality; heavily formatted or table-heavy PDFs may extract imperfectly.
- Output formatting/coloring in some renders needs further polishing.
- Currently tested on 2 case studies — expect to refine prompt/output structure with more use.
