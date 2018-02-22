# Linear Algebra
- Indices in Matrix: First index is row (*i*: row, *j*: column)
- A 3D (or higher-dimensional) array is a **tensor**
- In DL, we add scalars to matrices (not in linearl algebra): \(D_{i,j} = a \cdot B_{i,j} + c\)

## Matrix Multiplication
- Dimensions must be (q,p) = (q,k)(k,p)
    - `k`-sized vectors are multiplied together
- *Hadamard Prodcut*: Element-wise product, dimensions must be the same
- Matrix multiplication is associative and distributive, but **not commutative**
    - \(AB \neq BA\)

## Inverse Matrix
- Matrix which when multiplied with A will result in the Identity Matrix
- Used for solving equations
    - \(Ax = b \Rightarrow x = A^{-1}b\)
    - Numerically not ideal
- Equation system could also have no or infinite solutions
    - No "multipled solutions", because an infinite number of solutions can be built with two of them
- For \(A^{-1}\) to exist, we need \(m = n\) and all columns must be *linearly independent*
    - meaning no vector can be created by scaling two others

## Norms
- Measure to measure the length of a vector
- Most "famous": Euclidian norm ("pythagoras")
- The *squared* euclidian norm can be calculates as \(x^Tx\)
- \(L^1\)-norm: Add up every absolute value of the vector
    - Better for values closer to zero
- \(L^0\)-norm: Count the number of non-zero entries
    - **not a mathematical norm**, properties of a norm don't hold
- \(L^\infty\) is just the max
- Frobenius norm: Square every entry of a Matrix and square the result

## Special Matrices
- Diagonal Matrix: Everything 0 except diagonal
- `diag(v)`: `v` is the vector formed by the diagonal
- `diag(v)^-1` = `diag([1/v1, ... 1/vn])
- Symmetric matrix: \(A = A^{-1}\)
- orthogonal matrix: rows and columns are mutually orthonormal
    - two vectors orthonomal => dot product = 0

## Eigendecomposition
- \(Av = \lambda v\)
- We need to find Eigenvector \(v\) and Eigenvalue (\lambda\)
- We usually scale the Eigenvector to have length 1 (euclidian norm)
- \(A = V \text{diag}(\lambda)V^{-1}\)