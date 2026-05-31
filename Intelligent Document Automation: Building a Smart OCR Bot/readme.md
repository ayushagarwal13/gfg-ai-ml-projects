# Intelligent Document Automation OCR Bot

## Overview

This project is an AI-powered Intelligent Document Processing (IDP) and OCR automation system developed using Python, OpenCV, and OCR techniques. The notebook demonstrates how to preprocess scanned receipt images, correct skew, reduce noise, binarize images, and extract textual information from documents.

The project is based on the **SROIE2019 Receipt Dataset** and focuses on improving OCR accuracy through advanced image preprocessing.

---

# Features

* Image preprocessing using OpenCV
* Grayscale conversion
* Noise reduction using Gaussian Blur
* Image binarization (thresholding)
* Image deskewing and rotation correction
* OCR-ready image enhancement
* Receipt/document analysis
* Visualization using Matplotlib
* Dataset handling using KaggleHub

---

# Tech Stack

* Python
* OpenCV
* NumPy
* Matplotlib
* KaggleHub
* OCR Techniques
* Jupyter Notebook

---

# Dataset

This project uses the **SROIE2019 Receipt Dataset**.

Dataset includes:

* Receipt images
* OCR annotations
* Entity extraction labels

---

# Project Workflow

## 1. Dataset Collection

The receipt dataset is downloaded using KaggleHub.

## 2. Image Preprocessing

The following preprocessing steps are applied:

### Grayscale Conversion

Converts colored receipt images into grayscale for easier processing.

### Noise Reduction

Applies Gaussian Blur to remove unwanted noise.

### Binarization

Converts image into black-and-white format using thresholding.

### Deskewing

Automatically detects and corrects image rotation/skew.

---

# Folder Structure

```bash
Intelligent_Document_Automation_OCR_Bot/
│
├── Intelligent_Document_Automation_OCR_Bot.ipynb
├── README.md
└── dataset/
```

---

# Installation

## Clone the Repository

```bash
git clone https://github.com/your-username/intelligent-document-automation-ocr-bot.git
cd intelligent-document-automation-ocr-bot
```

## Install Dependencies

```bash
pip install opencv-python matplotlib numpy kagglehub
```

---

# Usage

Run the Jupyter notebook:

```bash
jupyter notebook Intelligent_Document_Automation_OCR_Bot.ipynb
```

Execute the notebook cells step by step to:

* Load receipt images
* Preprocess documents
* Correct skewed images
* Prepare images for OCR
* Visualize outputs

---

# Example Preprocessing Pipeline

```python
# Convert to grayscale
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Reduce noise
blur = cv2.GaussianBlur(gray, (5,5), 0)

# Thresholding
_, thresh = cv2.threshold(blur, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)
```

---

# Future Improvements

* Integrate Tesseract OCR
* Add NLP-based entity extraction
* Deploy as a Streamlit web application
* Add document classification
* Support PDFs and scanned invoices
* Build API endpoints for automation

---

# Applications

* Invoice processing
* Receipt digitization
* Banking document automation
* Insurance claim processing
* Enterprise document workflows
* OCR-based automation systems

---

# Results

The preprocessing pipeline significantly improves OCR readability by:

* Removing image noise
* Correcting skewed text
* Enhancing document clarity
* Improving text extraction accuracy

---

# Author

Ayush Agarwal

---

# License

This project is for educational and research purposes.
