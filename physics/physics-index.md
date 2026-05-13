# Physics

# Table of contents
1. [Physics](#physics)
   1. [Constraints resolution](#constraints-resolution)
      1. [2D example](#2d-example)
   2. [Generalized velocity constraint](#generalized-velocity-constraint)
      1. [Example: Finding the Jacobian for a position constraint](#example:-finding-the-jacobian-for-a-position-constraint)
      2. [Example: penetration constraint](#example:-penetration-constraint)

   2. [Generalized velocity constraint](#generalized-velocity-constraint)
   3. [Example: Finding the Jacobian for a position constraint](#example:-finding-the-jacobian-for-a-position-constraint)

## Constraints resolution

Constraints can be expressed as equality or inequality equations.

They can also be applied to positions, velocities or accelerations.

It is common for physics engines to express constraints as equation in velocity
space, which allows us to smoothly update the position (compared to fixing a
constraint in position space which causes an instantaneous change).

The constraint $C$ rapresents our error.

If we define a vector $P$ that contains position and orientation of the bodies,
for a generic constraint $C$ we can find the time derivative by *chain rule*.

$$
\dot{C} = \frac{\partial{C}}{\partial{P}} \: \frac{\partial{P}}{\partial{t}}
$$

We call the first part the *Jacobian matrix* and the second part is a vector of our velocities:

$$
\frac{\partial{C}}{\partial{P}} = J
$$


$$
\frac{\partial{P}}{\partial{t}} = v
$$

Which gives us the equation:
$$
\dot{C} = JV + b
$$

Where $b$ is a bias factor to smoothly fix the constraint.

### 2D example

In 2D, our $v$ is a vector:
$$
\begin{bmatrix}
Va_x \\
Va_y \\
Wa \\
Vb_x \\
Vb_y \\
Wb
\end{bmatrix}
$$

Where:
 - Vax = Linear velocity of A on the x axis;
 - Vay = Linear velocity of A on the y axis;
 - Wa  = Angular velocity of A
 - Vbx = Linear velocity of B on the x axis;
 - Vby = Linear velocity of B on the y axis;
 - Wb  = Angular velocity of B

The Jacobian is a *row* matrix of coefficient to apply to our velocities:
$$
\begin{bmatrix}
J_{v_{ax}} \:
J_{v_{ay}} \:
J_{w_a}  \:
J_{v_{bx}} \:
J_{v_{by}} \:
J_{w_b}
\end{bmatrix}
$$


## Generalized velocity constraint

Given the equation:
$$
JV + b = 0
$$

and given two velocity vectors:
 - $V_1$: the initial velocity that violates the constraint;
 - $V_2$: the velocity that resolved the costraints;

The change of velocity that solved the constraint is: $V_2 - V_1$.

Change in velocity multiplied by the mass gives us the *impulse*:

$$M(V_2 - V_1) = j$$

We borrow names from *lagrangian mechanics* and rewrite the previous as:

$$
M(V_2 - V_1) = L\lambda
$$

Where $L$ is the *impulse direction* and $\lambda$ is the *impulse magnitude*.

 - The direction of our impulse is our *Jacobian* transposed: $L = J^T$
 - We need to find the impulse magnitue $\lambda$

$$
\begin{aligned}
V_2 &= V_1 + \frac{J^T\lambda}{M} \\
V_2 &= V_1 + M^{-1} J^T \lambda
\end{aligned}
$$

Plugging $V_2$ back into the generalised velocity constraint $JV+b=0$ we find:

$$
\begin{aligned}
J(V_1 + M^{-1}J^T\lambda) + b &= 0 \\
J V_1 + J M^{-1} J^T \lambda + b &= 0 \\
(J M^{-1} J^T) \lambda &= -(J V_1 + b) \\
\lambda &= \frac{-(J V_1 + b)}{(J M^{-1} J^T)} \\
\end{aligned}
$$

### Example: Finding the Jacobian for a position constraint

![Position constraint](position-constraint.png)

First we define the constraint function in vector form:

$$
\begin{aligned}
C = (P_b - P_a) &\cdot (P_b - P_a) = 0 \\
C = P_a \cdot P_b - P_b \cdot P_a &- P_a \cdot P_b + P_a \cdot P_a = 0
\end{aligned}
$$

We proceed to take the first derivative of the constraints, given the derivative
of a dot product such as:

$$
\frac{d}{dt}(\vec{r} \cdot \vec{s}) = \vec{r} \cdot \frac{d\vec{s}}{dt} + \vec{s} \cdot \frac{d\vec{r}}{dt}
$$

This gives us:

$$
\dot{C} =
    (\frac{dP_a}{dt} \cdot P_b + P_b \cdot \frac{dP_b}{dt}) -
    (\frac{dP_b}{dt} \cdot P_a + P_b \cdot \frac{dP_a}{dt}) -
    (\frac{dP_a}{dt} \cdot P_b + P_a \cdot \frac{dP_b}{dt}) +
    (\frac{dP_a}{dt} \cdot P_a + P_a \cdot \frac{dP_a}{dt})
$$

$$
\dot{C} =
    \frac{dP_b}{dt} \cdot P_b + P_b \cdot \frac{dP_b}{dt} -
    \frac{dP_b}{dt} \cdot P_a - P_b \cdot \frac{dP_a}{dt} -
    \frac{dP_a}{dt} \cdot P_b - P_a \cdot \frac{dP_b}{dt} +
    \frac{dP_a}{dt} \cdot P_a + P_a \cdot \frac{dP_a}{dt}
$$

$$
\dot{C} = 2(P_b - P_a) \cdot \frac{dP_b}{dt} + 2(P_a - P_b) \cdot \frac{dP_a}{dt}
$$
From the previous equation, we know:

$$
\begin{split}
\frac{dP_a}{dt} &= v_a + (w_a \times r_a) \\
\frac{dP_b}{dt} &= v_b + (w_b \times r_b)
\end{split}
$$

Let's substitute it in the previous equation and we obtain:

$$
\dot{C} =
    2(P_b - P_a) \cdot (v_b + (w_b \times \vec{R_b}))
    2(P_a - P_b) \cdot (v_a + (w_a \times \vec{R_a}))
$$

Expanding out all terms we find:

$$
\dot{C} =
2(P_a - P_b) \cdot v_a + 2(P_a - P_b) \cdot \omega_a \times \vec{R_a} +
2(P_b - P_a) \cdot v_b + 2(P_b - P_a) \cdot \omega_b \times \vec{R_b}
$$

We can use the scalar triple product [vector identity](../maths/linear-algebra.md#vector-identities) to isolate the
velocities.

$$
\dot{C} =
2(P_a - P_b) \cdot v_a + \omega_a \cdot \vec{R_a} \times 2(P_a - P_b) +
2(P_b - P_a) \cdot v_b + \omega_b \cdot \vec{R_b} \times 2(P_b - P_a)
$$

Which can be written in matrix form:

$$
\dot{C} =
\begin{bmatrix}
2(P_a - P_b),& 2(\vec{R_a} \times (P_a - P_b)),&
2(P_b - P_a),& 2(\vec{R_b} \times (P_b - P_a))
\end{bmatrix}
*
\begin{bmatrix}
v_a \\ \omega_a \\ v_b \\ \omega_b
\end{bmatrix}
$$

### Example: penetration constraint

![Penetration constraint](penetration-constraint.png)

A penetration constraint can be expressed as the inequality:

$$
\dot{C} = \frac{dP_b}{dt} \cdot \vec{n} - \frac{dP_b}{dt} \cdot \vec{n}
$$


Where $\vec{n}$ is the collision normal and the derivative are the velocities at the collision points $P_a$ and $P_b$.

We know that the relative velocity:

$$
\begin{aligned}
\frac{dP_b}{dt} &= (v_b + (\omega_b \times r_b)) \cdot \vec{n} \\
\frac{dP_a}{dt} &= (v_a + (\omega_a \times r_a)) \cdot \vec{n} \\
\end{aligned}
$$

We can sobstitue and expand the dot product:

$$
\dot{C} =
v_b \cdot \vec{n} +
\vec{n} \cdot (\omega_b \times r_b) -
v_a \cdot \vec{n} -
\vec{n} \cdot (\omega_a \times r_a)
$$

One again, we can isolate the velocities using the [triple scalar vector identity](../maths/linear-algebra.md#vector-identities).


$$
\dot{C} =
v_b \cdot \vec{n} +
\omega_b \cdot (r_b \times \vec{n}) -
v_a \cdot \vec{n} -
\omega_a \cdot (r_a \times \vec{n})
$$

Which can be written in matrix form (where the row matrix is our Jacobian $J$).

$$
\dot{C} =
\begin{bmatrix} -\vec{n},& -r_a \times \vec{n},& \vec{n},& r_b \times \vec{n} \end{bmatrix} *
\begin{bmatrix} v_a \\ \omega_a \\ v_b \\ \omega_b \end{bmatrix}
$$

