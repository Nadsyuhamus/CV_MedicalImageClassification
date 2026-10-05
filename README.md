# Medical Image Classification

A computer vision project that compares a custom convolutional neural network (CNN) with MobileNetV2 transfer learning for classifying gastrointestinal endoscopy images from the Kvasir dataset.

The project is implemented in a Jupyter notebook and covers data exploration, image preprocessing, model training, performance evaluation, and an interactive image prediction demo.

## Features

- Visualize class distribution and sample images.
- Train a custom CNN with L2 regularization and dropout.
- Fine-tune MobileNetV2 using pretrained ImageNet weights.
- Compare training time and validation accuracy.
- Generate accuracy and loss curves, confusion matrices, and classification reports.
- Save trained models and select an image through a local file picker to compare predictions.

## Technology Stack

Python · TensorFlow/Keras · NumPy · OpenCV · scikit-learn · Matplotlib · Seaborn · Tkinter · Jupyter

## Dataset

The notebook uses the Kvasir gastrointestinal endoscopy dataset. Class names are detected automatically from the dataset's subfolders.

The dataset is not included in this repository. Obtain the dataset separately and place an archive named `med_cv.zip` in the project root. The archive must extract to:

```text
dataset_extracted/kvasir-dataset/<class_name>/<image_file>
```

Alternatively, extract the dataset yourself into that directory. Update `data_path` in the first code cell if your folder structure differs.

## Models and Training

| Setting | Custom CNN | MobileNetV2 |
| --- | --- | --- |
| Input size | 224 × 224 RGB | 224 × 224 RGB |
| Architecture | Three convolution and pooling blocks, followed by dense layers | ImageNet backbone, global average pooling, and dense classifier |
| Regularization | L2 and dropout | Dropout |
| Fine-tuning | Trained from scratch | Only the final 30 backbone layers are left trainable |
| Optimizer | Adam, learning rate 0.00001 | Adam, learning rate 0.00001 |
| Maximum epochs | 20 | 20 |
| Callback | Early stopping | Learning rate reduction on plateau |

Both models use categorical cross-entropy and a batch size of 32. The notebook allocates 70% of images to training and 30% to validation. Preprocessing includes scaling pixel values by 1/255, with rotation, horizontal flipping, and brightness augmentation.

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Nadsyuhamus/CV_MedicalImageClassification.git
cd CV_MedicalImageClassification
```

### 2. Create a Python environment

On macOS or Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

On Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
python -m pip install numpy matplotlib seaborn opencv-python tensorflow scikit-learn ipykernel
```

Use a Python version supported by your TensorFlow release. Tkinter must also be available in your Python installation for the image picker; it is not installed through the command above.

### 4. Run in VS Code

1. Open the cloned project folder in VS Code.
2. Install Microsoft's **Python** and **Jupyter** extensions.
3. Add `med_cv.zip` or the extracted dataset to the project folder.
4. Open `Pro_Med_Code.ipynb`.
5. Select the `.venv` environment using **Select Kernel**.
6. Run the notebook cells from top to bottom.

Internet access is required for the initial download of MobileNetV2's ImageNet weights. Training time depends on your hardware.

## Outputs

The notebook displays training curves, confusion matrices, classification reports, and a model comparison summary. It saves:

```text
custom_medical_model.h5
mobile_medical_model.h5
```

The final cell opens a local image picker and displays each model's predicted class and confidence score. It loads the saved models if available, otherwise it uses models from the current session. Imports and `class_names` must be defined before running the demo.

Datasets, Python environments, and saved model files are excluded from Git through `.gitignore`.

## Evaluation Notes

The current notebook uses the same image generator for training and validation, so random augmentation also affects validation images. It does not use a separate held-out test set. Consequently, the reported scores should be treated as validation results, and repeated evaluation may vary.

MobileNetV2 currently receives images scaled to [0, 1], rather than its architecture-specific preprocessing. A future improvement is to use a separate preprocessing pipeline for each model and apply MobileNetV2 preprocessing consistently during both training and prediction.

## Future Improvements

- Separate training augmentation from deterministic validation preprocessing.
- Add an independent test set and reproducible random seeds.
- Save the class mapping alongside each trained model.
- Pin package versions for reproducible installation.
- Build a web interface for the prediction demo.

## Author

**Nur Nadsyuha Binti Mustafa**  
Artificial Intelligence student, Universiti Teknologi Malaysia

This is an academic image classification project intended for learning and model comparison.
