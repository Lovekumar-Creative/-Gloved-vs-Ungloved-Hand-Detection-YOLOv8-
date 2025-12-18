# 🧤 Gloved vs Ungloved Hand Detection (YOLOv8)

## 📌 Project Overview
This project implements an end-to-end **object detection pipeline** to identify
whether a person is wearing gloves or not. The system is designed for a **safety
compliance use case** in industrial environments, where monitoring glove usage
is critical. The pipeline covers data preparation, model training, validation,
inference, and structured result logging.

---

## 📂 Dataset

**Dataset Name:** Custom Glove Detection Dataset  
**Source:**  
- Roboflow Universe (hand / PPE datasets)  
- Manual filtering and organization  

### Classes
- `gloved_hand`
- `bare_hand`

### Dataset Format
- YOLO annotation format (`.txt`)
- Dataset split into:
  - `train/`
  - `valid/`
  - `test/`

Only images with corresponding label files are used during training and
validation, following YOLO’s standard practice.

---

## 🧠 Model Used

- **Model:** YOLOv8 (Ultralytics)
- **Base Weights:** `yolov8n.pt`
- **Framework:** PyTorch (via Ultralytics)

Transfer learning was applied to adapt the pretrained YOLOv8 model (trained on
COCO) to the task of detecting **gloved vs bare hands**, which are not native
classes in the original dataset.

---

## 🔄 Preprocessing and Training

### Preprocessing
- Images resized to **640 × 640**
- Dataset validated for label consistency
- Separate training and validation splits

### Training
- Fine-tuned YOLOv8 using transfer learning
- Trained for multiple epochs
- Data augmentation applied to improve robustness and recall:
  - HSV color jitter (lighting variations)
  - Scaling and translation
  - Rotation
  - Horizontal flipping
  - Mosaic and MixUp augmentation

These augmentations help the model handle real-world challenges such as motion
blur, occlusion, and lighting changes.

---

## 📊 Evaluation

The model was evaluated on a fixed validation dataset using standard object
detection metrics:

- **Precision**
- **Recall**
- **mAP@50**
- **mAP@50–95**

Validation results showed high precision with moderate recall, indicating strong
detection accuracy with some missed edge cases such as partial occlusions.

---

## 🖼️ Inference and Output

After training, the best-performing model was used to run inference on unseen
test images.

### Outputs
- Annotated images with bounding boxes → `output/`
- Detection logs saved as JSON files → `logs/`

### JSON Log Format
```json
{
  "filename": "image1.jpg",
  "detections": [
    {
      "label": "gloved_hand",
      "confidence": 0.92,
      "bbox": [x1, y1, x2, y2]
    }
  ]
}
