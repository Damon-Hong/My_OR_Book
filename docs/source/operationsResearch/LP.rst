==================
Linear Programming
================== 

.. contents::

--------
LP Model 
--------

----------------
Solution Methods
---------------- 

 

The Simplex Method
==================

The matrix way for the simplex method
-------------------------------------

The easiest way of doing this is to use the **matrix representation** for the simplex method.

Consider a standard form linear program:

.. math::

    \max_{x} c^{T}x

subject to:

.. math::

    Ax=b

    x \geq 0

By dividing :math:`x` to :math:`x_{B}` and :math:`x_{N}`, the basic and non-basic variables,
we have:

.. math::

    \max_{x_{B},x_{N}} c_{B}^{T}x_{B} + c_{N}^{T}x_{N}

subject to:

.. math::

    & A_{B}x_{B} + A_{N}x_{N} = b

    & x_{B},x_{N} \geq 0

where :math:`c^{T} = [c_{B}^{T},c_{N}^{T}]`, :math:`A=[A_{B}, A_{N}]`.

After rearrange the optimization problem, we obtain:

.. math::

    \max_{x_{N}} c_{B}^{T}A_{B}^{-1}b - (c_{B}^{T}A_{B}^{-1}A_{N} - c_{N}^{T})x_{N}

subject to:

.. math::

    & I x_{B}+A_{B}^{-1}A_{N}x_{N} = A_{B}^{-1}b

    & x_{B}, x_{N} \geq 0

Let's ignore the sign constraints and let :math:`z` be the objective value. We then have:

.. math::

    & z \quad + (c_{B}^{T}A_{B}^{-1}A_{N} - c_{N}^{T})x_{N} = c_{B}^{T}A_{B}^{-1}b

    & \quad I x_{B}+ \quad A_{B}^{-1}A_{N}x_{N} = A_{B}^{-1}b


Therefore, given **any valid choice of B and N** (the index sets of basic and non-basic variables),
we may use the following table to calculate the simplex tableau:

.. table::

    =========  =============================================  =============================
    0          :math:`c_{B}^{T}A_{B}^{-1}A_{N} - c_{N}^{T}`    :math:`c_{B}^{T}A_{B}^{-1}b`
    =========  =============================================  =============================
    :math:`I`  :math:`A_{B}^{-1}A_{N}`                         :math:`A_{B}^{-1}b`
    basic      non-basic                                         RHS
    =========  =============================================  =============================


**References:**

`Operations Research Theory, Ling-Chiech Kung, National Taiwan University <https://www.coursera.org/learn/operations-research-theory/lecture/c2huQ/1-3-the-simplex-method-in-metrics>`_ 



--------------
Duality Theory
--------------

Primal and Dual
===============

- If primal problem is a maximization problem, then the dual problem aims to minimize the 
  **upper bound** of primal objective function value. 

- If primal problem is a minimization problem, then the dual problem aims to maximize the 
  **lower bound** of primal objective function value.  

**The general rule:**

In general, if the primal LP is: 

.. math:: 

    \underset{x_1,x_2,x_3}{\text{Maximize }} c_1x_1 + c_2x_2 + c_3x_3 

subject to: 

.. math:: 

    A_{1,1}x_1 + A_{1,2}x_2 + A_{1,3}x_3 \geq b_1 

    A_{2,1}x_1 + A_{2,2}x_2 + A_{2,3}x_3 \leq b_2

    A_{3,1}x_1 + A_{3,2}x_2 + A_{3,3}x_3 = b_3 

    x_1 \geq 0, x_2 \leq 0, x_3 \text{ is urs} 


Its dual LP is: 

.. math:: 

    \underset{y_1,y_2,y_3}{\text{Minimize }} b_1y_1 + b_2y_2 + b_3y_3

subject to: 

.. math:: 

    A_{1,1}y_1 + A_{2,1}y_2 + A_{3,1}y_3 \geq c_1 

    A_{1,2}y_1 + A_{2,2}y_2 + A_{3,2}y_3 \leq c_2

    A_{1,3}y_1 + A_{2,3}y_2 + A_{3,3}y_3 = c_3 

    y_1 \leq 0, y_2 \geq 0, y_3 \text{ is urs} 

.. note:: 

    The constraint coefficient matrix is **transposed**.  


**The dual problem can be derived as follows:**

1. The primal is a maximization problem, so we need a upper bound of the objective function. Therefore, :math:`\leq` is needed.
Because of it, we then know dual variable :math:`y_1` is nonpositive, :math:`y_2` is nonnegative, and :math:`y_3` is unrestricted in sign (urs). Then, the system of constraints 
reformulated as: 

.. math:: 

    A_{1,1}x_1y_1 + A_{1,2}x_2y_1 + A_{1,3}x_3y_1 \leq b_1 y_1

    A_{2,1}x_1y_2 + A_{2,2}x_2y_2 + A_{2,3}x_3y_2 \leq b_2y_2

    A_{3,1}x_1y_3 + A_{3,2}x_2y_3 + A_{3,3}x_3y_3 = b_3y_3 

2. If we combine these constraints together, then we get:
   
.. math::

    \begin{gathered}
    (A_{1,1}y_1+A_{2,1}y_2+A_{3,1}y_3)x_1 + (A_{1,2}y_1+A_{2,2}y_2+A_{3,2}y_3)x_2 + \\
    (A_{1,3}y_1+A_{2,3}y_2+A_{3,3}y_3)x_3 \leq b_1y_1 + b_2y_2 + b_3y_3
    \end{gathered}

Therefore, :math:`b_1y_1 + b_2y_2 + b_3y_3` can be treated as a upper bound. 

3. Then we can derive a sequence of inequalities, which is: 

.. math:: 

    \begin{gathered}
    c_1x_1+c_2x_2+c_3x_3 \leq (A_{1,1}y_1+A_{2,1}y_2+A_{3,1}y_3)x_1 + (A_{1,2}y_1+A_{2,2}y_2+A_{3,2}y_3)x_2 + \\
    (A_{1,3}y_1+A_{2,3}y_2+A_{3,3}y_3)x_3 \leq b_1y_1 + b_2y_2 + b_3y_3
    \end{gathered}

To make sure these (especially first) inequalities are always satisfied for all :math:`x_1 \geq 0, x_2 \leq 0`, and :math:`x_3` is urs, 
we can get a system of inequalities: 

.. math:: 

    c_1 \leq A_{1,1}y_1+A_{2,1}y_2+A_{3,1}y_3 

    c_2 \geq A_{1,2}y_1+A_{2,2}y_2+A_{3,2}y_3

    c_3 = A_{1,3}y_1+A_{2,3}y_2+A_{3,3}y_3


4. Finally, we want to minimize the upper bound value :math:`b_1y_1 + b_2y_2 + b_3y_3`, so that it can be closer to our original objective value. 
Therefore, we can formulate an optimization problem as follows: 

.. math:: 

    \underset{y_1,y_2,y_3}{\text{Minimize }} b_1y_1 + b_2y_2 + b_3y_3

subject to: 

.. math:: 

    A_{1,1}y_1 + A_{2,1}y_2 + A_{3,1}y_3 \geq c_1 

    A_{1,2}y_1 + A_{2,2}y_2 + A_{3,2}y_3 \leq c_2

    A_{1,3}y_1 + A_{2,3}y_2 + A_{3,3}y_3 = c_3 

    y_1 \leq 0, y_2 \geq 0, y_3 \text{ is urs} 

This is how we derive a dual problem from a primal maximization problem.


**Standard form primal problem:**

In general, if the primal LP is in the standard form as: 

.. math:: 

    \underset{x_1,x_2,x_3}{\text{Maximize }} c_1x_1 + c_2x_2 + c_3x_3 

subject to: 

.. math:: 

    A_{1,1}x_1 + A_{1,2}x_2 + A_{1,3}x_3 = b_1 

    A_{2,1}x_1 + A_{2,2}x_2 + A_{2,3}x_3 = b_2

    A_{3,1}x_1 + A_{3,2}x_2 + A_{3,3}x_3 = b_3 

    x_1 \geq 0, x_2 \geq 0, x_3 \geq 0 


The matrix representation is: 

.. math:: 

    \underset{x}{\text{Maximize }} c^{T}x

subject to: 

.. math:: 

    Ax = b

    x \geq 0



Then, its dual LP is: 

.. math:: 

    \underset{y_1,y_2,y_3}{\text{Minimize }} b_1y_1 + b_2y_2 + b_3y_3

subject to: 

.. math:: 

    A_{1,1}y_1 + A_{2,1}y_2 + A_{3,1}y_3 = c_1 

    A_{1,2}y_1 + A_{2,2}y_2 + A_{3,2}y_3 = c_2

    A_{1,3}y_1 + A_{2,3}y_2 + A_{3,3}y_3 = c_3 

    y_1, y_2, y_3 \text{ are urs} 

The matrix representation is: 

.. math:: 

    \underset{y}{\text{Minimize }} b^{T}y

subject to: 

.. math:: 

    A^{T}y = c

    y \text{ is urs}



.. note::

    **Proposition 1:** For any primal LP, there is a unique dual, whose dual is the primal.


Duality Theorems
================







------------
Applications
------------


