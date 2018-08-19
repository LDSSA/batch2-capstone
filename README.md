## Batch 2 Capstone Project

This capstone project is meant to *individually* test students for the following:

- Their grasp of the model development workflow
- Their ability to do basic EDA
- Their ability to train and evaluate a predictive model
- Their ability to professionally communicate their findings
    - This will be done via professional reports that are more important the the trained model itself
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

1. Observation #1 arrives with all features needed to provide a prediction arrives at your server
1. Your server will store the observation in your database
1. Your server must return a predction between 0 and 1 for observation #1
1. Some time later, the true outcome for observation #1 will arrive at your server
    - This true outcome taken from `y_test_1` for observation #1
1. Your server will store the true outcome for observation #1 in the database

#### X_test_2

For `X_test_2`, the flow will happen in this order for an example observation:

1. Observation #5000 arrives with all features needed to provide a prediction
1. Your server will store the observation in your database
1. Your server must return a predction between 0 and 1 for observation #5000

#### Upshots

Notice that for `X_test_1` and `X_test_2` you are essentially expanding your training sets. If you are expanding your training sets, that means that you can train and deploy another model that has had the benefit of seeing more data. You will need to use some judgement about when and how often to do this though because you are messing with production systems and the more you do this, the more chances there is for something to go wrong!

Also, it's quite important to make sure that you're recording every piece of data that comes into the server. This is VERY important for the second report because you'll need to analyze what kind of data showed up after you deployed your initial model and wrote the report for that.

### The reports

Both reports should be professional quality. This means that in order to receive a passing grade on it, we should feel comfortable submitting it to our boss or to a client. Developing the guidelines for these reports that gives clear guidance but doesn't just give a cookie-cutter recipe is a non-trivial task that will require quite a bit of judgement. 

### Evaluation

God willing, by the time we finish, we will have 30-40 more students that have submitted all material that must be graded quickly and with quality. By quickly I mean that it cannot take too much time per report because of limited instructor hours. By quality I mean it must be harsh but fair since we cannot certify anyone that does not deserve it while at the same time making sure that there are no surprises for the students.

In order to do this we must have clear guidelines for the reports that are clearly communicated to both the students and the instructors. We must also derive a simple and efficient grading rubric from these guidelines that allow instructors to quickly evaluate all of the material.

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
