# Brief - Predicting serious injury

It's 2028, and you are on a new assignment with a new client called Insurance Inc.  

The client is very excited to roll out a new health insurance product that offers ultra-low premiums on the simple condition that deductibles are subject to change at any time.

In order for Insurance Inc. to offer the lowest premiums possible, it needs to be able to assess your risk in a traffic situation in real-time so that your deductible can be adjusted. The data needed to do this is easily obtained through a smartphone app, that customers download and install. The app transmits real-time information about the customer, and their surroundings, that are found in your dataset.

This is where you come in. Using the data obtained from the smartphone app you are going to provide a probability of serious injury, for a given person in the moments before a traffic accident occurs.


## The dataset

- Each row in the dataset corresponds to a single person, involved in a car crash, moments before it occurs.
- The data column names and values are supposed to be quite explicit - there is no data dictionary to be supplied.
- The entries in `y_train` are: `1=serious_injury`, and `0=no_serious_injury`
    - This means that a prediction of `1` is absolute certainty that a person suffered a serious (and expensive) injury.

## Exploring the dataset and modeling

You are expected to explore and understand the dataset, and train a predictive model that outputs the *probability of serious injury* of a person involved in a car crash. This is a very difficult prediction task using only the given data, so don't expect your scores to be super high. 

You have already taken a look at many datasets. However, good practices are always good to remember:
* Take a good overview of the dataset before anything else - numerical variables, categorical variables, anomalies, outliers, missing values, etc. Basically, perform an Exploratory Data Analysis (EDA).  
* Quickly create a baseline model - this will be your starting point from where you will improve (or not!).  
* Think beyond what you have in your hands - you cannot fully guarantee that the data your model will see in production has exactly the same characteristics of the training dataset. Plan for failure.  
* Pipelines are life savers. You probably remember them. You generally have one initial notebook so messy with EDA, that you need to create one with the decisions (steps) you would like to have in your pipeline.  
* Don't overcomplicate your final solution - the more complicated it is, the more problems it might have in production.  

## Deploying and testing

Follow the guidelines for [deploying a model on heroku](https://github.com/LDSSA/heroku-model-deploy) and use the
`test-server.py` script to test your deployment. What you'll want to do is start your server and then run 
`python test-server.py`. There are a few options to use with it. Here are a few examples of how to use the script:

```bash
# This assumes that you have a file called X_train.csv in the data directory
# and you have started your server on your localhost because you are developing it
python test-server.py "data/X_train.csv" "http://127.0.0.1:5000/"

# This is the same scenario but with 100 observations instead of the default 10
python test-server.py "final_datasets/X_train.csv" "http://127.0.0.1:5000/" -n 100

# This will use a different random state to select the observations. You will use
# this after you have one set of observations working well and you want to test
# with a different set.
python test-server.py "final_datasets/X_train.csv" "http://127.0.0.1:5000/" -r 45

# now you've deployed it to heroku!
python test-server.py "final_datasets/X_train.csv" "https://deployed-model.herokuapp.com"
```

### Remember!

You only have 10K rows on the free tier of heroku so after testing, you will want to clear out your database to make
sure that you aren't taking up any precious space for when we actually start the simulator!

### Once more

[Here is a link to the report guidelines](https://docs.google.com/document/d/1cF65SxybGCo761LohjX4Sc51bwliFm4Shwx3mv7UAhM/edit?usp=sharing)



## Batch 2 Capstone Project

This capstone project is meant to *individually* test students for the following:

- Their grasp of the model development workflow
- Their ability to do basic EDA
- Their ability to train and evaluate a predictive model
- Their ability to professionally communicate their findings
    - This will be done via [professional reports](https://docs.google.com/document/d/1cF65SxybGCo761LohjX4Sc51bwliFm4Shwx3mv7UAhM/edit?usp=sharing) that are more important the the trained model itself
- Their ability to understand the paradigm of providing predictions on unseen data
    - Robustness to unseen data that could be quite dirty
    - Providing predictions over time rather than kaggle-style all at once
- Their ability to make decision on which technical tools to use considering the problem in hands.

This is quite different from the previous specializations because they will not be able to submit as many times as they want nor work in teams where they can mask an understanding of the material by brute-forcing search or blending into a crowd. 

This specialization will be the primary way in which we may certify them as entry level data scientists so it is very important that this capstone is both difficult and fair.

Another thing to note is that the EDA and model development portion of this project is not the primary focus of this specialization. *They should already know how to do this*. It should NOT take a long time to do EDA and train a model that performs quite well.

## Components

- A single BLU describing how to deploy a model to heroku while saving observations to a database
- 1 binary classification dataset that is split into 3 parts (see image below for details)
- An initial report that they must submit describing their EDA on the dataset and the model that they will deploy
- A simulator that feeds the test set and some true outcomes to the students deployed models over the course of a week or two.
- A final report describing the test set and any updates to the model that they deployed

### The BLU

This can probably be the same [learning material as last year](https://github.com/LDSSA/heroku-model-deploy) with a few minor updates. An update for windows users or a requirement to use windows subsystems for linux is almost certainly required.

### The dataset

This should be a binary classification dataset that we can logically split into the following parts:

<img src="https://docs.google.com/drawings/d/e/2PACX-1vR9XZmccYWwLYMl6LVUIrL2BW3HRU-KXbEyXx5ui2cU3n9iAFltBe8OJa-0WZ3Xaepvf6fcmKLkWe7k/pub?w=1440&amp;h=1080">

With the following additional requirements

- `X_test_1` and `X_test_2` must contain both numerical and categorical values that `X_train` did not have
- Model performance on `X_test_2` must be demonstrably better regardless of the model if re-trained on `X_test_1` and `y_test_1`
- There must be noticeable shifts in the populations or distributions of 2 features that can be detected using statistical tests.

### The simulation

#### X_test_1 and y_test_1

For `X_test_1`, the flow will happen in this order for an example observation:

1. Observation #1 arrives with all features needed to provide a prediction arrives at the student server
1. The student server will store the observation in your database
1. The student server must return a predction between 0 and 1 for observation #1
1. Some time later, the true outcome for observation #1 will arrive at the student server
    - This true outcome taken from `y_test_1` for observation #1
1. The student server will store the true outcome for observation #1 in the database

#### X_test_2

For `X_test_2`, the flow will happen in this order for an example observation:

1. Observation #5000 arrives with all features needed to provide a prediction
1. The student server will store the observation in your database
1. The student server must return a predction between 0 and 1 for observation #5000

#### Upshots

Notice that for `X_test_1` and `X_test_2` you are essentially expanding the training set. If the training set is expanded, it means that it is possible to train and deploy another model that has had the benefit of seeing more data. It will require some judgement about when and how often to do this though because it involves messing with production systems and the more one does this, the more chances there are for something to go wrong!

Also, it's quite important to make sure that the students are recording every piece of data that comes into the server. This is VERY important for the second report because they will need to analyze what kind of data showed up after they deployed the initial model and wrote the report for that.

### The reports

Both reports should be professional quality. This means that in order to receive a passing grade on it, we should feel comfortable submitting it to our boss or to a client. Developing the guidelines for these reports that gives clear guidance but doesn't just give a cookie-cutter recipe is a non-trivial task that will require quite a bit of judgement. 

### Evaluation

God willing, by the time we finish, we will have 30-40 more students that have submitted all material that must be graded quickly and with quality. By quickly I mean that it cannot take too much time per report because of limited instructor hours. By quality I mean it must be harsh but fair since we cannot certify anyone that does not deserve it while at the same time making sure that there are no surprises for the students.

In order to do this [have clear guidelines](https://docs.google.com/spreadsheets/d/1K8X7xNIi7CKIjtO2bEqWKxMhrRLKBYAHDJGwrg2cwjo/edit?usp=sharing) for the reports that are clearly communicated to both the students and the instructors. We must also derive a simple and efficient grading rubric from these guidelines that allow instructors to quickly evaluate all of the material.

## Schedule

Taken from [this spreadsheet](https://docs.google.com/spreadsheets/d/1XPbv9QSuy0nsCJUU6aKg7kTma_dRmQkZ1B1N-igvLZE/edit?usp=sharing), here is a breakdown of what components of the capstone will be covered on which days:

<img src="https://i.imgur.com/knhse0S.png">

With a work breakdown estimate of the following

<img src="https://i.imgur.com/gcMc51Y.png">

## Distribution of instructor work

1. Sam - Selection and preparation of dataset
1. Manu - Review and prep of BLU
1. Hugo L. and Pedro F. - Development of reporting guidelines and evaluation criteria
1. ? - Deployment of simulator
1. ? - Collection of reports from students
    - This one is mostly operational and does not require any technical knowledge. It will be maintaining a google drive folder and keeping tabs on who submitted what. 
1. ? - Calculating the `roc_auc_score` of each of the student models
1. As many of us as possible - Grading the reports

