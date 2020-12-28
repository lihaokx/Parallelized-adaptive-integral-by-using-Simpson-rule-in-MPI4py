# Parallelized-adaptive-integral-by-using-Simpson-rule-in-MPI4py

1. **Q1.ipynb** is the 1D parallelized adaptive integral algorithm.
2. **Q2.ipynb** is an example of Q1.ipynb
3. **Q3.ipynb** is the parallelized adaptive triple integral algorithm by using Simpson rule in MPI4py
4. **Q4Volume.ipynb** is to calculate the volume in a cubed-sphere sector.
5. **Q4Centroid.ipynb** is to calculate the coordinate of centroid in a cubed-sphere sector.
6. **Q4b.ipynb** is to calculate the cell average of a function. 


To calculate the Parallelized-adaptive integral by using, I used the Simpson rule and Compoiste Simpson rule. The MPI module is MPI4py

"""
This code uses the 1-D parallelized adaptive algorithm to calculate the integral of a funciton f.
a, b is the integration inteval.
tol is the tolerance.
f is the function of integrand.
result is the approximate result of the integration.
errorEstimate is the error estimated.
finestLevel is the finest grid in the interval.

Some part of the code could be referred to the following link. 
https://github.com/stefantaylor/Parallel-Programming-Languages-and-Systems/blob/master/cw2/aquadPartA.c

A stack is used to store the sub-interval of the domain and sub-tolerance. It is initilized with [a, b, tol].

A farmer cpu is used to monitor the other worker cpu, which are used to calculate the integral or divide the interval.

The main idea of the algorithm is as follows:
1.  A worker cpu calculate the integral I1 and I2 by using Simpson rule and composite Simpson rule
        I1 = h*(fleft + fmid*4 + fright )/6
        I2 = h*(fleft + 4*f((3*left+right)/4) + fmid*2 + 4*f((left+3*right)/4) + fright )/12
2.  If the difference between them is smaller than the tolrance, it will return I2 to the farmer cpu.
    If not, it will divide the tomain by 2, and send the interval to farmer cpu. This worker cpu will be idle.
3.  The farmer cpu pop a interval from the stack, and sends it to a worker cpu which is idle.
4.  After receiving the sub-interval from the worker cpus, the farmer cpu will add them to the stack.  

"""


