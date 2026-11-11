# Features and labels

## Summary

A *feature* is a measurable input property that a model uses to make a prediction, such as age, income or location. A *label* is the correct answer for an example, such as "loan approved" or "loan rejected", which a supervised model learns to predict. The quality of the features and of the labels limits the quality of any model more than the choice of algorithm does.

## Prerequisites

- [Machine learning](machine-learning.md) for the learning setting.

## Core concepts

### Definitions

- **Feature:** the input data of a model, one measurable property of an example. For predicting creditworthiness, the features might be age, income and location. A *feature vector* `x = (x1, x2, ..., xn)` collects the n features of one example.
- **Label:** the correct answer or outcome for an example, also called the *target*. For creditworthiness, the label is the loan decision: yes or no. Labels exist only in supervised learning.
- **Example (instance, sample):** one row of the dataset, a feature vector and, in supervised learning, its label.

The table shows a small labeled dataset. Each row is an example, the first three columns are features and the last one is the label.

| Age | Income (k$ per year) | Location | Loan approved (label) |
| --- | --- | --- | --- |
| 34 | 72 | urban | yes |
| 22 | 18 | rural | no |
| 51 | 95 | suburban | yes |
| 45 | 30 | urban | no |

### Kinds of features

| Kind | Example | Usual encoding |
| --- | --- | --- |
| Numeric, continuous | income, temperature | Used directly, often standardized to mean 0 and standard deviation 1. |
| Numeric, count | number of past loans | Used directly, sometimes log transformed. |
| Categorical, nominal | location: urban, suburban, rural | One-hot encoding: one binary column per category. |
| Categorical, ordinal | education level | Integer codes that keep the order. |
| Text, image, audio | an email body, a photo | Learned representations (embeddings) from a neural network. |

*Feature engineering* is the design of features from raw data, such as "days since last purchase" from a list of purchase dates. Deep learning reduced manual feature engineering for unstructured data by learning features from raw inputs, but it remains central for tabular data.

### Kinds of labels

- A **categorical label** makes the task *classification*: spam or not spam, one of 1,000 object classes.
- A **numeric label** makes the task *regression*: a price, a temperature, a delivery time.
- **Structured labels** are sequences or sets: a translation, a list of bounding boxes.

### Where labels come from

Labels are expensive. They come from human annotators, from business processes that record outcomes (a chargeback confirms fraud), or from the data itself in self-supervised learning (the next word of a sentence is the label for the words before it). Every source has noise: annotators disagree, and business outcomes are recorded late or only for some cases. A label that is only observed for approved loans (whether they were repaid) is a *selection biased* label, because nothing is known about the applicants who were rejected.

## Worked example

Turning the raw records above into a numeric feature matrix:

1. Standardize income: with mean 53.75 and standard deviation of about 31.1 (population formula), 72 becomes about 0.59 and 18 becomes about -1.15.
2. One-hot encode location into three columns: urban `(1, 0, 0)`, suburban `(0, 1, 0)`, rural `(0, 0, 1)`.
3. Encode the label as 1 for yes and 0 for no.

The first example becomes the feature vector `(34, 0.59, 1, 0, 0)` with label `1`. A model never sees the word "urban". It sees the column that has the value 1, which is why the encoding is part of the model and must be applied identically at inference time.

## Trade offs and when to use it

- **More features** can capture more signal, but also add noise, cost and the risk of overfitting (the *curse of dimensionality*: data becomes sparse as dimensions grow).
- **Hand engineered features** encode domain knowledge and are interpretable. **Learned features** scale to raw data but are opaque.
- **Protected attributes** (such as ethnicity or gender) may be illegal to use in some decisions, and other features can act as proxies for them (a postal code can correlate with ethnicity). Removing the attribute does not remove the bias.

## Common mistakes

- **Leaking the label into the features.** A feature that is computed after the outcome, such as "account closed" when predicting churn, makes offline scores meaningless. Fix: build features only from information available at prediction time.
- **Encoding categories as arbitrary integers.** Coding urban, suburban and rural as 1, 2 and 3 tells a linear model that suburban is "between" the others. Fix: one-hot encode nominal categories.
- **Fitting preprocessing on all data.** Computing the mean for standardization on the full dataset leaks test information into training. Fix: fit preprocessing on the training split only and reuse it.
- **Trusting labels blindly.** Fix: measure label noise, for example by having a sample labeled twice and computing the agreement.

## Practice

1. In a model that predicts the delivery time of a parcel, name three features and the label.
2. Why is one-hot encoding preferred over integer codes for a "color" feature?
3. A churn model reaches 99 % accuracy on day one. Which feature problem do you suspect first?

Answers:

1. Features: distance, carrier, day of the week. Label: delivery time in hours.
2. Colors have no order, and integer codes would impose a false order and distance between them.
3. Label leakage: a feature that is recorded only after a customer has churned.

## Further reading

- Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn, Keras and TensorFlow, 3rd edition*. O'Reilly. Chapter 2 walks through feature preparation on a real dataset.
- Kuhn, M. and Johnson, K. (2019). [*Feature Engineering and Selection: A Practical Approach for Predictive Models*](https://www.feat.engineering/). CRC Press. A book-length treatment of feature design, free online.

TODO: check the content of this note against the sources above and record what was verified and when.
