# Computational Mathematics Software Manual

## **Routine Name:** matrixMatrixProduct

**Description:** This routine computes the product of two matrices.  In the example below 
we use openmp to paralellize the operation.  We run this program on 8 threads.  

**Input:**  It takes two matrices, each represented as a vector of vectors of doubles.
We assume that the user is competent enough to input matrices of the appropriate size.  

**Output:** It outputs a vector of vectors of doubles representing the product of the matrices. 
The output of the main function in this example is the time necessary to the square of 
the matrix and the size of the matrix.  

**Code Example:** Below we compute the square of a n by n matrix for a matrix of size
1 by 1, 101 by 101, ..., 1901 by 1901.  These matrices are filled with random integers 
from 0 to 100. 

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

vector <vector <double>> matrixMatrixProduct(vector <vector <double>> matrix1, vector <vector <double>> matrix2){
    
    vector <vector <double>> ans; 
    int m = matrix1.size();
    int n = matrix1[0].size();
    if (n != matrix2.size()){
        cout << "Error, matrices not compatible for multiplication. " << endl;
        return ans; 
    }
    
    int p = matrix2[0].size();
    
    ans.resize(m);
    
    for (int i = 0; i < m; i++){
        ans[i].resize(p);
    }
    
    #pragma omp parallel for 
    for(int i=0;i<m;i++){
        for(int j=0; j<p; j++){
            ans[i][j]=0;
        }
    }
    
    #pragma omp parallel for         
    for(int i = 0; i < m; i++){
        for(int j = 0; j < p; j++){
            for(int k = 0; k < n; k++){
                ans[i][j] += matrix1[i][k]*matrix2[k][j];
            }
        }
    }
    
   return ans;
}



int main(){

vector <vector <double>> A; 
    vector <double> v; 
    
    srand(time(NULL)); 
    
    for (int M = 1; M < 2000; M += 100){
        A.resize(M);
        for (int i = 0; i < M; i++){
            A[i].resize(M); 
            for(int j = 0; j < M; j++){
                A[i][j] = rand()%101; 
            }
        }
        high_resolution_clock::time_point t1 = high_resolution_clock::now();
        matrixMatrixProduct(A,A);
        high_resolution_clock::time_point t2 = high_resolution_clock::now();
        duration<double> time_span = duration_cast<duration<double>>(t2 - t1);
        
        cout << time_span.count() << " " << M << endl; 
    }
    
}
```

**Results:** These results are plotted and compared with the times taken to run on a 
single thread, in the write up for homework 6.  
```C++
.818e-3 1
.77715e-2 101
.610733e-1 201
.209921 301
.501093 401
.974114 501
1.75393 601
3.19625 701
4.99039 801
7.43043 901
10.3573 1001
13.5237 1101
19.1889 1201
25.8319 1301
32.0778 1401
40.5137 1501
`47.183 1601
59.2684 1701
68.3969 1801
80.9098 1901
```

**Last Modification Date:** 10/18/17

**Author:** Thomas Hill
