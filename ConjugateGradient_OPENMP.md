# Computational Mathematics Software Manual

## **Routine Name:** ConjugateGradient (using OPEN MP)

**Description:** This program represents a routine for solving a linear system Ax = b
using Conjugate Gradient.  Here we have parallelized the code using OPENMP.  

**Input:**  The function takes as arguments a matrix (a vector of vector of doubles), 
the right hand side of the equation (a vector of doulbes), a vector represeting an initial
guess, a double tol representing tolerance, an integer for maximum number of iterations,
an integer which counts the number of iterations until convergence.  

**Output:** It outputs a vector representing the solution obtained by Conjugate Gradient.

**Code Example:** In the code example below I will compute the solution to a matrix filled with random entries 
of size 1000x1000.  

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


vector <double> ConjugateGradient(vector <vector <double>> &A, vector <double> b, 
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
    
    vector <double> p0 = r0;
    
    while(delta0 > tol*tol*b_delta and k < maxIter){
        
        vector <double> s = matrixVectorProduct(A, p0);
        
        double alpha = delta0/dotProduct(p0,s);
      
        for(int i = 0; i < n; i++){
            x1[i] = x0[i] + alpha*p0[i];
            r0[i] = r0[i] - alpha*s[i];
        }
        
        double delta1 = dotProduct(r0, r0);
        
        for(int i = 0; i < n; i++){
            p0[i] = r0[i] + (delta1/delta0)*p0[i];
        }
        
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
    vector <double> ans3 = ConjugateGradient(B, sol, guess, .0001, 10, iter);

    //Now we print the result and verify that it matches what is obtained from a 
    // computer algebra system.  
    
    for(int i = 0; i < ans3.size(); i++){
        cout << ans3[i] << endl; 
    }
    
    // Now lets try this on a bigger matrix.  
    int n = 1000;
    vector <vector <double>> A; 
    vector <double> x0(n,0); 
    
    vector <double> x;
    x.resize(n); 
    A.resize(n);
    int iter4 = 0; 
    
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
    vector <double> ans_4 = ConjugateGradient(A, b, x0, .000001, 1000, iter4); 
    
    cout << "This is the 2-norm of the vector representing the error between the actual value and the result from Conjugate Gradient: "; 
    cout << vectorErr2(x, ans_4) << endl; 
    cout << endl << "Number of iterations Conjugate Gradient: " << iter4 << endl << endl;
    
   
}
```

**Results:** Below are the results of running the main function.  We see that the solution
for the simple 5x5 example matches results obtained in MAPLE.  Then we see that after 6
iterations we are below our specified tolerance.  The parallelized code runs in 0.055 seconds,
significantly less than the unparallelized code (at 0.149 seconds).  

```C++
0.00736412
0.00475563
0.00800279
0.00790298
0.00762929


This is the 2-norm of the vector representing the error between the actual value and the result from Conjugate Gradient: 0.00114214

Number of iterations Conjugate Gradient: 6


```

**Last Modification Date:** 11/12/2017

**Author:** Thomas Hill
