# fNIRS_classification_model
This repository contains the code for training Support Vector Machine (SVM) and Linear Discriminant Analysis (LDA) models to classify perceived speech versus silence using fNIRS signals.
The data used is publicly available from the study ***"The use of broad vs restricted regions of interest in functional near-infrared spectroscopy for measuring cortical activation to auditory-only and visual-only speech"***. For more details about the dataset and the preprocessing steps, please refer to this [description](https://github.com/sposso/fNIRS-preprocessing-guide). 

## Experiment settings 
The dataset comprises auditory-only and resting condition classes, with 18 and 10 trials, respectively. To address this class imbalance, we applied the Adaptive Synthetic (ADASYN) sampling method to oversample the minority class. Given the imbalance, model performance was evaluated using the AUC metric instead of accuracy, as AUC is more sensitive to false positives and negatives.

To train the decoders, we used the implementation provided by Scikit-learn.. The regularization parameter values tested for the SVC  were 0.001, 0.01, 0.1, and 1, with a maximum of 250,000 iterations to ensure convergence. We adopted the machine learning methodology outlined [here](https://doi.org/10.3389/fnrgo.2023.994969) to evaluate the models, employing a nested cross-validation approach. The outer cross-validation consisted of five-fold cross-validation to define the test set. Meanwhile, the inner cross-validation, designed for hyperparameter tuning, used a group of three-fold cross-validation to separate the training and validation sets. Following the statistical recommendations for reporting results outlined [here](https://doi.org/10.3389/fnrgo.2023.994969), we evaluated the performance of each model using a one-tailed t-test to determine whether its accuracy exceeded the chance level. We set the significance level to 0.05.

### Selected time window 

We selected a time window that ranged from stimulus onset to 18 seconds. This time window was chosen to encompass the 12.5 seconds during which participants perceived the audio stimulus and the additional ~5 seconds required for the hemodynamic response to peak after stimulus onset

### Note: 

To get the results, you only have to run the Jupyter notebook **train_function.ipynb**

![Results of the SVC and LDA models](https://github.com/sposso/fNIRS_classification_model/blob/main/0_18/_folder_subj_0/summary.png)


### 📈 AUC Table (subject 0)
(Standard deviation (sd) from cross-validation)

| Dataset     | Chance level | LDA (sd)     | SVC (sd)     |
|:-----------:|:------------:|:------------:|:------------:|
| audio_study | 0.5          | 0.767 (0.162)| 0.758 (0.172)|

## Topographic map of the relevant features the linear support vector classifier ( SVC) learned.

We also visualize learned features from the linear SVC in a topographic map format, focusing on the time intervals where more feature coefficients > 0.8. To determine the relevant window time, we first sum the number of features with values $\geq 0.8 $ across the spatial dimension. Next, we apply a mean filter ( $window size = 2 s$) to smooth the temporal profile and compute the average counts over time. Finally, we identify the target interval by finding the intersection between 80 \% of the maximum value and the time axis. Notably, all participants exhibit asymmetrical patterns across both hemispheres, particularly in the relevant channels highlighted in red. This asymmetry suggests potential left lateralization in the decoding task's neural processes at specific times. We also see that the HbO features are most relevant when the stimulus ends, and the opposite is true for HbR features.

### Procedure

1. Reshape the learned features into a [channels × time points] format.![coefficient features learned by the linear SVC](https://github.com/sposso/fNIRS_classification_model/blob/main/Average_Figures_SVC/subject_0_weights.png)
2. Split the coefficient features into HbO and HbR components for separate analysis, normalize each to the [0, 1] range, and apply a threshold to identify coefficients greater than 0.80.
   ![Binarized hbo coefficients](https://github.com/sposso/fNIRS_classification_model/blob/main/Average_Figures_SVC/subject_0_binary_weights.png)
   ![Binarized hbr coefficients](https://github.com/sposso/fNIRS_classification_model/blob/main/Average_Figures_SVC/subject_0_binary_weights_hbr.png)

3. We first count the number of significant channels (coefficients > 0.8) and apply a mean filter with a 2-second window to smooth the temporal profile. Then, we compute the average count over time and identify the target interval by locating the intersection between 80% of the maximum value and the time axis.
   ![HbO threshold](https://github.com/sposso/fNIRS_classification_model/blob/main/Average_Figures_SVC/subject_0_Threshold_window_complete.png)
   ![HbR threshold](https://github.com/sposso/fNIRS_classification_model/blob/main/Average_Figures_SVC/subject_0_Threshold_window_complete.png)

4. Repeat the same procedure for each subject.

5. Based on the time windows identified for each subject, where feature values peak, we plotted the corresponding values on a topographic map. Channels highlighted in red indicate the most significant regions, corresponding to time intervals where the coefficient values are ≥ 0.8

   ![Topographic map](https://github.com/sposso/fNIRS_classification_model/blob/main/Average_Figures_SVC/_Attention_maps.png)
   


