# Computational Mathematics Software Manual

## **Routine Name:** SteepestDescent

**Description:** This program represents a routine for solving a linear system Ax = b
using Steepest Descent method.  Here we have parallelized the code using OPENMP.    

**Input:**  The function takes as arguments a matrix (a vector of vector of doubles), 
the right hand side of the equation (a vector of doulbes), a vector represeting an initial
guess, a double tol representing tolerance, an integer for maximum number of iterations,
an integer which counts the number of iterations until convergence.  

**Output:** It outputs a vector representing the solution obtained by Steepest Descent.

**Code Example:** 
In the code example below I will compute the solution to a matrix filled with random entries 
of size 1000x1000. (Also a simple 5x5 example to make sure that everything is order).  

```C++

#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;


//Here is some code from previous homework problems that is implemented here.

vector <double> matrixVectorProduct(vector<vector<double>> matrix, vector<double> vect){
    vector <double> ans; 
    for(int i = 0; i < vect.size(); i ++){
        ans.push_back(0); 
    }
    
    #pragma omp parallel for  
    for(int i = 0; i < matrix.size(); i++){
        for(int j = 0; j < vect.size(); j++){
            ans[i] += matrix[i][j]*vect[j];
        }
    }
    
    return ans; 
}

double vector2Norm(vector <double> vect){
    double ans = 0;
    int size = vect.size();
    for(int i = 0; i < vect.size(); i++){
        ans+= pow(abs(vect[i]),2);
    }
    ans = sqrt(ans);
    
    return ans; 
}

double vectorErr2(vector <double> vect1, vector <double> vect2){
    vector <double> ans; 
    for(int i = 0; i < vect1.size(); i++){
        ans.push_back(vect1[i] - vect2[i]);
    }
    return vector2Norm(ans);
    
}

double dotProduct(vector<double> vect1, vector <double> vect2){
    double ans = 0;
    for(int i = 0; i < vect1.size(); i++){
        ans+=vect1[i]*vect2[i];
    }
    return ans; 
}

vector <double> SteepestDescent(vector <vector <double>> &A, vector <double> b, 
        vector <double> x0, double tol, int maxIter, int &iter){
    
    int n = b.size(); 
    vector <double> x1(n,0); 
    vector <double> r0(n,0);
    
    vector <double> Ax0 = matrixVectorProduct(A, x0);
    
    for(int i = 0; i < n; i ++){
        r0[i] = b[i] - Ax0[i];
    }
    
    double delta0 = dotProduct(r0,r0);
    double b_delta = dotProduct(b,b);
    
    int k = 0; 
    
    
    while(delta0 > tol*tol*b_delta and k < maxIter){
        vector <double> s = matrixVectorProduct(A, r0);
        double alpha = delta0/dotProduct(r0,s);
        
        for(int i = 0; i < n; i++){
            x1[i] = x0[i] + alpha*r0[i];
            r0[i] = r0[i] - alpha*s[i];
        }
        
        double delta1 = dotProduct(r0, r0);
        
        k++;
        
        delta0 = delta1; 
        x0 = x1; 
        
    
    }
    iter = k; 
    return x0;
    
}


int main(){
    
    
    //First lets check that it works on simple 5x5 example.  
    
    vector <vector <double>> B = {{500, 1, 40, 2, -3},{2, 800, 20, -8, 11},{-1, 
        -2, 500, 1, 1},{1, 2, 3, 500, 1},{1, 4, 8, 12, 500}};
    vector <double> guess = {20, 3, 5, 7, 9};
    vector <double> sol = {4, 4, 4, 4, 4};
    int iter; 
    
    //Call the function to solve the system Bx = sol, for our intial guess.
    vector <double> ans2 = SteepestDescent(B, sol, guess, .0001, 10, iter);
   

    //Now we print the result and verify that it matches what is obtained from a 
    // computer algebra system.  
    
    for(int i = 0; i < ans2.size(); i++){
        cout << ans2[i] << endl; 
    }
    
    cout << endl << endl; 
    
    // Now lets try this on a bigger matrix.  
    int n = 1000;
    vector <vector <double>> A; 
    vector <double> x0(n,0); 
    
    vector <double> x;
    x.resize(n); 
    A.resize(n);
    int iter3 = 0;
     
    //Here I am filling the matrix A with random values, creating a random guess,
    // and creating a random solution vector.  For our example A is symmetric, 
    // positive definite.  
    for (int i = 0; i < n; i++){
        A[i].resize(n);
        x[i] = rand()%11 + 1;
        x0[i] = rand()%11 + 1;
        for(int j = 0; j < i+1; j++){
            A[i][j] = rand()%11 + 1; 
            A[j][i] = A[i][j]; 
            if(i == j) A[i][j]+= 1000; //this is to ensure diagonal dominance.  
        }
    }
    
    // Here I compute the right-hand side for the matrix.  
    vector<double> b = matrixVectorProduct(A,x);
    
    //Now I compute the solution using the routine 
    vector <double> ans_3 = SteepestDescent(A, b, x0, .000001, 1000, iter3); 
    vector <double> ans_4 = ConjugateGradient(A, b, x0, .000001, 1000, iter4); 
    
       
    
    cout << "This is the 2-norm of the vector representing the error between the actual value and the result from Steepest Descent: "; 
    cout << vectorErr2(x, ans_3) << endl; 
    cout << endl << "Number of iterations Steepest Descent: " << iter3 << endl << endl; 
    
   
}

```
**Results:**  Below are the results of the simple 5x5 example. Also the error for computation 
on the random 1000x1000 matrix.  It takes 0.055 seconds to run this code using OPENMP.  
Signifcantly less than the 0.149 seconds needed to run the code unparallelized.  

```C++
0.00736423
0.00475534
0.00800275
0.00790301
0.0076294


This is the 2-norm of the vector representing the error between the actual value and the result from Steepest Descent: 0.000830666

Number of iterations Steepest Descent: 18

```

**Last Modification Date:** 11/19/2017

**Author:** Thomas Hill
