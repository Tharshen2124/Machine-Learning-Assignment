# Machine Learning Assignment

This project is using the given dataset:

https://www.kaggle.com/datasets/nunenuh/pytorch-challange-flower-dataset

# Installation

## Prerequisites

* Python 3.13.4

## Setup

1. Create a virtual environment:

   ```bash
   python3 -m venv venv
   ```
2. Activate the virtual environment:
   **macOS/Linux:**

   ```bash
   source venv/bin/activate
   ```

   **Windows:**

   ```bash
   venv\Scripts\activate
   ```
3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

## Running the Notebook

After activating the virtual environment, launch Jupyter.

---

# Quick Start
**DOWNLOAD PREPROCESSED DATASET HERE:** https://drive.google.com/file/d/1NdNzIcfKYbauhbWqxVM6kM9WFYr8nlui/view?usp=sharing
To start training, run these sections in order:
1. **Setup**: Imports libraries and configures paths. Run all the cells under the Setup header (up until visualization BEFORE preprocessing).
2. **Configuration for Block SVD**: Sets the compression parameters ($k$, block size, etc.).
3. **Load Preprocessed Features**: Loads the pre-calculated data tensors.

## Expected output
<img width="1056" height="647" alt="image" src="https://github.com/user-attachments/assets/171ad996-f4c7-4344-8a24-2dae3df811f4" />
<img width="1055" height="782" alt="image" src="https://github.com/user-attachments/assets/6cb2dc10-6396-4b4b-9bcc-5742474c7a81" />

## Troubleshooting
**Path Errors:** If the "Setup" cell fails, check the `dataset_path` variable. Make sure it points to wherever you saved the `train`, `valid`, and `test` folders on your own computer.

**Missing Files:** If "Load Preprocessed Features" fails, ensure you have the `.pkl` files in your project directory. Put it inside the dataset folder!
# Preprocessing Pipeline

## 1. Standardization for image sizes
- Since the images in the dataset have varying sizes, I resized them to 128 128 pixels
- To calculate the original number of features before we applied any compression (SVD): Total Features = Height x Width x Channels
- 128 x 128 x 3 = 49,512 features per image.

## 2. Block SVD
- Here, we perform dimensionality reduction by dividing the image into grids (blocks), so it focuses on the local textures rather than just the overall image.
- Regarding $k$:
	**What is it?** It represents how many "features" we keep from each block. If its too low, images become blurry.
	
	**How to find?** By using an empirical approach, tested 50 sample images from the training set and use SSIM as the primary quality metric to ensure image quality is similar to BEFORE preprocessing.
	- Extract 450 random blocks (50 images × 3 RGB channels × 3 blocks per channel)
	- For each block, find the minimum k that achieves SSIM ≥ 0.90 (90%)
	- The k values ranged from 5 to 15, and the median was 10.
 -  After SVD compression, total features will go from **49,152** -> **31,200**
<img width="1140" height="507" alt="Screenshot 2026-01-22 122718" src="https://github.com/user-attachments/assets/01eb7a6c-057f-492c-9333-ff2b7a22793f" />
<img width="1081" height="567" alt="Screenshot 2026-01-22 113305" src="https://github.com/user-attachments/assets/cc12a259-ab85-4d39-b474-dfe3e8cc1853" />

---

# IMPORTANT NOTES:
I'm still testing the PCA function since our current features are still too big to feed into Logistic Regression and ANN. It IS possible to use them without but expect a longer training time.

<img width="1090" height="516" alt="Screenshot 2026-01-22 123729" src="https://github.com/user-attachments/assets/cccd95fc-5e63-4495-b657-accc67debbfc" />
<img width="1072" height="532" alt="Screenshot 2026-01-22 125344" src="https://github.com/user-attachments/assets/45025251-5a4b-40da-9d18-abdc58d7c134" />
<img width="1072" height="458" alt="Screenshot 2026-01-22 132516" src="https://github.com/user-attachments/assets/134f724a-0e97-40eb-93f4-cc39b6101f9a" />

So far, I managed to scale down the features to **5854** to maintain 98% variance. Can try and see if models have improved after this.


   
