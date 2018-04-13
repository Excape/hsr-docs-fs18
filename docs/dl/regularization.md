# Regularization for Deep Learning

- Goal: Make the algorithm "generalize", e.g. that it will perform well on data it has never been trained with
- We don't want to memorize the training data

## Parameter Regularization
- Take e.g. the L2-Norm
- Multiply it with a hyperparameter \(\alpha\) and add it to the cost function
- Goal: Weights should be small (weight decay)
- with \((1 - \epsilon \alpha)w\), weights want to go towards zero
- Intuition: Small curvatures get pulled to zero, big curvatures don't get affected by the regularization

- L1-Regularization is a "soft threshold" that makes a lot of weights go to zero
    - Fewer values to calculate

## Dataset Augmentation
- Create artificial training data
- e.g. for picture classification: Just turn the image slightly, or zoom in / out, darken / brighten, etc.
- The label stays the same, but we can multiply the training set
- i.e. a transformation that changes the input x, but not the output y
- inject noise to make the dataset more non-deterministic
    - We could also apply noise to the weights. Bayes philosophy that weights are just random variables with DFs, noise is a way to simulate that