# Computational Mathematics Software Manual

## **Routine Name:** vector2Norm

**Description:** This is a routine for calculating the 2-norm of a vector; i.e., it 
adds the sum of the squares of each component and then takes the square root of this 
number. 

**Input:**  a vector a doubles.

**Output:** a double representing the 2-norm of the vector. 

**Code Example:** In this example we calculate the 2-norm for the vectors [1,2,1], [1,1,1],
 and [-10, 3.1415926535, log(.145), 37, 101.11111].

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

double vector2Norm(vector <double> vect){
    double ans = 0;
    int size = vect.size();
    for(int i = 0; i < vect.size(); i++){
        ans+= pow(abs(vect[i]),2);
    }
    ans = sqrt(ans);
    
    return ans; 
}

int main(){
    
    vector <double> A = {1,2,1};
    vector <double> B = {1,1,1};
    vector <double> V = {-10, 3.1415926535, log(.145), 37, 101.11111};
    
    cout << vector2Norm(A) << endl;
    cout << vector2Norm(B) << endl;
    cout << vector2Norm(V) << endl;
    cout << endl;
    
}

```

**Results:** 
```C++

2.44949
1.73205
108.195

```


**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
