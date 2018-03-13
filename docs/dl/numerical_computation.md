# Numerical Computation

- Most of this is handled by frameworks

## Overflow and Underflow
- Underflow: So small that they're basically 0
    - \(\log x\) when x is zero: \(-\infty\)
- softmax takes a vector, the sum will be 1, and every number will be positive -> probability distribution
- sigmoid is softmax with 2 dimensions, where \(x = x_1 - x_2\)
- if the numbers in the vector are very large or very small (negative), there's an underflow / overflow problem
    - solution: subtract the max of the vector from every entry, and use the softmax on that
- often, we optimize for the log of probabilities -> \(\log \text{softmax}\)
    - useful, because log and exp cancel each other out
    - called "logsoftmax"

## Poor Conditioning
- Problem: Small change in input produces very large change in output

## Gradient-Based Optimization
- \(arg min f(x)\): the argument `x` that produces the minimum of \(f(x)\)
    - Called the "minimizer"
    - `f(arg min f(x)) = min`
- Idea: if the slope is negative, walk forwards, if it's positive, walk backwards: "gradient descent"
- Gives us a minimum
- Gradient = derivative in multiple dimensions
- If the gradient is 0, we're either at a minimum, a maximum or a _saddle point_
    - saddle points are common in higher dimensions
- the goal is finding a global minimum, but we don't know if we've found it with gradient descent, except if the function is "convex", e.g. a quadratic function with one minimum
- There's a derivative for every dimension
- The gradient is a vector with all the partial derivatives for every dimension

### Gradient descent for vectors
- Gradient is vector that points to the steepest slope
- We want to go in the reverse opposite of that vector to find the minimum
- analogy: Go down a mountain by taking the steepest step each time, you'd find the valley
- minimizing: cos should be -1 -> 180°
    - The opposite direction of \(u\) (the steepest direction)

## Jacobian and Hessian Matrices
- \(f(x)\) is now mapping from n-dimensional to m-dimensional vector
    - e.g. common between two layers in a neural network
- The Jacobian matrix \(J\) captures the derivate of every input entry w.r.t. to every output entry, so it's a n by m matrix
    - derivates say how the change of one value affects the other
- Derivatives of derivatives can be in different directions, so there are a lot of combinations
- Second derivative = curvature
    - The curvature of a plane is 0
    - This makes gradient descent "easy", just follow the derivative
- All the combinations of curvature are captured in the Hessian matrix \(H\)
- The optimal \(\epsilon\) could be calculated with the Hessian matrix, but that's too hard to calculate (and store), so not practical
- We can determine if we're at a minimum, maximum with the second derivative
    - If the derivate is 0, it could be a saddle point, but the test is inconclusive
    - The same concept works in higher dimensions with eigenvalues
    - If not all eigenvalues are either positive or negative, we're at a saddle point
    - Almost a chance of 1
- The condition number tells us how much the space is "warped"
    - if the condition number is high, gradient descent is harder (slower)
