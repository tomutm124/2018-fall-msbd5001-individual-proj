# 2018-fall-msbd5001-individual-proj

This is my solution for an in-class prediction competition for an introductory data science course that is part of my master's degree:
https://www.kaggle.com/c/msbd5001-fall2018

I got first place among more than 100 students. The goal of the competition was to predict the training time of a model on a specific machine given the model's training parameters and the parameters used to generate the training data (refer to the link above for details). We were given a training dataset that consists of rows of parameters and model training time, and were asked to predict the model training time for each row in a test dataset.

My solution is rather short. I partitioned the dataset by 2 ordinal features and then created a single feature that was expected to be linearly proportional to the training time in each partition. The feature is the "e_factor" below. Then I fit a linear line of (e_factor vs target) for each partition of data and used the fitted line to make predictions on the test dataset.

I believe this solution demonstrates the importance of understanding the data. I was told that a lot of students went straight to multivariate regression models using all parameters given in the dataset without trying to understand how each parameter is related to the target (model training time).


More details: (course requirements)

Programming language:
Python 3.6

Required packages:
numpy
pandas
matplotlib
seaborn
scipy
sklearn

How to run the code:
There are two Jupyter notebooks in this repo, each of them corresponds to one of my submissions selected for the private leaderboard. Just put the notebook in the same directory as test.csv and train.csv and then run it cell by cell from top to bottom to generate the predictions as a csv file.

Differences between the two notebooks:
The two notebooks are identical except for one single line: method2 dropped an outlier in cell 3.

Brief description of the method used:
1. Some simple data preprocessing (e.g. change n_classes=2 to n_classes=1 and change n_jobs=-1 to n_jobs=16)

2. Feature enginnering to get a feature called "e_factor":

all_data['e_factor'] = (np.ceil(all_data['n_classes'] / all_data['n_jobs'])) * all_data['max_iter'] * all_data['n_features'] * all_data['n_samples']

3. Divide the training and test data into 20 parts, each part corresponds to a combination of the feature "penalty" and 'n_jobs". There are 4 values for penalty and 5 values for n_jobs, so there are in total 4*5=20 parts

4. Train a linear regression model with only the e_factor feature on each of the parts, and do prediction on each of the parts

5. Merge the predictions in the parts. Note that the parts do not overlap, so merging is just simply concatenating the prediction results into one dataframe and then sort it by id.
