## 🔗 Try It Online

You can run this notebook directly in your browser using Binder:

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/devcodes2108/SecurePDF-BatchEncryptor/HEAD?urlpath=%2Fdoc%2Ftree%2FPdf_protector.ipynb)

# SecurePDF Batch Encryptor

A Jupyter Notebook tool to encrypt single or multiple PDF files using PyPDF2 and interactive ipywidgets. The notebook started as a single-file encryptor and evolved into a feature-rich batch processor that can decrypt already-encrypted PDFs (when provided the correct password), validate strong passwords, and save outputs to a chosen folder.

This repository contains the notebook:
- `Pdf_protector.ipynb` — interactive Jupyter notebook implementing single and batch encryption workflows.

---

## Features

- Single-file encryption via an interactive widget (choose input, specify output name, enter password).
- Batch encryption:
  - Select multiple PDFs via a SelectMultiple widget.
  - Provide a decryption password (for already-encrypted inputs) and an encryption password (for the resulting files).
  - Auto-prefix outputs with `protected_`.
  - Choose an output folder (defaults to your Downloads folder).
  - Produces a summary report with success / failure counts.
- Strong password validation (minimum 8 characters, uppercase, lowercase, digit, and special character).
- Clear All button to reset the widget state.
- Graceful handling of invalid files and exceptions; reports errors in the notebook output area.

---

## Quick analysis of notebook logic

- Uses PyPDF2.PdfReader and PyPDF2.PdfWriter to read, optionally decrypt, copy pages, and encrypt outputs.
- Checks for reader.is_encrypted and attempts reader.decrypt(decrypt_password) when needed.
- Validates encryption passwords using a regular-expression-based function that enforces strength rules.
- Writes outputs to the current working directory or a user-specified output path.
- The UI is built with `ipywidgets` for an in-notebook graphical experience.

---

## Requirements

- Python 3.8+ (the notebook was developed with Python 3.10)
- Jupyter Notebook / JupyterLab
- PyPDF2
- ipywidgets

Recommended: create a separate environment to keep dependencies isolated.

Conda (recommended)
```bash
conda create -n pdfenv python=3.10
conda activate pdfenv
conda install -c conda-forge PyPDF2 ipywidgets
```

Pip
```bash
python -m venv pdfenv
source pdfenv/bin/activate   # macOS / Linux
pdfenv\Scripts\activate      # Windows

pip install PyPDF2 ipywidgets
```

If using classic Jupyter Notebook (not JupyterLab), you may need to enable widgets:
```bash
jupyter nbextension enable --py widgetsnbextension
```

For JupyterLab 3.x+, ipywidgets support is typically included; otherwise install the labextension for older versions.

---

## Usage

1. Start Jupyter Notebook / Lab:
```bash
jupyter notebook
# or
jupyter lab
```

2. Open `Pdf_protector.ipynb`.

3. Follow the on-screen widgets:
   - Single-file mode: pick an input PDF, enter output filename and password, press "Encrypt PDF".
   - Batch mode: select one or more PDFs, set Decryption Password (if inputs are already encrypted), set Encryption Password (applied to outputs), choose an output folder (defaults to Downloads), then click "Encrypt Selected".
   - Use "Clear All" to reset widget values.

4. After completion, encrypted files will be saved in the specified folder with filenames prefixed by `protected_`.

---

## Example

- Encrypt a single file:
  - Input: `Sample.pdf`
  - Output: `protected_sample.pdf`
  - Password: `Str0ng!Pass`

- Batch encrypt multiple files:
  - Select files: `doc1.pdf`, `doc2.pdf`
  - Decryption Password: `IfAnyExistingPassword` (leave blank for unencrypted inputs)
  - Encryption Password: `NewStr0ng!Pass`
  - Output folder: `./encrypted`
  - Result files: `./encrypted/protected_doc1.pdf`, `./encrypted/protected_doc2.pdf`

---

## Security & Notes

- The notebook uses password-based encryption provided by PyPDF2. This is suitable for casual protection, but for high-security contexts consider stronger encryption tools and key management.
- If a PDF is already encrypted, you must provide its correct decryption password to read and re-encrypt it.
- Passwords are entered in widget Password fields which hide the characters in the notebook UI, but the notebook environment itself should still be treated as trusted (avoid using it on untrusted machines).
- The code enforces password complexity — weak passwords will be rejected with an explanatory message.

---

## Troubleshooting

- "PdfReadError" or "not a valid PDF": the input file may be corrupt or not a PDF.
- "Permission denied" when writing files: ensure the notebook process has write permission to the chosen output folder.
- ipywidgets not showing: verify `ipywidgets` is installed and enabled; for classic notebook run `jupyter nbextension enable --py widgetsnbextension`.

---



