# Computational Mathematics Software Manual

## **Routine Name:** inverse_power_iteration

**Description:** This is a routine which represents the inverse power iteration method for finding 
smalles of the eigenvalues and corresponding eigenvectors of a matrix.  

**Input:**  Takes as an argument a matrix, a vector which represents an intial guess to
the eigenvector, a double which represents the tolerance, an integer representing maximum
number iterations, a vector representing the eigenvector.  

**Output:** The function outputs the smallest eigenvalue.  

**Code Example:**  Below we run the code an example of the routine on a small matrix to 
verify that it is working on a 2x2.  Then we fill a 1000x1000 matrix with random values
and compute the eigenvalue and eigenvector.  We output the eigenvalue. We use some 
routines from previous homework problems.  

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

//We use some routines from previous homework assignments.  

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

double dotProduct(vector<double> vect1, vector <double> vect2){
    double ans = 0;
    for(int i = 0; i < vect1.size(); i++){
        ans+=vect1[i]*vect2[i];
    }
    return ans; 
}


vector <double> ConjugateGradient(vector <vector <double>> &A,  const vector <double>& b, 
        vector <double> x0, double tol, int maxIter){
    
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
    return x0; 
}



double inverse_power_iteration(vector <vector<double>> &A, vector <double> v0, double tol, 
                int maxIter, vector <double> &eigVec){
    vector <double> y = ConjugateGradient(A, v0, v0, tol, maxIter); 
    
    int cnt = 0; 
    double err = 10*tol; 
    double lambdaOld = 0.0; 
    int m = y.size(); 
    vector <double> x(m,0); 
    double lambdaMax; 
    
    while(tol < err and cnt < maxIter){
        double p = sqrt(dotProduct(y,y));
        for(int i = 0; i < m; i++){
            x[i] = y[i]/p; 
        }
        
        y = ConjugateGradient(A, x, x, tol, maxIter);
        lambdaMax = dotProduct(x,y);
        err = abs(lambdaMax - lambdaOld); 
        lambdaOld = lambdaMax; 
        cnt ++; 
    }
    
    eigVec = x; 
    
    return 1.0/lambdaMax; 
}




int main(){
    srand(time(NULL));
    //First let's run our code on a simple example to verify that it is working. 
    vector <vector <double>> B = {{2, .5}, {.5, 2}};
    vector <double> v = {1,3}; 
    vector <double> eigVec; 
    
    cout << "Here is the eigenvalue for Power Iteration: "; 
    cout << power_iteration(B, v, .000000001, 10000, eigVec) << endl; 
    cout << "Here is the eigenvector for Power Iteration: " << endl; 
    for(int i = 0; i < eigVec.size(); i ++){
        cout  << eigVec[i] << endl; 
    }
    
    v = {1,3};
    
    cout <<  endl << endl << "Here is the eigenvalue for Inverse Power Iteration: "; 
    cout << inverse_power_iteration(B, v, .00001, 1000, eigVec) << endl;
    
    cout << "Here is the eigenvector for Inverse Power Iteration: " << endl; 
    for(int i = 0; i < eigVec.size(); i ++){
        cout  << eigVec[i] << endl; 
    }
    
    
    // Now let's try this on a bigger matrix.  
    int n = 1000;
    vector <vector <double>> A; 
    vector <double> x0(n,0); 
    
    vector <double> x;
    x.resize(n); 
    A.resize(n);

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
    
    cout << endl << endl; 
        cout << inverse_power_iteration(A, x, .000001, 1000, eigVec) << endl;
    
    
    
    
}
```

**Results:** 
```C++


Here is the eigenvalue for Inverse Power Iteration: 1.50001
Here is the eigenvector for Inverse Power Iteration: 
-0.704022
0.710178


826.564
```

**Last Modification Date:** 11/18/2017

**Author:** Thomas Hill
