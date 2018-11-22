# 2018-fall-msbd5001-individual-proj

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

3. Divide the training and test data into 20 parts, each part corresponds to a combination of the feature "penalty" and 'n_jobs". There are 4 values for penalty and 5 values for n_jobs, to there are in total 4*5=20 parts

4. Train a linear regression model with only the e_factor feature on each of the parts, and do prediction on each of the parts

5. Merge the predictions in the parts. Note that the parts do not overlap, so merging is just simply concatenating the prediction results into one dataframe and then sort it by id.
