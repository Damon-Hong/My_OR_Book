===================
Integer Programming
===================

.. contents::

--------
IP Model 
--------

----------------
Solution Methods
---------------- 


The Branch and Bound Algorithm
==============================


------------
Applications
------------

Machine Scheduling Problem
==========================

Fifteen jobs, each with its processing time, should be scheduled on three machines. 
If two jobs cannot be scheduled on the same machine, they are called conflicting jobs. 
Table 1 lists the job IDs, processing times, and sets of conflicting jobs. 
For example, we cannot schedule any pair of jobs out of jobs 2, 5, 8 on the same machine. 
Note that a job may have no conflicting jobs. 

.. table:: Table 1: Data

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


Facility Location Problem 
=========================

A city is divided into :math:`n` districts. The time (in minutes) it takes an ambulance to travel from District :math:`i` to 
District :math:`j` is denoted as :math:`d_{ij}`. The population of District :math:`i` (in thousands) is :math:`p_{i}`. 
An example is shown in Table 2 and Table 3.  The distances between districts are shown in Table 2, and the population 
information is shown in Table 3.  In this instance, we have :math:`n = 8` districts. We may see that, e.g., it takes 5 
minutes to travel from District 2 to District 3, and there are 40,000 citizens.

.. table:: Table 2: distances

    ========  ==  ==  ==  ==  ==  ==  ==  ==
    District  1   2   3   4   5   6   7   8
    ========  ==  ==  ==  ==  ==  ==  ==  ==
    1         0   3   4   6   8   9   8   10
    2         3   0   5   4   8   6   12  9
    3         4   5   0   2   2   3   5   7
    4         6   4   2   0   3   2   5   4
    5         8   8   2   3   0   2   2   4
    6         9   6   3   2   2   0   3   2
    7         8   12  5   5   2   3   0   2
    8         10  9   7   4   4   2   2   0
    ========  ==  ==  ==  ==  ==  ==  ==  == 

.. table:: Table 3: District Population 

    ========  ==========
    District  Population
    ========  ==========
    1         40
    2         30
    3         35
    4         20
    5         15
    6         50
    7         45
    8         60
    ========  ==========

The city has mm ambulances and wants to locate them to mm of the districts. For each district, the population-weighted 
firefighting time is defined as the product of the district population times the amount of time it takes for the closest 
ambulance to travel to it. The decision maker aims to locate the mm ambulances to minimize the maximum population-weighted 
firefighting time among all districts.

As an example, suppose that :math:`m = 2`, :math:`n = 8`, :math:`d_{i,j}` and :math:`p_{i}` are provided in Table 2, 
and the two ambulances are located in District 1 and 8. We then know that for Districts 1, 2, and 3 the closest ambulance 
is in District 1 and for the remaining five districts the closet ambulance is in District 8. The firefighting time for the 
eight districts are thus 0, 3, 4, 4, 4, 2, 2, and 0 minutes, respectively. The population-weighted firefighting times may 
then be calculated as 0, 90, 140, 80, 60, 100, 90, and 0. The maximum among the eight districts is therefore 140. 

For this problem, formulate an integer program that can minimize the maximum population-weighted firefighting time among all 
districts. Then write a program to invoke a solver (e.g., write a Python program to invoke Gurobi Optimizer) to solve the 
above instance and find an optimal solution for each problem. Write down the minimized maximum population-weighted 
firefighting times among all districts of the two districts that ambulances should be located in (i.e., the objective value 
of an optimal solution). 

.. math:: 

    \min_{X_{i,j}, w} w 

subject to: 

.. math:: 

    & \sum_{j \in \mathcal{N}} x_{j} = m \\
    & Y_{i,j} \leq x_{j}, \forall i,j \in \mathcal{N} \\ 
    & \sum_{j \in \mathcal{N}} Y_{i,j} = 1 , \forall i \in \mathcal{N} \\ 
    & w \geq \sum_{j \in \mathcal{N}} d_{i,j} p_{i} Y_{i,j} , \forall i \in \mathcal{N} \\ 
    & x_{i}, Y_{i,j} \in \{ 0,1 \}, \forall i,j \in \mathcal{N} \\ 
    & w \geq 0 


where Decision variable :math:`Y_{i,j} = \left\{\begin{matrix}
1 & \text{if for District } i \text{ the cloest ambulance is located in District } j\\ 
0 & \text{otherwise}
\end{matrix}\right.`

Decision variable :math:`x_{j} = \left\{\begin{matrix}
1 & \text{if an ambulance is located in District } j\\
0 & \text{otherwise}
\end{matrix}\right.`

Decision variable :math:`w` is a trivial positive value. 


.. code:: 

    import numpy as np 
    import cvxpy as cp 

    m = 2
    n = 8 

    d = np.array([[0,3,4,6,8,9,8,10],
                [3,0,5,4,8,6,12,9],
                [4,5,0,2,2,3,5,7],
                [6,4,2,0,3,2,5,4],
                [8,8,2,3,0,2,2,4],
                [9,6,3,2,2,0,3,2],
                [8,12,5,5,2,3,0,2],
                [10,9,7,4,4,2,2,0]]) 

    p = np.array([[40,30,35,20,15,50,45,60]]).T 

    x = cp.Variable((n,1), boolean=True)
    Y = cp.Variable((n,n), boolean=True)
    w = cp.Variable()

    obj = cp.Minimize(w) 

    constrs = [cp.sum(x) == m]

    for i in range(n):
        constrs += [cp.reshape(Y[i,:],(1,n)) <= x.T]

    for i in range(n):
        constrs += [cp.sum(Y[i,:]) == 1]
        
    for i in range(n): 
        constrs += [w >= d[i,j]*p[i]*Y[i,j] for j in range(n)]
        

    prob = cp.Problem(obj, constrs) 

    prob.solve(solver=cp.GUROBI)

