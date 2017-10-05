# Computational Mathematics Software Manual

## **Routine Name:** matrixInfNorm

**Description:** This routine calculates the infinity norm of a given matrix; i.e., the
maximum absolute row sum.  

**Input:**  It takes as an argument a vector of vectors of doubles. 

**Output:** It outputs the infinity norm of the matrix as a double. 

**Code Example:** In the example we calculate the infinity norm for several matrices, 
given in the main function. 

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

double matrixInfNorm(vector <vector<double>> matrix){
    double ans = 0; 
    for(int j = 0; j < matrix.size(); j ++){
        double sum = 0.0; 
        for(int i = 0; i < matrix[j].size(); i++){
            sum += abs(matrix[j][i]);
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
    
    cout << matrixInfNorm(C) << endl;
    cout << matrixInfNorm(D) << endl;
    cout << matrixInfNorm(E) << endl;
    cout << matrixInfNorm(F) << endl;
    cout << endl;
    
}

```

**Results:** 
```C++
16
6
5926
14737.5
```

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
