# Machine Learning Basics

- "Generalization": Perform well on data it hasn't seen before
- Linear regression measures the error on the training data, not on the test data
    - Only works if training and test data sets have the same distribution
- The "i.i.d. assumptions"
    - Assume that all examples are independent, and training and test set are identically distributed
    - Use distribution of one sample to generate all test and training samples
    - Optimizing for the training data will therefore also optimize for the test data
- Overfitting: Reach a low training error, but the test error is much larger
    - i.e. no generalization, only "memorized" the training data
    - Giving it too much capacity
- Capacity: Flexibility the system has
    - e.g. for linear regression, increase capacity by allowing higher-order polynomials
- Check for systems: If you can't get your system to overfit (by giving it lots of capacity), it doesn't learn properly

- Even with a perfect model, with a reasonably complex system, there's always an error
    - e.g. digital communication: noise makes it impossible to always decide correctly (between 1 and 0)

- "No Free Lunch" theorem: No machine learning algorithm is universally better than any other
    - But assumes data of all possible distributions
    - In ML, we always concentrate on a limited range of data
    - i.e. there's no "general" good ML algorithm

## Regularization
- \(\lambda w^T w\): All elements squared and summed up
    - i.e. small weights are preferable for low cost
- If we pick \(\lambda\) to be large, weights will almost be zero to achieve minimal cost (underfitting)
- Also called a "penalty term"
- Goal ist to minimize the generalization error but not the training error

## Hyperparameters
- \(\lambda\) is a "hyperparameter"
    - parameters that are not learned by the scheme
    - to train these parameters, we use part of the training data as "validation data" to "tune" the algorithm
- After we've used a test set once, it is "tainted" and should theoretically not be used anymore
    - hard in practice to have enough data

## Estimators, Bias, Variance
- Estimator: "Schätzer"
- Bias: Difference between Error of "guess" and real data
- Estimator is unbiased: It is on average right ("Erwartungstreu")
- Variance of Estimator
    - "SE": Standard Error, a.k.a \(\sqrt{var(x)}\)
- The variance of a sample is \(\frac{\sigma}{\sqrt m}\)
    - i.e. lots of data reduces the variance of the estimator
