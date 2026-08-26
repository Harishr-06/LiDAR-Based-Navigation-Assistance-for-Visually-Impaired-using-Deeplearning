# LiDAR-Based Navigation Assistance for Visually Impaired

A deep learning-based point-cloud processing project for obstacle detection and semantic segmentation using **LiDAR data and Dynamic Graph Convolutional Neural Network (DGCNN)**.

## 📌 Project Overview

This project focuses on using 3D LiDAR point-cloud data to identify and classify objects in the surrounding environment. A DGCNN model is trained to learn geometric relationships between neighboring points and perform point-wise semantic segmentation.

he trained model is evaluated on LiDAR point-cloud data to identify navigation-related objects and simulate obstacle warnings.

## 🎯 Objectives

* Process 3D LiDAR point-cloud data for environment understanding.
* Train a **DGCNN model** for point-cloud semantic segmentation.
* Classify navigation-related objects from LiDAR data.
* Evaluate the trained model using training and validation performance.
* Simulate obstacle detection using the trained model.

## 🧠 Model

The project uses **Dynamic Graph Convolutional Neural Network (DGCNN)**.

DGCNN constructs a dynamic graph from the input point cloud using the **k-Nearest Neighbour (k-NN)** algorithm. In this implementation:

* **k = 20 neighbours**
* Edge features are generated using neighbouring point relationships.
* EdgeConv layers extract local geometric features.
* Features from multiple layers are combined.
* Global features are aggregated and used for point-wise segmentation.

This allows the model to learn spatial and geometric relationships within LiDAR point clouds.

## 📊 Dataset

The project uses processed point-cloud data derived from:

* **ScanNet**
* **PandaSet**

The input data is stored in `.npz` format containing point-cloud coordinates and semantic labels.

### Selected Classes

The model is trained on 12 navigation-related classes:

* Wall
* Floor
* Cabinet
* Chair
* Table
* Door
* Window
* Road
* Sidewalk
* Building
* Vegetation
* Car

Other labels are mapped to an **unknown** class.

## 🔄 Data Preprocessing

The preprocessing pipeline includes:

1. Loading processed `.npz` point-cloud files.
2. Aligning point coordinates with semantic labels.
3. Randomly sampling **1,024 points** from each point cloud.
4. Centering the XYZ coordinates.
5. Normalizing the point cloud using the maximum distance from the center.
6. Applying data augmentation during training.
7. Mapping the original semantic labels to the selected classes.

### Data Augmentation

The training pipeline applies:

* Random rotation
* Random reflection
* Random scaling
* Gaussian noise

These techniques improve the model's ability to generalize to different point-cloud configurations.

## ⚙️ Training Configuration

| Parameter               | Value                  |
| ----------------------- | ---------------------- |
| Model                   | DGCNN                  |
| Input Features          | XYZ (3)                |
| Points per Sample       | 1,024                  |
| Batch Size              | 16                     |
| Epochs                  | 100                    |
| Optimizer               | AdamW                  |
| Initial Learning Rate   | 1 × 10⁻⁴               |
| Learning Rate Scheduler | Cosine Annealing       |
| Minimum Learning Rate   | 1 × 10⁻⁶               |
| k-NN Neighbours         | 20                     |
| Loss Function           | Weighted Cross Entropy |
| Framework               | PyTorch                |
| Hardware                | CUDA GPU if available  |

## 🏗️ DGCNN Architecture

The model consists of multiple EdgeConv-based feature extraction layers:

```text
Input Point Cloud
       │
       ▼
k-NN Graph Construction (k = 20)
       │
       ▼
EdgeConv Layer 1 → 64 Features
       │
       ▼
EdgeConv Layer 2 → 64 Features
       │
       ▼
EdgeConv Layer 3 → 128 Features
       │
       ▼
EdgeConv Layer 4 → 256 Features
       │
       ▼
Feature Aggregation
       │
       ▼
Global Feature Extraction
       │
       ▼
Segmentation Head
       │
       ▼
Point-wise Class Prediction
```

## 🧪 Training and Validation

The dataset is divided into training and validation/testing subsets. The model is trained for 100 epochs and evaluated using:

* Training loss
* Validation loss
* Training accuracy
* Validation accuracy

The best model is saved based on the lowest validation loss.

## 📈 Output

The training program generates:

* Training loss curve
* Validation loss curve
* Training accuracy curve
* Validation accuracy curve
* Best trained DGCNN model (`.pth`)
* Point-cloud ground-truth visualizations
* Point-cloud prediction visualizations

Example output structure:

```text
dgcnn_simplified_results/
│
├── dgcnn_simplified_best.pth
├── loss_curve.png
├── acc_curve.png
├── simulation_1.png
├── simulation_2.png
├── simulation_3.png
├── simulation_4.png
└── simulation_5.png
```

## 🚨 Obstacle Detection Simulation

After training, the best-performing model is loaded and evaluated on validation samples.

The system identifies objects from the predicted point-cloud classes. Objects such as:

* Person
* Car
* Truck
* Bus
* Motorcycle
* Bicycle

are treated as potentially dangerous objects.

When a dangerous object is detected, the program generates a warning message and can provide voice feedback using `pyttsx3`.

## 🛠️ Technologies Used

* **Python**
* **PyTorch**
* **DGCNN**
* **LiDAR Point Clouds**
* **NumPy**
* **scikit-learn**
* **Matplotlib**
* **PyTorch DataLoader**
* **pyttsx3**
* **CUDA**

## 📁 Project Structure

```text
LiDAR-DGCNN-Navigation/
│
├── dgcnn.ipynb
├── README.md
│
├── dataset/
│   └── processed_labeled_v3/
│
└── results/
    ├── dgcnn_simplified_best.pth
    ├── loss_curve.png
    ├── acc_curve.png
    └── simulation_*.png
```

> Dataset files are not included in this repository because of their size and licensing restrictions.

## 🚀 How to Run

### 1. Clone the repository

```bash
git clone https://github.com/Harishr-06/LiDAR-Based-Navigation-Assistance-for-Visually-Impaired-using-Deeplearning.git
```

### 2. Install dependencies

```bash
pip install numpy torch scikit-learn tqdm matplotlib pyttsx3
```

For GPU training, install the appropriate CUDA-enabled version of PyTorch for your system.

### 3. Prepare the Dataset

Place the processed `.npz` files in the dataset directory and update the `DATA_ROOT` path in the notebook:

```python
DATA_ROOT = r"PATH_TO_YOUR_DATASET"
```

### 4. Run the Notebook

Open:

```text
dgcnn.ipynb
```

and execute the training cell.

The program will:

1. Load the point-cloud files.
2. Split the dataset.
3. Preprocess and augment the data.
4. Train the DGCNN model.
5. Validate the model.
6. Save the best model.
7. Generate training curves.
8. Run obstacle-detection simulations.



## 👨‍💻 Author

**Harish R**

Project: **LiDAR-Based Navigation Assistance for Visually Impaired using DGCNN**
