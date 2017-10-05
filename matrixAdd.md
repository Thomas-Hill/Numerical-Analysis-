# Computational Mathematics Software Manual

## **Routine Name:** matrixAdd

**Description:** This function adds two matrices together.  We assume the user is 
competent enough to put matrices of the same size into this routine. 

**Input:**  The routine takes two matrices (vector of vectors of doubles) of the same size.

**Output:** The routine outputs the sum of the two matrices. 

**Code Example:** Here we add two pairs of matrices given in the main function below. 

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

vector <vector <double>> matrixAdd(vector <vector <double>> matrix1, vector <vector <double>> matrix2){
    for (int i = 0; i < matrix1.size(); i++){
        for (int j = 0; j < matrix1[i].size(); j++){
            matrix1[i][j] += matrix2[i][j];
            cout << matrix1[i][j] << " " ;
        }
        cout << endl; 
    }
    
    return matrix1; 
}

int main(){
    
    vector<vector<double>> C = {{-5,2,1},{1,2,-9},{0,-4,12}};
    vector<vector<double>> D = {{1,2,3},{1,2,3},{1,2,3}};
    vector<vector<double>> E = {{-1,10,32,100},{1000,2893,-1999,34},{147.33333,-2.718,3.14159,0}, {1,2,3,4}};
    vector<vector<double>> F = {{0,1,3,9},{-8,2,-19,3},{14734.333, 0,-3.14159,0},{1,2,3,4}};
    
    matrixAdd(C,D);
    cout << endl; 
    matrixAdd(E,F);
    cout << endl;   
}

```

**Results:** 
```C++
-4 4 4 
2 4 -6 
1 -2 15 

-1 11 35 109 
992 2895 -2018 37 
14881.7 -2.718 0 0 
2 4 6 8 

```

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
