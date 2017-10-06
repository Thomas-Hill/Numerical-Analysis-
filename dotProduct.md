# Computational Mathematics Software Manual

## **Routine Name:** dotProduct 

**Description:** This routine computes the dot product or inner product of two vectors. 

**Input:**  It takes as arguments two vectors. We assume that the user is competent 
enough to input vectors of compatible length. 

**Output:** It outputs a scalar representing the inner product. 

**Code Example:** In the example we compute the dot product of two pairs of vectors 
given in the main function.  

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

double dotProduct(vector<double> vect1, vector <double> vect2){
    double ans = 0;
    for(int i = 0; i < vect1.size(); i++){
        ans+=vect1[i]*vect2[i];
    }
    return ans; 
}

int main(){

    vector <double> A = {1,2,1};
    vector <double> B = {1,1,1};
    vector <double> V = {-10, 3.1415926535, .145, 37};
    vector <double> W = {1,2,3,4};
    
    cout << dotProduct(A,B) << endl;
    cout << dotProduct(V,W) << endl;
}
```

**Results:** 
```C++
4
144.718   
```

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
