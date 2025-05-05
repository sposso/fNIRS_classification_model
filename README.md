# fNIRS_classification_model
This repostiry contains the code required to train an SVM and LDA model to distinguish between perceived speech and silence from fNIRS  signals.
The data used is a publicly available dataset from the study "The use of broad vs restricted regions of interest in functional near-infrared spectroscopy for measuring cortical activation to auditory-only and visual-only speech". For more details about the data, you can check this [description](https://github.com/sposso/fNIRS-preprocessing-guide). 

## Experiment settings 
The dataset comprises auditory-only and resting condition classes, with 18 and 10 trials, respectively. To address this class imbalance, we applied the Adaptive Synthetic (ADASYN) sampling method \cite{he2008adaptive} to oversample the minority class. Given the imbalance, model performance was evaluated using the AUC metric instead of accuracy, as AUC is more sensitive to false positives and negatives.

To train the decoders, we used the implementation provided by Scikit-learn.. The regularization parameter values tested for the SVC  were 0.001, 0.01, 0.1, and 1, with a maximum of 250,000 iterations to ensure convergence. We adopted the machine learning methodology outlined [here](https://doi.org/10.3389/fnrgo.2023.994969) to evaluate the models, employing a nested cross-validation approach. The outer cross-validation consisted of five-fold cross-validation to define the test set. Meanwhile, the inner cross-validation, designed for hyperparameter tuning, used a group of three-fold cross-validation to separate the training and validation sets. Following the statistical recommendations for reporting results outlined [here](https://doi.org/10.3389/fnrgo.2023.994969), we evaluated the performance of each model using a one-tailed t-test to determine whether its accuracy exceeded the chance level. We set the significance level to 0.05.

![Results of the SVC and LDA models](https://github.com/sposso/fNIRS_classification_model/blob/main/0_18/_folder_subj_0/summary.png)


### 📈 AUC Table

(Standard deviation from cross-validation)

| Dataset     | Chance level | LDA (sd)     | SVC (sd)     |
|:-----------:|:------------:|:------------:|:------------:|
| audio_study | 0.5          | 0.792 (0.167)| 0.758 (0.172)|
