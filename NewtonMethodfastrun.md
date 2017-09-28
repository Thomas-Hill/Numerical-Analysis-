# Computational Mathematics Software Manual

## **Routine Name:** newtonMethod

**Description:** This routine represents a method for finding roots of a function using Newton's method of approximation. 
this algorithm is known to converge quadratically for differentiable functions assuming
we choose a suitable intial guess. We also require that the functions derivative 
is non-zero at the location of the root we are interested in. To evalute functions we
use an auxilary routine called "function".  We require the user to know the derivative
and appropriately modify "function" for its implementation in the newtonMethod routine. 

**Input:**  It takes as an argument a string specifying the function, a string to specifiy 
the derivative of the function, a double representing an intitial guess, 
a tolerance and a maximum number of iterations. 

**Output:** The routine outputs a double representing the approximate location of the root, and an integer 
representing the number of iterations done.

**Code Example:** In this example we find the approximate values of the roots of x*exp(-x), and 3.0*x*sin(10.0*x).

```C++
//This is the newtonMethod function.  
double newtonMethod(string f, string deriv, double guess, double tol, int maxIter){
    double error = tol*10;//we start the error off large enough so that we enter the while loop. 
    int count = 0; 
    double ans = 0; 
    
    //this loop iterates Newtons method steps while we haven't exceeded our maximum number of iterations 
    // maximum number of iterations and our error is bigger than the specified tolerance. 
    while(count < maxIter and tol < error){
        ans = guess - function(f, guess)/function(deriv, guess);
        count++;
        error = abs(ans - guess);
        guess = ans;
    }
    cout << "Number of iterations for Newtons Method: " << count << endl; 
    cout << "This is approximately where the root of the function is: " << guess << endl; 
}

int main(){

    newtonMethod("f1", "df1", 1.5, .00001, 1000000);
    
    cout << endl; 
    
    newtonMethod("f2", "df2", .5, .00001, 1000000);
    
    cout << endl; 
   
}


```

**Results:** 
```C++

Number of iterations for Newtons Method: 4
This is approximately where the root of the function is: 1.5708

Number of iterations for Newtons Method: 6
This is approximately where the root of the function is: -9.38962e-14
```
These results agree with the true value of the roots we see from plotting the functions in a computer algebra software. 

**Last Modification Date:** 9/28/17

**Author:** Thomas Hill
