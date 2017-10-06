# Computational Mathematics Software Manual

## **Routine Name:** matrixMatrixProduct

**Description:** This routine computes the product of two matrices.  

**Input:**  It takes two matrices, each represented as a vector of vectors of doubles.
We assume that the user is competent enough to input matrices of the appropriate size.  

**Output:** It outputs a vector of vectors of doubles representing the product of the matrices. 

**Code Example:** Below we compute the product of two pairs of matrices.  

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

void printMatrix(vector<vector<double>> a)
{
    for (int i=0; i<a.size(); i++)
    {
        for (int j=0;j<a[i].size();j++){
            std::cout << a[i][j] << " ";
        }
        cout << endl;
    }
}

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
    
    for(int i=0;i<m;i++){
        for(int j=0; j<p; j++){
            ans[i][j]=0;
        }
    }
            
    for(int i = 0; i < m; i++){
        for(int j = 0; j < p; j++){
            for(int k = 0; k < n; k++){
                ans[i][j] += matrix1[i][k]*matrix2[k][j];
            }
        }
    }
    
    printMatrix(ans);
    return ans;
}


int main(){

    matrixMatrixProduct(C,D);
    cout << endl; 
    matrixMatrixProduct(X,Y);
    
}
```

**Results:** 
```C++
-2 -4 -6 
-6 -12 -18 
8 16 24 

50 60 
30 40 
```

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
