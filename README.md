# Car-Insurance-Fraud-Detection
Among all kinds of fraud, insurance fraud is one the most damaging one that a company can face. According to National Insurance Crime Bureau (NICB) and Federal Bureau of Investigation (FBI), fraud is the second most costly white-collar crime in America behind tax evasion. The total cost of insurance fraud (non-health insurance) estimated to be more than $40 billion (₹ 277902 crores) per year. In this project, we have chosen to work on Car Insurance Fraud, as already many credit card fraud detection solutions have sprung up, but very less of those are related to car insurance.
Fraud rings or groups may fake traffic deaths or stage collisions to make false insurance or exaggerated claims and collect insurance money. The ring may involve insurance claims adjusters and other people who create phony police reports to process claims. Two types of car insurance fraud exist: -

    Hard auto-insurance fraud - It includes activities such as staging automobile collisions, filing claims when the claimant was not actually involved in the accident, submitting claims for medical treatments that were not received, or inventing injuries. Can also occur when claimants falsely report their vehicle as stolen.
    Soft auto-insurance fraud - It includes filing more than one claim for a single injury, filing claims for injuries not related to an automobile accident, misreporting wage losses due to injuries, and reporting higher costs for car repairs than those that were actually paid.

Soft frauds account for the majority of auto-insurance frauds.

Faking a "whiplash" injury can get a fraud easy compensation money amounting to £2500 (₹ 22,06,360).

Exploratory Data Analysis: -

First, we will understand the dataset chosen, on which we will perform our fraud detection process. The dataset we chose is "claims Original.csv". We load it onto a Jupyter notebook (which runs on Python).

The first 10 observations, as loaded onto a Jupyter Notebook.

    There are 15420 observations. There are 33 features. Out of which 32 are independent features, and 1 ('FraudFound_P') is a dependent feature.

Here are what the features mean: -

    Month - Month in which accident took place
    Week of Month - Accident week of month
    Day of Week - Accident day of week
    Make - Manufacturer of the car (19 companies)
    Accident Area - Rural or Urban
    Day of Week Claimed - Claim day of week
    Month Claimed - Claim month
    Week of Month Claimed - Claim week of month
    Sex - Male or Female
    Marital Status - Single, Married, Widow and Divorced
    Age - Age of policy holder
    Fault - Policy Holder or Third Party
    Policy Type - Type of policy (1 to 9)
    Vehicle Category - Sedan, Sport or Utility
    Vehicle Price - Price of the vehicle with 6 categories
    Policy Number- Index of the Policy
    Rep. Number - ID of the person who process the claim (16 IDs)
    Deductible - Amount to be deducted before claim disbursement
    Driver Rating - Driving Experience with 4 categories
    Days Policy Accident - Days left in policy when accident happened
    Days Policy Claim - Days left in policy when claim was filed
    Past number of Claims - Past number of claims
    Age of Vehicle - Vehicle's age with 8 categories
    Age of Policy Holder - Policy holder's age with 9 categories
    Police Report Filed – Yes (1) or no (0)
    Witness Present – Yes (1) or no (0)
    Agent Type - Internal or External
    Number of Supplements - Number of supplements
    Address Change Claim – No. of times change of address requested
    Number of Cars - Number of cars owned by the person claiming insurance
    Year - 1994, 1995 and 1996
    Base Policy - All perils, Collision or Liability
    FraudFound_P - Fraud found (yes '1' or no '0')

There are no missing elements present in our dataset.

Feature Selection and Engineering: -

    First, we draw a correlation coefficient heatmap of all the features.
    We can see that none of the numerical features are that closely related to one another.
    The only feature we see is Year and PolicyNumber, which is insignificant as we will remove both of those features as Year simple states one of the three years – 1994, 1995 and 1996; whereas PolicyNumber just serves as an index column.
    Next, we run statistical tests to determine which features are important and which of them should be removed from our dataset, before feeding it into our model.
    For categorical features vs categorical features, we use Chi-squared test.
    For numerical features vs categorical features, we use One-Way Anova test.
    All the p-values for the respective features are given below.

    In Chi-squared , we remove all the features which have a p-value greater than 0.05.

    Another way of doing feature selection is by running a Random Forest model on top of our dataset. Doing this gives us a list of importances with respect to their features.

    Here are some of the features with their importances: -

    After putting a threshold (in this case, 0.02), we only keep these columns: -

    We create a new csv file to store only the important features, to be carried onto the algorithm execution.

    Before fitting a model, we will first encode these categorical features, since algorithms cannot make sense of non-numerical values during fitting.

    Here, we will use One-Hot encoding using Pandas. We use a function get_dummies(), which returns all unique values of features, in binary form; which can be read by the algorithm.

Algorithms/Models: -

    The first algorithm we will use is Self-Organising Maps (SOMs). It is an unsupervised learning algorithm which represents multi-dimensional input data in a 2-dimensional map.

    Outliers can be detected through the SOM as they will have greater distance between itself and the winning neuron. Here is the SOM formed during the first iteration.
    Here, the mappings with the co-ordinate (3,4), contains the chunk of the outliers in the dataset.
    The SOM predicted 12% of the observations to be frauds/outliers. This, however, is not constant, as the SOM changes its form every time we run the algorithm.
    We can tweak it to make the output outliers result in a less than 5% amount.
    After reverse mapping, we compare it with the actual frauds in the dataset, and get the following confusion matrix and scores: -

    The second algorithm we will use is DBSCAN , which stands for Density-based spatial clustering of applications with noise.

    Fitting the dataset into this model labels all outliers with -1. We take note of those indexes and compare them with the actual frauds. Here is the confusion matrix and the scores: -

    The third algorithm we will use is Isolation Forest. It is specifically designed to only detect outliers.

    It works on the principle that outliers can be easily isolated in less steps using randomized trees, as compared to inliers.
    After getting the indexes of observations which are frauds, we compare them with the actual frauds
    Following is the confusion matrix and the various scores: -

    The fourth algorithm we will be using is Local Outlier Factor. It finds anomalous data points by measuring the local deviation of a given data point with respect to its neighbours.

    It shares a similar concept to DBSCAN. It also labels its outliers as -1.
    We compare the outliers detected with the actual outliers present in the dataset.
    Following is the confusion matrix and the various scores: -

Scores: -

    SOM has the highest F1 score, with LOF being a close second.

    LOF has the highest recall score.

    SOM has the highest precision score.
