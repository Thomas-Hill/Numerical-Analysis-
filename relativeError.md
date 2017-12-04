# Computational Mathematics Software Manual

## **Routine Name:** relativeError 

**Description:** This routine returns the relative error between two numbers (doulbes).  

**Input:**  It takes as input two doulbes.

**Output:** The function returns the relative error between the two doubles. 
If the first is zero, the function returns and error. 

**Code Example:** 

```C++

#include <iostream>
#include <math.h>

using namespace std;

double relativeError(double actual, double approximation){
    if(actual == 0){
        cout << "Error division by zero. "; 
        return 0.0; 
    }
    return abs(actual-approximation)/actual;
}

int main(){

    cout << relativeError(0, .9) << endl;
    cout << relativeError (1.1, 1.0) << endl;
    cout << relativeError (5, 5.1) << endl;
}

```

**Results:**   The output of running the main function is the following:

```C++
Error division by zero. 0
0.1
0.1

```

**Last Modification Date:** 12/4/2017

**Author:** Thomas Hill
