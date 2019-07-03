# Machine Learning Engineer Nanodegree
## Capstone Proposal
Michel Moreau
July 2nd, 2019

## Proposal  
[**CellSignal** - _Disentangling biological signal from experimental noise in cellular images_](https://www.rxrx.ai/)

### Introduction
My final project is to participale in the NeurIPS competition on Kaggle called **CellSignal** - _Disentangling biological signal from experimental noise in cellular images_. More information about this competition is available here:  
- Competition's website https://www.rxrx.ai  
- Kaggle competition's link: https://www.kaggle.com/c/recursion-cellular-image-classification/overview

Full disclosure: Some text in this proposal will be taken word for word from the competition's websites.

### Domain Background  
Recursion Pharmaceuticals, creators of the industry’s largest dataset of biological images, generated entirely in-house, believes AI has the potential to dramatically improve and expedite the drug discovery process. More specifically, machine learning could help understand how drugs interact with human cells.

This competition is designed to disentangle experimental noise from real biological signals. The goal is to classify images of cells under one of 1,108 different genetic perturbations, and thus eliminate the noise introduced by technical execution and environmental variation between \[drug\] experiments.

This is a multiclass classification challenge applied to a healthcare related topic. Other papers have been published in the past in this broad field:  
- https://arxiv.org/abs/1903.10035
- https://onlinelibrary.wiley.com/doi/abs/10.1002/mrm.27229  
- https://pdfs.semanticscholar.org/2583/2df680518e0b36734f54fb640668fe834be8.pdf  
- https://onlinelibrary.wiley.com/doi/full/10.1002/mrm.26841  

### Problem Statement
This is a multiclass classification problem.  

The inputs will be 512x512 pixels images and the output is a genetic perturbation (siRNA) represented by an integer ranging from 1 to 1,108.

> below is an input example  
![cells in a well](https://raw.githubusercontent.com/michelml/ml-cellsignal/master/example_input.png)

> below is an output example  

```
922
```

### Datasets and Inputs  
![the dataset](https://raw.githubusercontent.com/michelml/ml-cellsignal/master/dataset_description.png)

The data is available on the Kaggle's competition site https://www.kaggle.com/c/recursion-cellular-image-classification/data . For more information about the dataset, see the [competition's website](https://rxrx.ai).

The input images are all 512x512 pixels black and white.  

```sh  
identify example_input.png  
# --> example_input.png PNG 512x512 512x512+0+0 8-bit Gray 256c 58265B 0.000u 0:00.014
```  

Along with images, three other features are provided:  
- `experiment`: the cell type and batch number  
- `plate`: plate number within the experiment  
- `well`: location on the plate

The outcome variable is siRNA, which is a multiclass variable which can be an integer ranging from 1 to 1,108, each integer representing a different genetic disruption.

The data is already splitted into training and test sets. Further splitting will be considered if needed.

##### Some context

One of the main challenges for applying AI to biological microscopy data is that even the most careful replicates of a process will not look identical. This dataset challenges you to develop a model for identifying replicates that is robust to experimental noise.

The same siRNAs (effectively genetic perturbations) have been applied repeatedly to multiple cell lines, for a total of 51 experimental batches. Each batch has four plates, each of which has 308 filled wells. For each well, microscope images were taken at two sites and across six imaging channels. Not every batch will necessarily have every well filled or every siRNA present.



### Solution Statement  
The solution for this problem will likely be resolved with the type of model architecture used in computer vision and image classification, e.g convolutional neural networks. This is a multiclass classification problem, but algorithms and model architectures we have seen in the dogs classification project https://github.com/MichelML/udacity-dog-project/, such as VGG-16 and ResNet-50, will be considered for this project.

### Benchmark Model
As discussed previously, our solution will most likely use a convolutional neural network architecture. We will thus use a vanilla CNN model as our benchmark, such as the one we built during step 3 of the dogs classification project https://github.com/MichelML/udacity-dog-project/blob/master/dog_app.ipynb .  

To justify our decision to go along with a CNN architecture, it is to be said ResNet-50 have achieved very good results in multiclass image classification problems in the recent past \([see source](https://arxiv.org/abs/1903.10035)\), having reached 98.87% accuracy when classifying histopathology images.

### Evaluation Metrics
Submissions will be evaluated on Multiclass Accuracy, which is simply the average number of observations with the correct label.

[It is okay to stick with accuracy in our context](https://towardsdatascience.com/accuracy-paradox-897a69e2dd9b), as each class has a roughly equal number of data points.

#### Submission File
For each id_code in the test set, we will predict the correct siRNA. As per the competition's indications, The file should contain a header and have the following format:

```  
id_code,sirna
HEPG2-08_1_B03,911
HEPG2-08_1_B04,911
etc.   
```

### Project Design
**N.B. This design is inspired by Appendix B of the book  [Hands-On Machine Learning with Scikit-Learn and TensorFlow: Concepts, Tools](https://www.amazon.ca/Hands-Machine-Learning-Scikit-Learn-TensorFlow/dp/1491962291) by Aurélien Géron**  

1. Get the data
2. Explore the data to gain insights.  
3. Prepare the data to better expose the underlying data patterns to Machine Learning algorithms.  
4. Explore many different models and short-list the best ones.  
5. Fine-tune your models and combine them into a great solution.  

#### Get the data   
1. Get the data from the Kaggle's competition.  
2. Convert the data to a format we can easily manipulate (without changing the data itself).  

#### Explore the data  
1. Create a Jupyter notebook to keep record of our data exploration.  
2. Study each attribute and its characteristics:  
    - Name  
    - Type (categorical, int/float, bounded/unbounded, text, structured, etc.)
    - % of missing values  
    - Noisiness and type of noise (stochastic, outliers, rounding errors, etc.)
    - Possibly useful for the task?  
    - Type of distribution (Gaussian, uniform, logarithmic, etc.)
3. Visualize the data.  
4. Identify the promising transformations we may want to apply.  
5. Document what we have learned.  

#### Prepare the data  
1. Data cleaning:  
    - Fix or remove outliers (optional).  
    - Fill in missing values (e.g., with zero, mean, median...) or drop their rows (or columns).  
2. Feature selection (optional):  
    - Drop the attributes that provide no useful information for the task.  
3. Feature engineering, where appropriates:  
    - Discretize continuous features.  
    - Decompose features (e.g., categorical, date/time, etc.).  
    - Add promising transformations of features (e.g., log(x), sqrt(x), x^2, etc.).
    - Aggregate features into promising new features.  

#### Short-list promising models  
1. Train many quick and dirty models for each CNN architectures identified, using standard parameters.
2. Measure and compare their performance.  
    - For each model, use N-fold cross-validation and compute the mean and standard deviation of their performance. 
3. Analyze the most significant variables for each algorithm.  
4. Analyze the types of errors the models make.  
    - What data would a human have used to avoid these errors?  
5. Short-list the top promising models, preferring models that make different types of errors.  

#### Fine-Tune the System  
1. Fine-tune the hyperparameters using cross-validation.  
2. Try Ensemble methods. Combining our best models will often perform better than running them invdividually.  
3. Once we are confident about our final model, measure its performance on the test set to estimate the generalization error.

#### Iterate
Repeat any step necessary until the competition is still open, to climb into the Kaggle ladder.

-----------  

