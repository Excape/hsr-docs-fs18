# Deep Feedforward Network

## Basics
- In supervised learning, we know the desired output for a given input
- \(\theta\) represents the parameters of the network, a.k.a the weights and biases
- the middle layers are "hidden layers", because we don't know what good values for them are

## XOR Example
- Answer with linear regression is always 1/2, since it has the LSE

## Gradient-Based Learning
- Initialize weights with small random values
    - Otherwise the output is just 0
    - And to break symmetry between neuros, otherwise they would do similar / same things
- Sigmoid used to be popular, but now ReLU is mostly used (`max(0, z)`)
- Good Architecture is not well-defined
    - How many units, layers, etc.?
    - approach: Use someone else's architecture that already works
    - There are rarely completely new problems

## Back Propagation
- Basically calculates the gradient of the cost functions with respect to the weights and biases
- The gradient is calculated _analytically_, not computationally

### Chain Rule
- FF-Network is like a nested function (with each layer)
- backprop is a dynamic algorithm that calculates the chain rule efficiently
- Jacobian-matrix is diagonal if the function is element-wise
    - ReLU and sigmoid are
