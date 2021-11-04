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




------------
Applications
------------



