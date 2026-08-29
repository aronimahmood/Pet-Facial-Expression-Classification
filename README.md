# Pet Facial Expression Classifier

A convolutional neural network (CNN) for classifying facial expressions in pet and animal images into three categories:

- Angry
- Sad
- Happy

The project investigates whether facial-expression classification is feasible for animals such as dogs, sheep, and goats using a relatively small image dataset.

## Project Overview

Animal facial expressions can be difficult to classify because different species have different facial structures and because emotion labels are often visually ambiguous. This project compares two CNN models:

1. A basic CNN without regularization
2. A regularized CNN using dropout, L2 weight decay, data augmentation, and label smoothing

The original dataset included an `Other` class. This class was removed because it did not have consistent visual characteristics and made the classification task less well-defined.

## Dataset

The final dataset contains three expression classes:

| Class | Description |
|---|---|
| `Angry` | Images labeled as showing an angry or aggressive expression |
| `Sad` | Images labeled as showing a sad or subdued expression |
| `Happy` | Images labeled as showing a happy or positive expression |

The evaluation set contained approximately:

- 54 Angry images
- 54 Sad images
- 55 Happy images

The dataset is relatively small, which limits the model's ability to learn generalizable visual features.

## Models

### Basic CNN

The basic CNN was trained without the regularization techniques used in the final model. Its purpose was to provide a baseline for comparison.

### Regularized CNN

The regularized model used:

- Dropout
- L2 weight decay
- Data augmentation
- Label smoothing
- Hyperparameter tuning

These techniques were used to reduce overfitting and prevent the model from collapsing into a single-class prediction.

## Results

| Model | Accuracy | Macro Precision | Macro Recall | Macro F1 |
|---|---:|---:|---:|---:|
| Basic CNN | 33.13% | -- | -- | -- |
| Regularized CNN | 49.69% | 0.514 | 0.497 | 0.492 |

Since this is a three-class classification problem, a uniform random classifier would achieve approximately 33.3% accuracy.

The basic CNN achieved 33.13% accuracy and predicted most images as `Sad`, indicating class collapse and limited learning of meaningful visual features.

The regularized CNN achieved 49.69% accuracy, which is substantially better than the random baseline. Although the performance is moderate, the model learned features that allowed it to distinguish between some of the expression categories.

## Confusion Matrix Analysis

The regularized CNN produced the following results:

| True class | Correct | Predicted as another class |
|---|---:|---|
| Angry | 36/54 | 9 Sad, 9 Happy |
| Sad | 22/54 | 21 Angry, 11 Happy |
| Happy | 23/55 | 24 Angry, 8 Sad |

The model performed best on the `Angry` class. The `Sad` and `Happy` classes were more difficult to distinguish, suggesting that these expressions have considerable visual overlap.

The basic CNN showed severe class collapse:

- All Angry images were predicted as Sad
- All Sad images were predicted as Sad
- All Happy images were predicted as Sad

This behavior demonstrates the importance of regularization when training CNNs on small datasets.

## Hyperparameter Tuning

A grid search was used to evaluate different learning rates and batch sizes.

The best-performing combination was:

```text
Learning rate: 0.001
Batch size: 32

Lower learning rates, such as 0.0001 and 0.0003, resulted in slower convergence and lower performance. A higher learning rate of 0.003 caused unstable training and poorer generalization.

A batch size of 32 provided the best balance between gradient stability and generalization among the tested configurations.
Effect of Removing the Other Class

The original dataset included an Other category, but this class was difficult to define consistently. Removing it improved validation accuracy from approximately 35% to approximately 49%.

This suggests that poorly defined or noisy labels can make it harder for a supervised learning model to learn clear class boundaries. Removing the ambiguous class created a more focused three-class problem.

However, removing Other does not necessarily mean that the model has learned animal emotions reliably. It only means that the remaining labels produced a more consistent classification task.
Limitations

The current model has several limitations:

    The dataset is small and may not represent the full range of animal appearances, breeds, poses, and lighting conditions.
    Facial expressions can be ambiguous and may vary significantly between species.
    The labels may reflect human interpretation rather than objectively verified animal emotional states.
    The model was trained from scratch and therefore did not benefit from pre-trained visual features.
    Performance remains moderate at approximately 50% accuracy.
    Images from different animal species may require species-specific models or additional metadata.
    Accuracy alone may not fully describe performance because the model can still confuse visually similar classes.

The predictions should therefore be interpreted as visual expression classifications rather than reliable measurements of an animal's actual emotional state.
Future Work

Possible improvements include:

    Applying transfer learning with models such as ResNet, VGG, EfficientNet, or MobileNet
    Expanding the dataset with more diverse and higher-quality images
    Increasing the number of examples for each animal species
    Using species-specific classification models
    Improving annotation guidelines and obtaining multiple annotators per image
    Applying additional augmentation techniques
    Using more systematic hyperparameter optimization
    Evaluating precision, recall, F1-score, and balanced accuracy per class
    Testing the model on a separate dataset from a different source
    Comparing the CNN with modern vision transformer models

Conclusion

The experiments show that regularization and dataset refinement had a greater effect on performance than increasing model depth alone.

Removing the ambiguous Other class produced a clearer classification task, while dropout, L2 weight decay, data augmentation, and label smoothing helped prevent class collapse. The regularized CNN achieved 49.69% accuracy compared with 33.13% for the basic CNN.

These results suggest that animal facial-expression classification is possible to some extent, but reliable performance requires a larger, better-labeled, and more diverse dataset.
Technologies

    Python
    TensorFlow/Keras or PyTorch
    NumPy
    Pandas
    Matplotlib
    scikit-learn

Project Structure
text

.
├── data/
├── notebooks/
├── src/
├── models/
├── results/
├── requirements.txt
└── README.md

Installation

Clone the repository:
git clone https://github.com/your-username/pet-expression-classifier.git
cd pet-expression-classifier



Install the dependencies:

pip install -r requirements.txt

Usage:

Update the dataset path in the configuration or training script, then run:

python train.py

To evaluate a trained model:
python evaluate.py


python evaluate.py

The evaluation script reports accuracy, precision, recall, F1-score, and the confusion matrix
