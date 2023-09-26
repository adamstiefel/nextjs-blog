---
title: "Machine Learning - Ensemble Bagging Hands On"
date: "2023-09-21"
---

Ensemble Hands On - Bagging

We will build a model to predict if someone who is looking for a loan will default or not. Several independent variables are available like checking account balance, credit history, purpose, loan amount etc.

We will talk about bagging and random forests. We will cover hyper tuning and different base estimator in bagging. Are target for predictions will be accuracy and recall.

First we have to import the neccassary libraries.

import pandas as pd
import numpy as np
from sklearn import metrics
import matplotlib.pyplot as plt
%matplotlib inline
import warnings
warnings.filterwarnings('ignore')
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.model_selection import GridSearchCV
from sklearn import metrics
from sklearn.ensemble import BaggingClassifier, RandomForestClassifier
from sklearn.linear_model import LogisticRegression

Then we will load the data and read the first 10 rows. 

url = "credit.csv"
creditData = pd.read_csv(url)

#creditData = pd.read_csv("credit.csv")
creditData.head(10) #several missing values!


Then we check how many rows and columns the data set contains.

creditData.shape

We check the values and common those values occur for default column.

creditData['default'].value_counts()

Now we will skip ahead to after we have cleaned up the data and we are ready to set it up for analysis

First we will split the data into a train and test set

Drop target from x and add target to y

X = creditData.drop("default" , axis=1)
y = creditData.pop("default")


Stratify allows us to create training and test sets in a consistent way in the way they are split in terms of the target variable. Maintatain the ratio of the target variable distribution among values. This is more important the more imbalanced your data is. Y in this case refers to the target variable.

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=.30, random_state=1,stratify=y)

Then we create a function to create a confusion matrix to test how are various models are predicting. Another function to get numerical score of accuracy, recall, and precision.

Now all of the housekeeping code is done and we will start building the model!

Building the Model

We will build two ensemble models - a bagging classifier and random forest classifier. We will first build with default parameters then use hyperparameter tuning in order to optimize model performance. The primary metric we are targeting is recall but we will also calculate accuracy and precision. 

Recall is the ratio of true positives to actual positives. High recall implies a low amount of false negatives. Which in our case means low chances of predicting a defaulter as a non defaulter.

Bagging Classifier

First create estimator then train estimator with training data.

#base_estimator for bagging classifier is a decision tree by default
bagging_estimator=BaggingClassifier(random_state=1)
bagging_estimator.fit(X_train,y_train)


We run this code to see how it does. We notice that it is overfit since it performs much better on training data than on test data.

#Using above defined function to get accuracy, recall and precision on train and test set
bagging_estimator_score=get_metrics_score(bagging_estimator)


Visualize confusion matrix to see actual yes and no vs predicted.

make_confusion_matrix(bagging_estimator,y_test)


Random Forest Classifier

Now we try a random forest classifier.

First we initiliaze the estimator. Then we train the estimator on the training data

#Train the random forest classifier
rf_estimator=RandomForestClassifier(random_state=1)
rf_estimator.fit(X_train,y_train)


Then we see how it performs in terms of accuracy, recall, and precision. It also overfits the data.

#Using above defined function to get accuracy, recall and precision on train and test set
rf_estimator_score=get_metrics_score(rf_estimator)

Then we see how it does in terms of the confusion matrix to see predicted vs actual yes and no's.

make_confusion_matrix(rf_estimator,y_test)


Hyperparameter Tuning

This is a way to try to improve models to get better results.

Bagging Classifier - Hyperparameter Tuning 

base_estimator: The base estimator to fit on random subsets of the dataset. If None(default), then the base estimator is a decision tree.
- Different models


n_estimators: The number of trees in the forest, default = 100.
- Number of models

max_features: The number of features to consider when looking for the best split.
- percent number
- 1 = 100%
- What fraction of features should it consider for splitting

bootstrap: Whether bootstrap samples are used when building trees. If False, the entire dataset is used to build each tree, default=True.
- Sampling with replacement

bootstrap_features: If it is true, then features are drawn with replacement. Default value is False.
- Bootstrapping on the columns as well

max_samples: If bootstrap is True, then the number of samples to draw from X to train each base estimator. If None (default), then draw N samples, where N is the number of observations in the train data.
- How many samples do we choose if bootstrapping
- Typically same # of samples as # of data points

oob_score: Whether to use out-of-bag samples to estimate the generalization accuracy, default=False.
- Can I see how well method does only with training data and not testing data
- Questionable measure of success


We will play with max_samples, max_features, and n_estimators. We create estimator. Create parameters for us to tune. Score theme. Run grid search. Set to best clf then train.

# Choose the type of classifier. 
bagging_estimator_tuned = BaggingClassifier(random_state=1)

# Grid of parameters to choose from
## add from article
parameters = {'max_samples': [0.7,0.8,0.9,1], 
              'max_features': [0.7,0.8,0.9,1],
              'n_estimators' : [10,20,30,40,50],
             }

# Type of scoring used to compare parameter combinations
acc_scorer = metrics.make_scorer(metrics.recall_score)

# Run the grid search
grid_obj = GridSearchCV(bagging_estimator_tuned, parameters, scoring=acc_scorer,cv=5)
grid_obj = grid_obj.fit(X_train, y_train)

# Set the clf to the best combination of parameters
bagging_estimator_tuned = grid_obj.best_estimator_

# Fit the best algorithm to the data.
bagging_estimator_tuned.fit(X_train, y_train)

Then we get scoring metrics

#Using above defined function to get accuracy, recall and precision on train and test set
bagging_estimator_tuned_score=get_metrics_score(bagging_estimator_tuned)

Then get confusion matrix

make_confusion_matrix(bagging_estimator_tuned,y_test)


Hypertuning in this case overfit the model. Careful to also not overfit testing data as well.

Lets try switching base estimator to logistic regression for bagging classifier.


bagging_lr=BaggingClassifier(base_estimator=LogisticRegression(solver='liblinear',random_state=1,max_iter=1000),random_state=1)
bagging_lr.fit(X_train,y_train)


Not overfitting but is not doing well. Looks like it could be underfitting. Not fitting noise.
#Using above defined function to get accuracy, recall and precision on train and test set
bagging_lr_score=get_metrics_score(bagging_lr)


Confusion matrix.

make_confusion_matrix(bagging_lr,y_test)

Random Forest Classifier

Now lets tune the random forest classifier to try to get better results.

Most classifiers are exactly the same.

n_estimators: The number of trees in the forest, default = 100.

max_features: The number of features to consider when looking for the best split.

class_weight: Weights associated with classes in the form {class_label: weight}.If not given, all classes are supposed to have weight one.
- This one is unique
- Weigh Importanance

For example: If the frequency of class 0 is 80% and the frequency of class 1 is 20% in the data, then class 0 will become the dominant class and the model will become biased toward the dominant classes. In this case, we can pass a dictionary {0:0.2,1:0.8} to the model to specify the weight of each class and the random forest will give more weightage to class 1.

bootstrap: Whether bootstrap samples are used when building trees. If False, the entire dataset is used to build each tree, default=True.

max_samples: If bootstrap is True, then the number of samples to draw from X to train each base estimator. If None (default), then draw N samples, where N is the number of observations in the train data.

oob_score: Whether to use out-of-bag samples to estimate the generalization accuracy, default=False.

Note: A lot of hyperparameters of Decision Trees are also available to tune Random Forest like max_depth, min_sample_split etc.


First we make classifer. Then we set parameters to tweak. Then get score. Then run grid search. The get best combo of params. Then train best fit algo.
# Choose the type of classifier. 
rf_estimator_tuned = RandomForestClassifier(random_state=1)

# Grid of parameters to choose from
## add from article
parameters = {"n_estimators": [150,200,250],
    "min_samples_leaf": np.arange(5, 10),
    "max_features": np.arange(0.2, 0.7, 0.1),
    "max_samples": np.arange(0.3, 0.7, 0.1),
             }

# Type of scoring used to compare parameter combinations
acc_scorer = metrics.make_scorer(metrics.recall_score)

# Run the grid search
grid_obj = GridSearchCV(rf_estimator_tuned, parameters, scoring=acc_scorer,cv=5)
grid_obj = grid_obj.fit(X_train, y_train)

# Set the clf to the best combination of parameters
rf_estimator_tuned = grid_obj.best_estimator_

# Fit the best algorithm to the data.
rf_estimator_tuned.fit(X_train, y_train)


Get metrics

#Using above defined function to get accuracy, recall and precision on train and test set
rf_estimator_tuned_score=get_metrics_score(rf_estimator_tuned)


We havent yet told are learning algorithm that recall is of upmost importance

This is where class_weights comes into play

Model performance has not been good which could be due to imbalance of 70% non-defaulters and 30% defaulters.

We will make the model aware that class of interest is defaulters.

Only major difference here is the class weight. We are setting it to the 30 70.

# Choose the type of classifier. 
rf_estimator_weighted = RandomForestClassifier(random_state=1)

# Grid of parameters to choose from
## add from article
parameters = {
    "class_weight": [{0: 0.3, 1: 0.7}],
    "n_estimators": [100,150,200,250],
    "min_samples_leaf": np.arange(5, 10),
    "max_features": np.arange(0.2, 0.7, 0.1),
    "max_samples": np.arange(0.3, 0.7, 0.1),
}

# Type of scoring used to compare parameter combinations
acc_scorer = metrics.make_scorer(metrics.recall_score)

# Run the grid search
grid_obj = GridSearchCV(rf_estimator_weighted, parameters, scoring=acc_scorer,cv=5)
grid_obj = grid_obj.fit(X_train, y_train)

# Set the clf to the best combination of parameters
rf_estimator_weighted = grid_obj.best_estimator_

# Fit the best algorithm to the data.
rf_estimator_weighted.fit(X_train, y_train)


Better performance. Not great.


Check feature importance

importances = rf_estimator_weighted.feature_importances_
indices = np.argsort(importances)
feature_names = list(X.columns)

plt.figure(figsize=(12,12))
plt.title('Feature Importances')
plt.barh(range(len(indices)), importances[indices], color='violet', align='center')
plt.yticks(range(len(indices)), [feature_names[i] for i in indices])
plt.xlabel('Relative Importance')
plt.show()

Compare all models until now get them all in a table

# defining list of models
models = [bagging_estimator,bagging_estimator_tuned,bagging_lr,rf_estimator,rf_estimator_tuned,
          rf_estimator_weighted]

# defining empty lists to add train and test results
acc_train = []
acc_test = []
recall_train = []
recall_test = []
precision_train = []
precision_test = []

# looping through all the models to get the accuracy, precall and precision scores
for model in models:
    j = get_metrics_score(model,False)
    acc_train.append(np.round(j[0],2))
    acc_test.append(np.round(j[1],2))
    recall_train.append(np.round(j[2],2))
    recall_test.append(np.round(j[3],2))
    precision_train.append(np.round(j[4],2))
    precision_test.append(np.round(j[5],2))


    comparison_frame = pd.DataFrame({'Model':['Bagging classifier with default parameters','Tuned Bagging Classifier',
                                        'Bagging classifier with base_estimator=LR', 'Random Forest with deafult parameters',
                                         'Tuned Random Forest Classifier','Random Forest with class_weights'], 
                                          'Train_Accuracy': acc_train,'Test_Accuracy': acc_test,
                                          'Train_Recall':recall_train,'Test_Recall':recall_test,
                                          'Train_Precision':precision_train,'Test_Precision':precision_test}) 
comparison_frame