# Computational Mathematics Software Manual

## **Routine Name:** matrix1Norm

**Description:** This routine calculates the 1-norm of a given matrix; i.e., the maximum
absolute column sum. 

**Input:**  It takes as an argument a vector of vectors of doubles. 

**Output:** It outputs a double representing the absolute 1-norm of the matrix. 

**Code Example:** In this example we calculate the matrix 1 norm for several matrices
given in the main function.  

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

double matrix1Norm (vector <vector<double>> matrix){
    double ans = 0; 
    for(int j = 0; j < matrix.size(); j ++){
        double sum = 0.0; 
        for(int i = 0; i < matrix[j].size(); i++){
            sum += abs(matrix[i][j]);
        }
        if(sum > ans) ans = sum; 
    }
    return ans;
}

int main(){
    vector<vector<double>> C = {{-5,2,1},{1,2,-9},{0,-4,12}};
    vector<vector<double>> D = {{1,2,3},{1,2,3},{1,2,3}};
    vector<vector<double>> E = {{-1,10,32,100},{1000,2893,-1999,34},{147.33333,-2.718,3.14159,0}, {1,2,3,4}};
    vector<vector<double>> F = {{0,1,3,9},{-8,2,-19,3},{14734.333, 0,-3.14159,0},{1,2,3,4}};
    vector<vector<double>> Y = {{1,2},{3,4},{5,6},{7,8}};
    
    cout << matrix1Norm(C) << endl;
    cout << matrix1Norm(D) << endl;
    cout << matrix1Norm(E) << endl;
    cout << matrix1Norm(F) << endl;
    cout << matrix1Norm(Y) << endl;
}
```

**Results:** 
```C++
22
9
2907.72
14743.3
6
```

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
