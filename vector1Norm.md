# Computational Mathematics Software Manual

## **Routine Name:** vector1Norm

**Description:** This routine takes as arguements a vector and calculates the 1-norm of
the vector; i.e., the sum of the absolute values of the components.  

**Input:**  A vector of doubles. 

**Output:** A double representing the 1-norm of the vector.

**Code Example:** Below we calculate the one-norm for the vectors [1,2,1], [1,1,1], 
and [-10, 3.1415926535, log(.145), 37, 101.11111].
    

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

double vector1Norm(vector<double> vect){
    double ans = 0; 
    for(int i = 0; i < vect.size(); i++){
        ans+= abs(vect[i]);
    }
    
    return ans;
}

int main(){
    
    vector <double> A = {1,2,1};
    vector <double> B = {1,1,1};
    vector <double> V = {-10, 3.1415926535, log(.145), 37, 101.11111};
    
    cout << vector1Norm(A) << endl;
    cout << vector1Norm(B) << endl;
    cout << vector1Norm(V) << endl; 
}


```

**Results:** 
```C++
4
3
153.184
```
This agrees with what we obtain from doing this in Maple.  

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
