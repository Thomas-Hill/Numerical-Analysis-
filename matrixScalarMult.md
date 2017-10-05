# Computational Mathematics Software Manual

## **Routine Name:** matrixScalarMult

**Description:** This function multiplies a scalar and matrix (a double, and a vector of
vector of doubles). 

**Input:**  This function takes a vector of vectors of doubles (representing a matrix), 
and double.  

**Output:** This function outputs a vector of vectors of doubles representing the product 
of the scalar and the Matrix.  

**Code Example:** 

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

vector <vector <double>> matrixScalarMult(vector <vector <double>> matrix, double scalar){
    for (int i = 0; i < matrix.size(); i++){
        for (int j = 0; j < matrix[i].size(); j++){
            matrix[i][j] = scalar*matrix[i][j];
            cout << matrix[i][j] << " " ;
        }
        cout << endl; 
    }
    
    return matrix; 
}

int main(){
    
    vector<vector<double>> C = {{-5,2,1},{1,2,-9},{0,-4,12}};
    vector<vector<double>> D = {{1,2,3},{1,2,3},{1,2,3}};
    vector<vector<double>> E = {{-1,10,32,100},{1000,2893,-1999,34},{147.33333,-2.718,3.14159,0}, {1,2,3,4}};
    vector<vector<double>> F = {{0,1,3,9},{-8,2,-19,3},{14734.333, 0,-3.14159,0},{1,2,3,4}};
    
    matrixScalarMult(C,2);
    cout << endl; 
    matrixScalarMult(D,3.8948);
    cout << endl; 
    matrixScalarMult(E,-12.2);
    cout << endl; 
    matrixScalarMult(F,0);
    cout << endl;
       
}

```

**Results:** 
```C++

-10 4 2 
2 4 -18 
0 -8 24 

3.8948 7.7896 11.6844 
3.8948 7.7896 11.6844 
3.8948 7.7896 11.6844 

12.2 -122 -390.4 -1220 
-12200 -35294.6 24387.8 -414.8 
-1797.47 33.1596 -38.3274 -0 
-12.2 -24.4 -36.6 -48.8 

0 0 0 0 
-0 0 -0 0 
0 0 -0 0 
0 0 0 0 

```

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
