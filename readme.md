# Quick Start
To start training, run these sections in order:
1. **Setup**: Imports libraries and configures paths. Run all the cells under the Setup header (up until visualization BEFORE preprocessing).
2. **Configuration for Block SVD**: Sets the compression parameters ($k$, block size, etc.).
3. **Load Preprocessed Features**: Loads the pre-calculated data tensors.

## Troubleshooting
**Path Errors:** If the "Setup" cell fails, check the `dataset_path` variable. Make sure it points to wherever you saved the `train`, `valid`, and `test` folders on your own computer.
**Missing Files:** If "Load Preprocessed Features" fails, ensure you have the `.pkl` or `.pt` files in your project directory. Put it inside the dataset folder!
# Preprocessing Pipeline
## 1. Standardization for image sizes
- Since the images in the dataset have varying sizes, I resized them to $128 \times 128$ pixels
- To calculate the original number of features before we applied any compression (SVD):$$\text{Total Features} = \text{Height} \times \text{Width} \times \text{Channels}$$
- 128 x 128 x 3 = 49,512 features per image.

# 2. Block SVD
- Here, we perform dimensionality reduction by dividing the image into grids (blocks), so it focuses on the local textures rather than just the overall image.
- Regarding $k$:
	**What is it?** It represents how many "features" we keep from each block. If its too low, images become blurry.
	
	**How to find?** By using an empirical approach, tested 50 sample images from the training set and use SSIM as the primary quality metric to ensure image quality is similar to BEFORE preprocessing.
	- Extract 450 random blocks (50 images × 3 RGB channels × 3 blocks per channel)
	- For each block, find the minimum k that achieves SSIM ≥ 0.90 (90%)
	- The k values ranged from 5 to 15, and the median was 10.
![](attachment/802baa14d022cca130b60b2b77f2c7a8.png)
![](attachment/510a44c6b0c9533dc818a1010e7ce835.png)

---
# Extra
Here I was testing to find how many components for PCA we can use to avoid images becoming blurry. 

using 1000 components![](attachment/deb812f5e4758b1e5ab27eb9bb749202.png)

using 4350 components  
![](attachment/76c3f261c81177a5c0208bf29337a135.png)

using 5854 components
![](attachment/418c44516ea834b75511175dd84858c7.png)