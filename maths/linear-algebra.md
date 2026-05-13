# Math: Linear algebra

# Table of contents
1. [Math: Linear algebra](#math:-linear-algebra)
   1. [Linear algebra](#linear-algebra)
      1. [Basic operations](#basic-operations)
      2. [2D vector rotation](#2d-vector-rotation)
   2. [Vector identities](#vector-identities)

## Linear algebra

### Basic operations

 * Dot product:

$$
\mathbf{a} \cdot \mathbf{b} = \sum_{i=1}^{n} a_i b_i \implies a_1b_1 + a_2b_2 + \dots + a_nb_n
$$
 * Cross product:

$$
\mathbf{a} = \begin{bmatrix}a_x \\ a_y \\ a_z \end{bmatrix} \\
\mathbf{b} = \begin{bmatrix}b_x \\ b_y \\ b_z \end{bmatrix}
$$

$$
\mathbf{a} \times \mathbf{b} = \begin{bmatrix}
    a_yb_z - a_zb_y \\
    a_zb_x - a_xb_z \\
    a_xb_y - a_yb_x \\
\end{bmatrix}
$$

### 2D vector rotation

$$
\begin{aligned}
x' &= x \cos{\beta} - y \sin{\beta} \\
y' &= x \sin{\beta} + y \cos{\beta}
\end{aligned}
$$

Demonstration:

>! ![Construction](vec2-rotation.png)
>! Given the image above, we know the following information:
>! $ x = r \cos{(\alpha)} $
>! $ y = r \sin{(\alpha)} $
>

## Vector identities

 * $A + B = B + A$  - Commutativity of addition
 * $A \cdot B = B \cdot A$ - Commutativity of scalar product
 * $A \times B = -(A \times B)$ - Anticommutativity of cross product
 * $c(A + B) = cA + cB$ - Distributivity of multiplication by a scalar over addition
 * $(A + B) \cdot C = A \cdot C + B \cdot C$ - Distributivity of scalar product over addition
 * $(A + B) \times C = A \times C + B \times C$ - Distributivity of vector product over addition
 * Scalar triple product:

$$
A \cdot (B \times C) = B \cdot (C \times A) = C \cdot (A \times B) =
\begin{vmatrix}A B C\end{vmatrix} =
\begin{vmatrix}A_x B_x C_x \\ A_y B_y C_y \\ A_z B_z C_z\end{vmatrix}
$$


