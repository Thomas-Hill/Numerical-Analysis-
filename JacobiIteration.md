# Computational Mathematics Software Manual

## **Routine Name:** JacobiIteration  

**Description:** This program represents a routine for solving a linear system Ax = b
using Jacobi Iteration.  

**Input:**  The function takes as arguments a matrix (a vector of vector of doubles), 
the right hand side of the equation (a vector of doulbes), a vector represeting an initial
guess, a double tol representing tolerance, an integer for maximum number of iterations,
an integer which counts the number of iterations until convergence.  

**Output:** It outputs a vector representing the solution obtained by Jacobi Iteration.

**Code Example:** I encountered memory problems when trying to do matrices larger than 150. 
In the code example below I will compute the solution to a matrix filled with random entries 
of size 100x100.  

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



vector <double> JacobiIteration(vector <vector <double>> &A, vector <double> b, 
                    vector <double> x0, double tol, int maxIter, int &iter){
    
    int n = b.size();
    
    vector <double> r(n,0); 
    vector<double> x1(n,0); 
    
    int k = 0; 
    double error = tol*10; 
    
    while(error > tol and k < maxIter){
        
        for(int i = 0; i < n; i++){
            double ans = 0.0; 
            for(int j = 0; j < i; j++){
                ans += A[i][j]*x0[j];
            }
            for(int j= i+1; j<n; j++){
                ans += A[i][j]*x0[j];
            }
            x1[i] = (b[i] - ans)/A[i][i];
        }
        
     
        
        error = vectorErr2(x1,x0);
        
        k++;
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
    vector <double> ans = JacobiIteration(B, sol, guess, .0001, 10, iter); 

    //Now we print the result and verify that it matches what is obtained from a 
    // computer algebra system.  
    for(int i = 0; i < ans.size(); i++){
        cout << ans[i] << endl; 
    }
    
    cout << endl << endl; 
       
    // Now lets try this on a bigger matrix.  
    int n = 100;
    vector <vector <double>> A; 
    vector <double> x0(n,0); 
    
    vector <double> x;
    x.resize(n); 
    A.resize(n);
    int iter1 = 0; 

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
    vector <double> ans_1 = JacobiIteration(A, b, x0, .000001, 1000, iter1); 
        
    cout << "This is the 2-norm of the vector representing the error between the actual value and the result from Jacobi Iteration: "; 
    cout << vectorErr2(x, ans_1) << endl; 
    cout << endl << "Number of iterations for Jacobi: " << iter1 << endl << endl; 
       
}
```

**Results:** Below are the results of running the main function.  We see that the solution
for the simple 5x5 example matches results obtained in MAPLE.  Then we see that after 33
iterations we are below our specified tolerance.  

```C++

0.00736442
0.00475563
0.00800269
0.00790297
0.0076295


This is the 2-norm of the vector representing the error between the actual value and 
the result from Jacobi Iteration: 3.19907e-07

Number of iterations for Jacobi: 33
```

**Last Modification Date:** 11/12/2017

**Author:** Thomas Hill
