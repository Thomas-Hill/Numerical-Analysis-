# Computational Mathematics Software Manual

## **Routine Name:** vectorErr1

**Description:** This routine calculates the error between two vectors with respect to 
the 1-norm.

**Input:**  Takes two vectors.  It also uses a function previously defined and explained
vector1Norm. 

**Output:** It outputs double representing the error with respect to the one norm. 

**Code Example:** Here we find the error or difference between pairs of vectors given
in the main function.  

```C++

#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

double vectorErr1(vector <double> vect1, vector <double> vect2){
    vector <double> ans; 
    for(int i = 0; i < vect1.size(); i++){
        ans.push_back(vect1[i] - vect2[i]);
    }
    return vector1Norm(ans);
    
}

int main(){
    
    vector <double> A = {1,2,1};
    vector <double> B = {1,1,1};
    vector <double> V = {-10, 3.1415926535, .145, 37};
    vector <double> W = {1,2,3,4};
    
    cout << vectorErr1(A,B) << endl;
    cout << vectorErr1(V,W) << endl;   
    
}
```

**Results:** 
```C++
1
47.9966
```

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
