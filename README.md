# Semantic Alignment of fMRI Data Using CLIP

1. Problem Statement

The objective of this task was to explore the provided fMRI dataset and implement a CLIP-based contrastive tuning approach to align the fMRI data with semantic information derived from images or textual descriptions. The goal was to determine an effective alignment between fMRI signals and meaningful image representations, allowing us to understand the correlations between visual stimuli and brain activity. The dataset used for this task was obtained from the BOLD5000 project, containing a large number of fMRI scans corresponding to various visual stimuli.

2. Data Understanding and Preprocessing

The BOLD5000 dataset contains fMRI scans recorded from subjects viewing different images. Each scan represents the brain's response to a specific visual stimulus. The data structure is a four-dimensional array, with three spatial dimensions and one temporal dimension corresponding to each voxel in the brain over time. The primary challenge with the fMRI data was the presence of high-dimensional information, along with potential missing values (NaNs) and variability across the dataset.

To prepare the data for alignment with CLIP, several preprocessing steps were performed:

Flattening the Data: The 4D fMRI data was flattened to create a 2D structure, where each row represents a voxel's response over time. This was essential for applying further transformations and reducing data dimensionality.

Handling Missing Values: Missing values were handled by using a mean imputation strategy, replacing NaNs with the mean value of the corresponding feature.

Dimensionality Reduction: Due to the high dimensionality of the fMRI data, Principal Component Analysis (PCA) was used to reduce the number of features to 50 components. This helped to mitigate computational load and focused the analysis on the most significant variance in the data.

An example visualization of a slice from the fMRI data is provided below to confirm that the data was correctly loaded:

![image](https://github.com/user-attachments/assets/0b1cd62e-fd35-462a-8f4a-15fff7c10f6b)


The above image shows example slices from different runs of the fMRI data, which helped verify the correctness of the data loading process.

3. Methodology

For semantic alignment, we chose to focus on image data instead of textual descriptions. The decision was based on the nature of the BOLD5000 dataset, which explicitly provides visual stimuli corresponding to fMRI scans. By using images, we could more directly evaluate the correlation between visual inputs and brain responses. The CLIP (Contrastive Language-Image Pre-training) model was utilized to generate semantic embeddings for the images, which were then aligned with the fMRI data.

Model Architecture and Alignment Process

Feature Extraction and Normalization: fMRI features were extracted after PCA reduction and then standardized to ensure that both fMRI and image embeddings were on comparable scales. The image features were generated using the CLIP model's image encoder and similarly normalized.

Linear Transformation for Dimensional Alignment: A linear layer was implemented to transform the fMRI features to match the dimensionality of the CLIP image embeddings. This transformation included two fully connected layers with a ReLU activation function in between, allowing the model to capture more complex relationships between fMRI and image embeddings.

Cosine Similarity and Contrastive Loss: The primary objective was to maximize the cosine similarity between the transformed fMRI features and the corresponding image embeddings. For training, a Cosine Embedding Loss was used, which is well-suited for this type of semantic alignment task.

4. Results

The training process was conducted over 20 epochs, and the loss curve is presented below. Initially, the loss was relatively high, but it gradually decreased, eventually converging to near zero. This indicates that the model was able to effectively align the fMRI features with the semantic image embeddings, demonstrating successful learning of the relationships between brain activity and visual stimuli.



Epochs: The x-axis represents the number of training epochs (0 to 20).

Loss: The y-axis represents the training loss, which shows a steep decline during the first few epochs, followed by a more gradual decline.

The final loss values indicate a good level of alignment, with the model able to capture correlations between fMRI data and semantic image information.

Epoch [1/20], Loss: 1.0316689014434814

Epoch [2/20], Loss: 0.6602054834365845

Epoch [3/20], Loss: 0.45991581678390503

Epoch [4/20], Loss: 0.424766480922699

Epoch [5/20], Loss: 0.24030500650405884

Epoch [6/20], Loss: 0.12421280145645142

Epoch [7/20], Loss: 0.07311588525772095

Epoch [8/20], Loss: 0.046590566635131836

Epoch [9/20], Loss: 0.02980726957321167

Epoch [10/20], Loss: 0.022791564464569092

Epoch [11/20], Loss: 0.020026862621307373

Epoch [12/20], Loss: 0.018105506896972656

Epoch [13/20], Loss: 0.016220033168792725

Epoch [14/20], Loss: 0.014258623123168945

Epoch [15/20], Loss: 0.012210428714752197

Epoch [16/20], Loss: 0.010184764862060547

Epoch [17/20], Loss: 0.008358955383300781

Epoch [18/20], Loss: 0.006860613822937012

Epoch [19/20], Loss: 0.0057160258293151855

Epoch [20/20], Loss: 0.004875659942626953

![image](https://github.com/user-attachments/assets/2105a83a-aa67-4b7f-9c40-65d42390aac9)

5. Challenges and Solutions

High Dimensionality: The original fMRI data was highly dimensional, which posed computational challenges. This was mitigated by using PCA for dimensionality reduction, which significantly improved the efficiency of the training process.

Mismatch in Feature Ranges: The difference in feature ranges between fMRI and image embeddings initially led to high loss values. This was addressed by standardizing both sets of features before alignment, ensuring they were on comparable scales.

Complexity of Mapping Brain Activity to Semantics: Aligning fMRI data with semantic information is inherently challenging due to the complexity of brain responses. To address this, we used a multi-layer linear transformation with a non-linear activation function, which improved the model's ability to capture complex relationships.

6. Justification for Image and Dataset Selection

Image Selection: The image used for alignment was 'COCO_train2014_000000211198.jpg'. This specific image was chosen because it represents a rich and varied visual scene, which allows us to evaluate how different aspects of a complex visual input are processed by the brain. The selected image contains multiple objects and visual elements, making it suitable for assessing the model's ability to align diverse visual features with corresponding brain activity.

fMRI Dataset Selection: The fMRI dataset used for this alignment was 'CSI1_GLMbetas-TYPED-FITHRF-GLMDENOISE-RR_ses-01.nii'. This particular dataset was chosen because it represents a preprocessed version of the fMRI data, including HRF optimization, GLMdenoise, and Ridge regression (RR), which ensures cleaner signals that are better suited for alignment tasks. The use of a denoised and GLM-fitted dataset helps in reducing noise and improving the reliability of the feature extraction process.


7. Conclusion

The semantic alignment of fMRI data using CLIP-based contrastive tuning was successful in correlating brain responses with visual stimuli. Through careful preprocessing, dimensionality reduction, and feature transformation, we were able to train a model that effectively aligned fMRI data with image embeddings. The resulting loss curve and cosine similarity values indicate a meaningful alignment, which can serve as a foundation for further analysis in understanding brain-visual relationships.

Further improvements could involve experimenting with different non-linear models, larger batch sizes (if computationally feasible), or incorporating temporal dynamics of fMRI data to better capture time-varying aspects of brain responses.
