# Unormalized_Spectral_Clustering

This repository contains the implementation of a version of the Unnormalized Spectral Clustering Algorithm in Both C and Python, using Python C API wrapper. 

## About
The K-means algorithm is a popular clustering method for finding a partition of N unlabeled
observations x1, x2, . . . , xN ∈ Rd into K distinct clusters, where K is a parameter of the method.

Kmeans algorithm is an iterative algorithm that tries to partition the dataset 
into K distinct non-overlapping subgroups (clusters), where each data point belongs to exactly one cluster.
Each cluster k is represented by it’s centroid which is the mean µk ∈ Rd of the cluster’s members.

## Algorithm
1. Form the weighted adjacency matrix W (Weighted Adjacency Matrix) from X.
2. Compute the graph Laplacian L.
3. Compute the first k eigenvectors u1, . . . , uk corresponding to the k smallest eigenvalues of L (k is determined according to the Eigengap Heuristic).
   Finding Eigenvalues and Eigenvectors using e [Jacobi eigenvalue algorithm]:
   [Jacobi eigenvalue algorithm] is an iterative method for the calculation of the eigenvalues and eigenvectors of a real symmetric matrix (diagonalization).
   
5. Let U ∈ Rn×k be the matrix containing the vectors u1, . . . , uk of L as columns.
6. Treating each row of U as a point in Rk, cluster them into k clusters using K-means++.

The Eigengap Heuristic :
In order to determine the number of clusters k, we will use eigengap heuristic as follow:
let (δi)i=1,...,n−1 be the eigengap for the ascending ordered eigenvalues 0 = λ1 ≤ . . . ≤ λn of L, defined as: δi = |λi − λi+1|
The eigengap measure could indicate the number of clusters k through the following estimation:
k = argmaxi(δi), i = 1, . . . , jn//2 .
In case of equality in the argmax of some eigengaps, use the lowest index.


## Description

### 1. spkmeans.py: c
    - Reading user CMD arguments: 
    (a) k  (only available in Python implementation)
    (b) goal - Can get the following values: 
      [spk] (perform full spectral kmeans algorithm - only available in Python implementation), 
      [wam] (calculate and output the Weighted Adjacency Matrix),
      [ddg] (calculate and output the Diagonal Degree Matrix), 
      [gl] (calculate and output the Graph Laplacian), 
      [jacobi] (calculate and output the eigenvalues and eigenvectors).

    (c) file_name (.txt): The path to the Input file, it will contain N data points for all above
          goals except Jacobi, in case the goal is Jacobi the input is a symmetric matrix.
      
    - Implementation of the k-means++ initialization when the goal=spk. 
      if the goal=’spk’, the spk() method from the C module mykmeanssp is called, with passing the initial centroids, the datapoints and other arguments. 
    -  After getting the final centroids, they are printed as output.
    -  For every other goal argument, the corresponding function from the C module mykmeanssp is called.
    
### 3. spkmeans.h: C header file.
### 4. spkmeans.c: C interface  (same as detailed in the Python interface)
### 5. Utils.c : Useful basic functions.
### 6. spkmeanslogic.c : Kmeans algorithms implementation in C.
### 7. spkmeansmodule.c: Python C API wrapper.
### 8. setup.py: The setup file.
### 9. Makefile: make script to build the C interface.


# Output
### From the Python interface:
• Case of ’spk’: The first line contains the indices of the observations chosen by the K-means++ algorithm as the initial centroids. 
We refer to the first observation index as 0, the second as 1 and so on, up until N - 1. The second line onward contains the calculated final centroids from the K-means algorithm, separated by a comma, such
that each centroid is in a line of its own.
• Case of ’Jacobi’: The first line contains the eigenvalues, second line onward will be the corresponding eigenvectors (printed as columns).
• Else: output the required matrix separated by a comma, such that each row is in a line of its own.

### From the C interface:
• Case of ’Jacobi’: The first line contains the eigenvalues, second line onward will be the corresponding eigenvectors (printed as columns).
• Else: output the required matrix separated by a comma, such that each row is in a line of its own.


## Compile and Running
## C
Compile the C program using : ```make```.
After successful compilation, the program can run for Example: ```./spkmeans gl input_1.txt``` .

## Python
Compile the extension using: ```python3 setup.py build_ext --inplace```.
After successful compilation, the program can run for Example: ```python3 spkmeans.py 3 spk input_1.txt```.
