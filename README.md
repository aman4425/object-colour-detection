# 🎨 Object Colour Detection

A Computer Vision project that detects and annotates multiple colour regions in an image using HSV colour space segmentation, morphological filtering, and contour analysis — built with OpenCV and Python.

---

## 📌 Problem Statement

Automated colour recognition is a foundational task in visual perception systems — from robotic sorting to assistive tools for the visually impaired. This project addresses the challenge of **robustly detecting multiple colours simultaneously** in a real-world image, handling noise, lighting variation, and the quirks of how colour spaces work (like red wrapping around the HSV hue wheel).

---

## 🧠 How It Works

The system follows a four-stage pipeline:

```
Input Image → HSV Conversion → Colour Masking → Morphological Cleanup → Contour Detection → Annotated Output
```

1. **Convert to HSV** — decouples colour (Hue) from brightness (Value), making detection lighting-robust
2. **Apply per-colour HSV masks** — each colour is defined by calibrated lower/upper HSV bounds
3. **Morphological filtering** — MORPH_OPEN removes noise, MORPH_CLOSE fills holes (7×7 elliptical kernel, 2 iterations)
4. **Contour extraction** — filters contours by minimum area (2000px), draws bounding boxes with colour labels and area percentages

### Colours Detected

| Colour | Notes |
|--------|-------|
| 🔴 Red | Dual HSV range (wraps at 0° and 180°) |
| 🟢 Green | Single HSV range |
| 🔵 Blue | Single HSV range |
| 🟡 Yellow | Single HSV range |
| 🟠 Orange | Single HSV range |
| 🟣 Purple | Single HSV range |
| ⚪ White | Detected via low saturation + high value |

---

## 📁 Repository Structure

```
object-colour-detection/
├── Object_colour_detection.ipynb   # Main notebook
├── Sample_images/                  # Sample images to test the project
└── README.md                       # This file
```

---

## 🚀 Getting Started

### Option 1 — Google Colab (Recommended)

1. Open the notebook in [Google Colab](https://colab.research.google.com/)
2. Upload `Object_colour_detection.ipynb`
3. Run all cells
4. When prompted, upload any image from your machine (you can use images from the `Sample_images` folder)
5. View the annotated output and colour coverage stats

### Option 2 — Local Setup

**Prerequisites**
```bash
pip install opencv-python numpy
```

**Modifications needed for local use** (replace the Colab upload section):
```python
# Replace this:
from google.colab.patches import cv2_imshow
from google.colab import files
uploaded = files.upload()
image_path = list(uploaded.keys())[0]

# With this:
import cv2
image_path = "your_image.jpg"   # path to your local image

frame = cv2.imread(image_path)
annotated, stats = detect_colors(frame)

cv2.imshow("Colour Detection", annotated)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

## 📊 Sample Output

For an image containing coloured objects, the system outputs:

**Console:**
```
Detected Colors:
Blue: 34.2%
Red: 18.7%
Yellow: 11.3%
Green: 6.8%
```

**Annotated Image:**  
Each detected region is boxed with a coloured rectangle and a label showing the colour name and its percentage coverage of the total image area.

> 💡 Sample images to test the project are available in the `Sample_images/` folder.

---

## ⚙️ Configuration

You can tune the system by modifying constants at the top of the notebook:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `MIN_AREA` | `2000` | Minimum contour area in pixels — increase to filter more noise |
| HSV ranges in `COLORS` dict | Calibrated values | Adjust if detection is missing colours or generating false positives |
| Kernel size in `cv2.getStructuringElement` | `(7, 7)` | Larger = more aggressive noise removal |

---

## 🔧 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `opencv-python` | ≥ 4.5 | Image processing, HSV conversion, morphology, contour analysis |
| `numpy` | ≥ 1.21 | Array operations for mask creation |
| `google-colab` | Colab built-in | File upload & image display (Colab only) |

---

## 🧩 Key Technical Decisions

**Why HSV instead of RGB?**  
RGB values change significantly with lighting intensity. HSV separates Hue (the "colour") from Value (brightness), making colour detection far more consistent across different lighting conditions.

**Why two ranges for Red?**  
OpenCV's HSV hue wheel runs from 0 to 180. Red appears at both ends (near 0° and near 180°), so a single range would miss half of it. The system uses `cv2.bitwise_or` to combine two masks.

**Why morphological filtering?**  
Raw HSV masking on real images produces hundreds of tiny noise blobs (from JPEG compression, shadows, surface textures). MORPH_OPEN removes small noise; MORPH_CLOSE fills holes in detected regions.

---

## 📚 Course

**Computer Vision (CSE3010)** — Bring Your Own Project (BYOP) Capstone  
Faculty: Rajneesh Kumar Patel | Slot: D11+D12  
Deadline: March 31, 2026

---

## 👤 Author

**Aman Prakash**  
Registration No: 23BAI10378  
B.Tech Computer Science (AI/ML Specialisation), VIT Bhopal
