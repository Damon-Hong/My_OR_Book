=======================
Optimization Algorithms
=======================

.. contents:: 

------------------
The Simplex Method
------------------


------------------------------
The Branch and Bound Algorithm
------------------------------



------------------------------------
Gradient Descent and Newton's Method
------------------------------------ 

--------
Examples
--------

Machine Scheduling Problem
==========================

Fifteen jobs, each with its processing time, should be scheduled on three machines. 
If two jobs cannot be scheduled on the same machine, they are called conflicting jobs. 
Table 1 lists the job IDs, processing times, and sets of conflicting jobs. 
For example, we cannot schedule any pair of jobs out of jobs 2, 5, 8 on the same machine. 
Note that a job may have no conflicting jobs. 

=====  ================  ================
Job    Processing time   Conflicting jobs
=====  ================  ================
1      7                 None 
2      4                 5,8
3      6                 None
4      9                 None
5      12                2,8
6      8                 9
7      10                10
8      11                2,5
9      8                 6
10     7                 7
11     6                 15
12     8                 None
13     15                None
14     14                None
15     3                 11 
=====  ================  ================

We want to schedule the jobs to minimize makespan. For example, we may schedule jobs 1, 4, 7, 8, 
and 13 to machine 1, jobs 2, 6, 10, 11, and 14 to machine 2, and jobs 3, 5, 9, 12, and 15 to machine 3. 
The total processing times on the three machines are 52, 39, and 37, respectively. 
The makespan is thus 52. While this is a feasible schedule, this may or may not be an optimal schedule. 
When we try to improve the schedule, be careful about conflicting jobs. For example, we cannot exchange 
jobs 8 and 11 (even though this reduces the makespan) because that will result in machine 2 processing 
conflicting jobs 2 and 8, which is infeasible. 

Formulate a linear integer program that generates a feasible schedule to minimize makespan. 
Then write a computer program (e.g., using Python to invoke Gurobi Optimizer) to solve this instance 
and obtain an optimal schedule. Write down the minimized makespan (i.e. the objective value of 
an optimal solution).


.. math::

    \min_{X_{i,j}, w} w   

subject to:

.. math::
     
    w \geq \sum_{j \in \mathcal{J}} t_{j} X_{i,j} , \forall i \in \mathcal{I} \\
    \sum_{i \in \mathcal{I}} X_{i,j} = 1 , \forall j \in \mathcal{J} \\
    \sum_{j \in \Omega_{1,2,3,4}} X_{i,j} \leq 1 , \forall i \in \mathcal{I} 


where :math:`i \in \mathcal{I}` denotes machine index. :math:`j \in \mathcal{J}` denotes job index.

Conflicting job sets are :math:`\left\{\begin{matrix}
\Omega_{1} & \{ 2,5,8 \}\\ 
\Omega_{2} & \{ 6,9 \}\\ 
\Omega_{3} & \{ 7,10 \}\\ 
\Omega_{4} & \{ 11,15 \}
\end{matrix}\right.`  


Decision variable :math:`X_{i,j} = \left\{\begin{matrix}
1 & \text{if job } j \text{ is allocated to machine } i\\ 
0 & \text{otherwise}
\end{matrix}\right.`

Decision variable :math:`w` is a trivial positive value. 


.. code::

    import numpy as np 
    import cvxpy as cp 

    X = cp.Variable(shape=(3,15),boolean=True)
    w = cp.Variable()

    t = np.array([[7,4,6,9,12,8,10,11,8,7,6,8,15,14,3]]).T 

    obj = cp.Minimize(w) 

    constrs = []

    constrs += [w >= X[0,:]@t]
    constrs += [w>= X[1,:]@t]
    constrs += [w >= X[2,:]@t]

    for j in range(15):
        constrs += [X[0,j]+X[1,j]+X[2,j] == 1]
        
    for i in range(3):
        constrs += [X[i,1]+X[i,4]+X[i,7] == 1]
        constrs += [X[i,5]+X[i,8] <= 1]
        constrs += [X[i,6]+X[i,9] <= 1]
        constrs += [X[i,10]+X[i,14] <= 1]

    prob = cp.Problem(obj, constrs)

    prob.solve(solver=cp.GUROBI)

