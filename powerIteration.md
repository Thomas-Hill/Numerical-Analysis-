# Computational Mathematics Software Manual

## **Routine Name:** power_iteration

**Description:** This is a routine which represents the power iteration method for finding 
the eigenvalues and eigenvectors of a matrix.  

**Input:**  Takes as an argument a matrix, a vector which represents an intial guess to
the eigenvector, a double which represents the tolerance, an integer representing maximum
number iterations, a vector representing the eigenvector.  

**Output:** The function outputs the largest eigenvalue.  

**Code Example:**  Below we run the code an example of the routine on a small matrix to 
verify that it is working on a 2x2.  Then we fill a 1000x1000 matrix with random values
and compute the eigenvalue and eigenvector.  We output the eigenvalue.  

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

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


double power_iteration(vector <vector<double>> &A, vector <double> v0, double tol, 
                int maxIter, vector <double> &eigVec){
    
    vector <double> y = matrixVectorProduct(A, v0); 
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
        y = matrixVectorProduct(A, x);
        lambdaMax = dotProduct(x,y);
        err = abs(lambdaMax - lambdaOld); 
        lambdaOld = lambdaMax; 
        cnt ++; 
    }
    
    eigVec = x; 
    
    return lambdaMax; 
    
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
    cout << power_iteration(A, x, .0000001, 1000, eigVec) << endl;      
}

```

**Results:** 
```C++

Here is the eigenvalue for Power Iteration: 2.5
Here is the eigenvector for Power Iteration: 
0.707094
0.70712

7002.42
```

**Last Modification Date:** 11/18/2017

**Author:** Thomas Hill
