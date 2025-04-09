+++
title = "Secure OCR System"
date = 2024-08-20
summary = "An OCR pipeline secured with hash verification and multi-step validation."
tags = ["OCR", "Security", "Python"]
draft = false
link = "/projects/post2/"
+++

## 📌 Summary

This project is an ongoing effort to build a **secure and flexible OCR pipeline** capable of converting **images and PDFs into readable text**. Unlike simple one-off OCR scripts, this system focuses on:

- **Multi-format support** (image + PDF)
- **Secure text extraction** without network dependencies
- **CRNN integration** for improved accuracy on noisy samples

---

## 🧪 Objective

- Build a local OCR tool with no cloud or API dependency
- Process both images and PDF files efficiently
- Append extracted text continuously for large documents
- Support poor-quality inputs with **robust recognition**

---

## 🧰 Tools & Stack

- Python
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
- [PyTesseract](https://pypi.org/project/pytesseract/)
- OpenCV
- CRNN (Convolutional Recurrent Neural Network – In Progress)
- PDF2Image

---

## 🖼️ Image Preprocessing

To improve OCR accuracy, especially on scanned documents, we apply preprocessing techniques like grayscale conversion, thresholding, and denoising.

```python
import cv2

img = cv2.imread("input.jpg")
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
thresh = cv2.adaptiveThreshold(gray, 255, cv2.ADAPTIVE_THRESH_MEAN_C,
                               cv2.THRESH_BINARY, 11, 2)
```

---

## 🔠 Text Extraction with Tesseract

```python
import pytesseract
from PIL import Image

text = pytesseract.image_to_string(Image.open("input.jpg"))
with open("output.txt", "a") as f:
    f.write(text + "\n")
```

---

## 📄 PDF Support

Using `pdf2image` to convert PDF pages into image frames before feeding them to the OCR pipeline:

```python
from pdf2image import convert_from_path

pages = convert_from_path('document.pdf', 300)
for i, page in enumerate(pages):
    page.save(f"page_{i}.jpg", "JPEG")
```

---

## 🔐 Why Secure?

- **No external APIs** — everything runs locally
- **No data exposure** — safe for sensitive documents
- **Customizable pipeline** — can be deployed offline or embedded in private systems

---

## 🚧 CRNN Integration (In Progress)

We're currently integrating a lightweight **CRNN model** for better accuracy in cases where:

- The text is handwritten or distorted
- Tesseract’s heuristics fail
- Line-by-line sequence modeling is necessary

CRNN uses a combination of **CNN layers** for feature extraction and **RNN (LSTM/GRU)** for sequential character prediction.

---

## 📌 Features (Planned)

- [x] Image & PDF input support  
- [x] Secure local execution  
- [x] Continuous text appending  
- [ ] CRNN model integration  
- [ ] GUI or command-line interface

---

## 🔗 References & Docs

- [Tesseract OCR GitHub](https://github.com/tesseract-ocr/tesseract)
- [CRNN Paper (Arxiv)](https://arxiv.org/abs/1507.05717)
- [pdf2image PyPI](https://pypi.org/project/pdf2image/)
- [OpenCV Docs](https://docs.opencv.org/)

---

## 🧠 What I’m Learning

This project is teaching me how to:

- Design OCR systems that are **modular and secure**
- Improve OCR performance with **preprocessing pipelines**
- Explore deep learning methods like **CRNNs** for better accuracy

---

Want to contribute or test the system? Feel free to reach out or check my [other projects](/projects/).
