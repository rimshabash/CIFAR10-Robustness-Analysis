# CIFAR-10 Robustness Analysis

A from-scratch CNN-based image classification project that investigates **model robustness under distribution shifts** using the CIFAR-10 dataset.

The project focuses on limited-data training, Inception-style architecture, data augmentation, regularization, ablation studies, class-wise analysis, hidden corruption evaluation, and test-time augmentation.

---

## 📌 Project Overview

This project evaluates how a lightweight CNN performs on:

- **Clean CIFAR-10 test images**
- **Shifted test images** with rotation, brightness/contrast changes, and occlusion
- Different **data augmentation strategies**
- Different **regularization techniques**
- **Distribution shifts and robustness**
- **Test-Time Augmentation (TTA)**

The model is trained completely **from scratch without pretrained weights**.

---

## 🎯 Objectives

The main objectives of this project are to:

- Build a lightweight CNN under **1 million trainable parameters**
- Use only **500 CIFAR-10 training images**
- Analyze overfitting with limited training data
- Improve robustness using data augmentation and regularization
- Perform a systematic **ablation study**
- Analyze class-wise performance and confusion patterns
- Evaluate performance under distribution shifts
- Investigate whether Test-Time Augmentation improves robustness

---

## 📊 Dataset Setup

The CIFAR-10 dataset contains:

- **10 classes**
- **32 × 32 RGB images**
- **50,000 training images**
- **10,000 test images**

For this project, only **500 training images** were used:

> **50 images per class**

The 500 images were divided into:

| Split | Images |
|---|---:|
| Training | 400 |
| Validation | 100 |
| Clean Test | 10,000 |
| Shifted Test | 10,000 |

The training subset was selected using a fixed random seed to maintain reproducibility.

---

## 🧠 Model Architecture

A lightweight CNN was designed specifically for this project.

The architecture contains:

- Convolutional layers
- Batch Normalization
- ReLU activation
- Max Pooling
- **Inception-style module**
- Global Average Pooling
- Fully connected classifier
- Dropout

### Inception-Style Module

The model uses parallel branches:

- `1 × 1` convolution
- `1 × 1 → 3 × 3`
- `1 × 1 → 5 × 5`
- Pooling + projection

This allows the network to learn features at different spatial scales.

### Model Constraints

- Trained from scratch
- No pretrained weights
- Less than **1 million trainable parameters**
- Maximum **60 epochs**

---

## 🔄 Data Augmentation

Several augmentation strategies were investigated.

### Geometric Augmentation

- Random Crop
- Horizontal Flip
- Random Rotation up to ±25°

### Photometric Augmentation

- Brightness adjustment
- Contrast adjustment
- Saturation adjustment
- Gaussian Blur
- Gaussian Noise

### Additional Augmentation

- Random Erasing

All augmentations were applied **on-the-fly during training**.

No pre-generated augmented copies of the training data were used.

---

## 🛡️ Regularization and Training Techniques

The project investigated several techniques to reduce overfitting:

- Batch Normalization
- Dropout
- Weight Decay
- Label Smoothing
- Learning Rate Scheduling
- Early Stopping
- Mixup

The model was selected using **validation performance only**.

The clean and shifted test sets were not used for hyperparameter tuning.

---

## 🧪 Ablation Study

Seven configurations were evaluated using three different random seeds.

| Configuration | Validation Accuracy | Clean Test | Shifted Test |
|---|---:|---:|---:|
| No Augmentation | **52.33 ± 0.58%** | **42.73 ± 0.72%** | 21.94 ± 4.84% |
| Geometric Only | 41.33 ± 5.13% | 33.98 ± 2.21% | **27.91 ± 2.44%** |
| Photometric Only | 47.00 ± 3.46% | 38.21 ± 1.87% | 25.98 ± 2.33% |
| Geometric + Photometric | 42.33 ± 1.15% | 34.42 ± 1.21% | 26.78 ± 2.04% |
| + Random Erasing | 43.67 ± 5.77% | 34.31 ± 1.79% | 27.85 ± 1.25% |
| + Mixup + Label Smoothing | 39.00 ± 1.00% | 31.66 ± 1.84% | 21.91 ± 3.94% |
| **Final Model** | **52.33 ± 0.58%** | **42.73 ± 0.72%** | **21.94 ± 4.84%** |

### Final Model Selection

The final model was selected according to the **highest mean validation accuracy**, as required by the experimental protocol.

Therefore:

> **Final Configuration: No Augmentation**

The final model achieved:

- **Validation Accuracy:** 52.33 ± 0.58%
- **Clean Test Accuracy:** 42.73 ± 0.72%
- **Shifted Test Accuracy:** 21.94 ± 4.84%

---

## 📈 Robustness Analysis

The results show an important trade-off between clean accuracy and robustness.

The **No Augmentation** configuration achieved the highest validation and clean-test accuracy.

However, **Geometric Augmentation** achieved the highest shifted-test accuracy:

> **27.91 ± 2.44%**

This represents an improvement of approximately **5.97 percentage points** over the final no-augmentation model on the shifted test set.

This indicates that geometric transformations helped the model handle some of the spatial changes introduced in the shifted test data.

---

## 🔍 Class-wise Analysis

The final model was further analyzed using:

- Per-class accuracy
- Normalized confusion matrix
- Misclassified examples

### Mean Class Accuracy

| Class | Accuracy |
|---|---:|
| Airplane | 44.07% |
| Automobile | 59.67% |
| Bird | 31.93% |
| Cat | 30.33% |
| Deer | 29.83% |
| Dog | 27.30% |
| Frog | 52.80% |
| Horse | 50.43% |
| Ship | 58.40% |
| Truck | 42.53% |

### Major Confusion Patterns

Some of the most common class confusions were:

- Truck → Automobile
- Dog → Cat
- Airplane → Ship
- Ship → Airplane
- Bird → Deer
- Cat → Dog
- Deer → Frog
- Deer → Horse

These confusions are understandable because several CIFAR-10 classes have visually similar shapes, colors, or backgrounds at the low **32 × 32** image resolution.

---

## 🧩 Distribution Shift

The shifted test set introduced transformations such as:

- Random rotation up to ±25°
- Brightness changes
- Contrast changes
- Approximately 25% rectangular occlusion

The shifted-test performance was consistently lower than clean-test performance.

This indicates a **distribution shift** between the training/clean-test data and the shifted test distribution.

The large training-validation gap also indicates **overfitting**, especially because only 400 images were available for training.

---

## ⚠️ Hidden Corruption

The assignment also requires evaluation on a hidden corruption.

The exact hidden-corruption transformation was not provided in the available instructions, so a specific corruption such as grayscale or channel swapping was **not assumed**.

Therefore, the numerical hidden-corruption result should only be added once the exact instructor-provided corruption is known.

---

## 🚀 Test-Time Augmentation

Test-Time Augmentation (TTA) was evaluated by averaging predictions from multiple transformed versions of each test image.

The evaluated transformations included:

- Original image
- Horizontal flip
- Small left shift
- Small right shift
- Small upward shift
- Small downward shift

### Results

| Metric | Normal | TTA |
|---|---:|---:|
| Mean Shifted Accuracy | 21.94% | 21.60% |
| Standard Deviation | 4.84% | 5.55% |

Overall improvement:

> **−0.34 percentage points**

TTA increased inference cost without improving the overall shifted-test accuracy.

The measured inference time was approximately:

- Normal inference: **2.67 seconds**
- TTA inference: **11.99 seconds**
- Approximately **4.49×** higher measured inference time

Therefore, TTA was not beneficial for this particular model and dataset.

---

## 🏆 Key Findings

### Best Clean Performance

**No Augmentation**

- Validation: **52.33 ± 0.58%**
- Clean Test: **42.73 ± 0.72%**

### Best Shifted-Test Performance

**Geometric Augmentation**

- Shifted Test: **27.91 ± 2.44%**

### Largest Robustness Improvement

Geometric augmentation provided the largest improvement over the final no-augmentation model:

> **+5.97 percentage points**

### TTA

TTA did not improve overall shifted-test performance and significantly increased inference cost.

---

## 🛠️ Technologies Used

- Python
- PyTorch
- Torchvision
- NumPy
- Pandas
- Matplotlib
- Scikit-learn
- Google Colab
- CIFAR-10

---


## ▶️ How to Run

### 1. Clone the repository

```bash
git clone https://github.com/rimshabash/CIFAR10-Robustness-Analysis.git
cd cifar10-robustness-analysis
```

### 2. Install dependencies

```bash
pip install torch torchvision numpy pandas matplotlib scikit-learn
```

### 3. Run the notebook

Open:

```text
notebooks/CIFAR10_Robustness_Analysis.ipynb
```

The notebook can be executed in **Google Colab** with GPU support.

---

## 🔁 Reproducibility

Three random seeds were used:

```text
42
123
2026
```

Results are reported as:

> **Mean ± Standard Deviation**

A fixed random seed was also used when creating the 500-image training subset and its train/validation split.

---

## 📌 Conclusion

This project demonstrates that training a CNN with only **500 CIFAR-10 images** creates a challenging learning environment with significant overfitting and sensitivity to distribution shifts.

The experiments show that the best configuration for validation and clean-test accuracy was the **No Augmentation** model, while **Geometric Augmentation** provided the strongest shifted-test robustness.

The results highlight an important trade-off between **standard accuracy and robustness**, showing that the configuration with the highest clean accuracy is not necessarily the most robust to distribution shifts.

---

## 👩‍💻 Author

**Rimsha Bashir**

BS Computer Science  
FAST National University of Computer & Emerging Sciences (FAST-NUCES)

---
