# Computational Mathematics Software Manual

## **Routine Name:** vectorInfNorm

**Description:** This routine calculates the infinity norm of a vector; i.e., the absolute
value of the largest component of the vector. 

**Input:**  a vector of doubles. 

**Output:** a double that represents the infinity norm of the matrix. 

**Code Example:**  In this example we calculate the infinity-norm for the vectors [1,2,1], [1,1,1], and [-10, 3.1415926535, log(.145), 37, 101.11111].

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

double maxEntry(vector <double> vect){
    double ans = max(abs(vect[0]), abs(vect[1])); 
    for(int i = 2; i < vect.size(); i++){
        ans = max(ans, abs(vect[i]));
    }
    return ans;
}

double vectorInfNorm(vector <double> vect){
    double ans = 0; 
    int size = vect.size();
    ans = maxEntry(vect);
    return ans; 
    
}

int main(){
    
    vector <double> A = {1,2,1};
    vector <double> B = {1,1,1};
    vector <double> V = {-10, 3.1415926535, log(.145), 37, 101.11111};
    
    cout << vectorInfNorm(A) << endl;
    cout << vectorInfNorm(B) << endl;
    cout << vectorInfNorm(V) << endl;
    
}

```

**Results:** 
```C++

2
1
101.111

```

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
