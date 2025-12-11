# CS572 Brain Tumor Segmentation

A comprehensive implementation of unsupervised clustering methods for brain tumor segmentation on BraTS 2020 MRI data. This project compares three clustering algorithms: Fuzzy C-Means (FCM), Gaussian Mixture Model (GMM), and Spectral Clustering.

## Overview

This project implements a complete pipeline for brain tumor segmentation using multi-modal MRI data (T1, T1CE, T2, FLAIR). The segmentation identifies three tumor regions:
- **Whole Tumor (WT)**: Complete tumor region
- **Tumor Core (TC)**: Core tumor region
- **Enhancing Tumor (ET)**: Enhancing tumor region

## Project Structure

```
CS572_Final/
├── src/
│   ├── cs572_preprocessing.py    # Data preprocessing pipeline
│   ├── fcm_cs572.py              # Fuzzy C-Means clustering
│   ├── gaussian_cs572.py         # Gaussian Mixture Model clustering
│   └── spectral_cs572.py         # Spectral clustering
├── notebook_files/                # Jupyter notebooks (original implementations)
├── requirements.txt               # Python dependencies
└── README.md                      # This file
```

## Features

### Preprocessing Pipeline
- **Brain Masking**: Automatic brain extraction from multi-modal MRI
- **Intensity Clipping**: Percentile-based intensity normalization (1st-99th percentile)
- **Z-score Normalization**: Standardization of intensity values
- **Volume Cropping**: Bounding box-based cropping to focus on brain region
- **ROI Selection**: Region of interest selection based on FLAIR and T1CE intensities
- **Feature Extraction**: Multi-channel feature vectors from 4 MRI modalities
- **Dimensionality Reduction**: Optional PCA for feature reduction

### Clustering Methods

1. **Fuzzy C-Means (FCM)**: Soft clustering with fuzzy membership assignments
2. **Gaussian Mixture Model (GMM)**: Probabilistic clustering with diagonal covariance
3. **Spectral Clustering**: Graph-based clustering with subsampling and label propagation

### Evaluation
- **Hungarian Algorithm**: Cluster-to-ground-truth label alignment
- **Dice Score**: Evaluation metric for WT, TC, and ET regions
- **Visualization**: Side-by-side comparison of predictions and ground truth

## Requirements

- Python 3.7+
- See `requirements.txt` for full dependency list

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd CS572_Final
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

## Usage

### Data Preparation

Download the BraTS 2020 Training Data using one of the following methods:

#### Option 1: Using Kaggle API (Recommended)

1. Install the Kaggle API:
```bash
pip install kaggle
```

2. Set up your Kaggle credentials:
   - Go to your [Kaggle Account Settings](https://www.kaggle.com/account)
   - Scroll down to the "API" section
   - Click "Create New Token" to download `kaggle.json`
   - Place `kaggle.json` in `~/.kaggle/` (Linux/Mac) or `C:\Users\<username>\.kaggle\` (Windows)
   - Set appropriate permissions:
     ```bash
     chmod 600 ~/.kaggle/kaggle.json  # Linux/Mac
     ```

3. Download the dataset:
```bash
kaggle datasets download -d awsaf49/brats20-dataset-training-validation
```

4. Extract the dataset:
```bash
unzip brats20-dataset-training-validation.zip -d /path/to/dataset/
```

#### Option 2: Manual Download

1. Visit the [BraTS 2020 dataset page on Kaggle](https://www.kaggle.com/datasets/awsaf49/brats20-dataset-training-validation)
2. Click the "Download" button (requires Kaggle account)
3. Extract the downloaded ZIP file to your desired location

#### After Download

Update the data path in the preprocessing script:
```python
root_dir = '/path/to/dataset/'  # Update this path
```

The dataset structure should be:
```
/path/to/dataset/
└── BraTS2020_TrainingData/
    └── MICCAI_BraTS2020_TrainingData/
        ├── BraTS20_Training_001/
        │   ├── BraTS20_Training_001_t1.nii
        │   ├── BraTS20_Training_001_t1ce.nii
        │   ├── BraTS20_Training_001_t2.nii
        │   ├── BraTS20_Training_001_flair.nii
        │   └── BraTS20_Training_001_seg.nii
        └── ...
```

## Dataset

This project uses the **BraTS 2020 Training Data**, which includes:
- T1-weighted MRI
- T1-weighted contrast-enhanced MRI (T1CE)
- T2-weighted MRI
- FLAIR MRI
- Ground truth segmentation masks

Each patient directory should contain files with naming convention:
- `{patient_id}_t1.nii`
- `{patient_id}_t1ce.nii`
- `{patient_id}_t2.nii`
- `{patient_id}_flair.nii`
- `{patient_id}_seg.nii`


### Running the Pipeline

The scripts are designed to be run sequentially:

1. **Preprocessing**:
```python
python src/cs572_preprocessing.py
```

2. **FCM Clustering**:
```python
python src/fcm_cs572.py
```

3. **Gaussian Mixture Model**:
```python
python src/gaussian_cs572.py
```

4. **Spectral Clustering**:
```python
python src/spectral_cs572.py
```

**Note**: The clustering scripts expect preprocessed data from the preprocessing step. Ensure variables are properly shared between scripts or modify the scripts to load saved preprocessed data.

## Results

Each clustering method outputs:
- Cluster assignments for each voxel
- Dice scores for WT, TC, and ET regions
- Visualization comparisons with ground truth
