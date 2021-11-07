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

    \begin{aligned}  
        & \underset{x_1,x_2,x_3}{\text{Maximize }} c_1x_1 + c_2x_2 + c_3x_3

        & \text{subject to:} 

        & A_{1,1}x_1 + A_{1,2}x_2 + A_{1,3}x_3 = b_1 

        & A_{2,1}x_1 + A_{2,2}x_2 + A_{2,3}x_3 = b_2

        & A_{3,1}x_1 + A_{3,2}x_2 + A_{3,3}x_3 = b_3 

        & x_1 \geq 0, x_2 \geq 0, x_3 \geq 0 
    \end{aligned}
    

The matrix representation is: 

.. math:: 
    :label: 1

    \begin{aligned}
    & \underset{x}{\text{Maximize }} c^{T}x

    & \text{subject to:} 

    & Ax = b

    & x \geq 0
    \end{aligned}



Then, its dual LP is: 

.. math:: 
    
    \begin{aligned}
    & \underset{y_1,y_2,y_3}{\text{Minimize }} b_1y_1 + b_2y_2 + b_3y_3

    & \text{subject to:} 

    & A_{1,1}y_1 + A_{2,1}y_2 + A_{3,1}y_3 \geq c_1 

    & A_{1,2}y_1 + A_{2,2}y_2 + A_{3,2}y_3 \geq c_2

    & A_{1,3}y_1 + A_{2,3}y_2 + A_{3,3}y_3 \geq c_3 

    & y_1, y_2, y_3 \text{ are urs} 
    \end{aligned}

The matrix representation is: 

.. math:: 
    :label: 2

    \begin{aligned}
    & \underset{y}{\text{Minimize }} b^{T}y

    & \text{subject to:} 

    & A^{T}y \geq c

    & y \text{ is urs}
    \end{aligned}


.. note::

    **Proposition 1:** For any primal LP, there is a unique dual, whose dual is the primal.


Duality Theorems
================

Weak Duality
------------

.. note:: 

    **Proposition 2 (Weak Duality):** For the LPs defined in :eq:`1`, if :math:`x` and :math:`y` are primal and dual feasible, then 
    :math:`c^{T}x \leq b^{T}y`. 

This is because dual problem provides an **upper bound** of the primal problem. 

Sufficiency of Optimality
-------------------------

.. note:: 

    **Proposition 3 (Sufficiency Condition for Optimality):** If :math:`\overline{x}` and :math:`\overline{y}` are primal and dual feasible, and :math:`c^{T}\overline{x} = b^{T}\overline{y}`, 
    then :math:`\overline{x}` and :math:`\overline{y}` are primal and dual optimal. 

**Proof:**

For all dual feasible :math:`y`, we have :math:`c^{T}\overline{x} \leq b^{T}y` by weak duality. But we are given that :math:`c^{T}\overline{x} = b^{T}\overline{y}`, so we have 
:math:`b^{T}\overline{y} = b^{T}y` for all dual feasible :math:`y`. This is telling us that :math:`\overline{y}` is dual optimal. For :math:`\overline{x}` is the same. 

.. note::

    Given a primal feasible solution :math:`\overline{x}`, if we can find a dual feasible solution so that there objective values are **identical**, then :math:`\overline{x}` is the optimal solution.


The Dual Optimal Solution 
-------------------------

If we have solved the primal LP, the **dual optimal solution** is there. 

.. note:: 

    **Proposition 4 (Dual Optimal Solution):** For the LPs defined in :eq:`1` , if :math:`\overline{x}` is 
    primal optimal with basis B, then :math:`\overline{y} = c_{B}^{T}A_{B}^{-1}` is dual optimal. 

**Proof:**

If :math:`\overline{x}` is primal optimal, then let's say :math:`\overline{y}` is the coresponding dual optimal, then
:math:`c^{T}\overline{x} = b^{T}\overline{y}`. Because :math:`c^{T}\overline{x} = c^{T}_{B}A_{B}^{-1}b`, then 
:math:`\overline{y} = c_{B}^{T}A_{B}^{-1}`.   

.. note::

    **Proposition 5 (Strong Duality):** For the LPs defined in :eq:`1`, :math:`\overline{x}` and :math:`\overline{y}` are primal 
    and dual optimal if and only if :math:`\overline{x}` and :math:`\overline{y}` are primal and dual feasible and 
    :math:`c^{T}\overline{x} = b^{T}\overline{y}`. 




Complementary Slackness
-----------------------

Consider :math:`v`, the **slack variable** of the dual LP: 

.. math:: 
    :label: 3

        \begin{aligned}
        & \underset{y,v}{\text{minimize }} b^{T}y \\
        & \text{subject to:} \\ 
        & A^{T}y - v = c \\
        & v \geq 0
        \end{aligned}

.. note::

    **Proposition 6 (Complementary Slackness):** For the LPs defined in :eq:`1` and :eq:`3`, :math:`\overline{x}` and :math:`(\overline{y}, \overline{v})`
    are primal and dual optimal if and only if they are feasible and :math:`\overline{v}^{T}\overline{x} = 0`. 


**Proof:**

We have :math:`c^{T}\overline{x} = (A^{T}\overline{y}-\overline{v})^{T}\overline{x} = (\overline{y}^{T}A - \overline{v}^{T})\overline{x}= \overline{y}^{T}A\overline{x} - \overline{v}^{T}\overline{x} = \overline{y}^{T}b - \overline{v}^{T}\overline{x} = b^{T}\overline{y} - \overline{v}^{T}\overline{x}`.
Therefore, :math:`\overline{v}^{T}\overline{x} = 0` if and only if :math:`c^{T}\overline{x} = b^{T}\overline{y}`, that is  :math:`\overline{x}` and :math:`(\overline{y}, \overline{v})`
are primal and dual optimal according to strong duality. 





Shadow Prices
-------------

.. note:: 

    **Definition 1 (Shadow Prices):** For an LP that has an optimal solution, the shadow price of a constraint is the amount of objective value increased when the RHS of 
    that constraint is increased by 1, assuming the current optimal basis remains optimal. 

- NOTE that we assume the current optimal basis (optimal basic variables of primal LP) does not change. 
- If shifting a constraint does not affect the optimal solution, the shadow price must be **zero**. 
- Not all binding constraints has nonzero shadow prices. 

.. note:: 

    **Proposition 8:** Shadow prices are *zero* for constraints that are *nonbinding* at the optimal solution. 

.. note::

    **Proposition 9:** For any LP, shadow prices equal the values of dual variables in the dual optimal solution. 


**Proof:**

Let B be the old optimal basis and :math:`z = c_{B}^{T}A_{B}^{-1}b` be the old objective value. If :math:`b_{1}` becomes :math:`b^{'} = b_1 +1`, then :math:`z` becomes: 
:math:`z^{'} = z + (c_{B}^{T}A_{B}^{-1})_{1}`. So the shadow price of constraint 1 is :math:`z^{'} = z + (c_{B}^{T}A_{B}^{-1})_{1}`. In general, the shadow price of constraint 
:math:`i` is :math:`z^{'} = z + (c_{B}^{T}A_{B}^{-1})_{i}`. As :math:`c_{B}^{T}A_{B}^{-1}` is the dual optimal solution, the proof is complete. 








------------
Applications
------------


