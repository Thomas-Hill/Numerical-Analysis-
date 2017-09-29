# Computational Mathematics Software Manual

## **Routine Name:** newtonMethod_withChecks

**Description:** This routine represents a method for finding roots of a function using Newton's method of approximation. 
this algorithm is known to converge quadratically for differentiable functions assuming
we choose a suitable intial guess. We also require that the functions derivative 
is non-zero at the location of the root we are interested in. To evalute functions we
use an auxilary routine called "function".  We require the user to know the derivative
and appropriately modify "function" for its implementation in the newtonMethod routine. 
Unlike the newtonMethod routine, this routine verifies that the point is sufficiently close
for convergence within some neighborhood of the intial guess, and makes sure that the 
derivative is non-zero for all sequence points. 

**Input:**  It takes as an argument a string specifying the function, a string to specifiy 
the derivative of the function, a double representing an intitial guess, 
a tolerance and a maximum number of iterations. 

**Output:** The routine outputs a double representing the approximate location of the root, and an integer 
representing the number of iterations done.  If the point for some reason does not gaurantee 
convergence an informative error message is printed to the console.  

**Code Example:** In this example we find the approximate values of the roots of x*exp(-x), 3.0*x*sin(10.0*x), and x*cos(10*x). 
```C++

//this is the code for Newtons Method which checks that the function we are trying to find the root of and the intial point have 
// the properties required for convergence. 
double newtonMethod_withChecks(string f, string deriv, double guess, double tol, int maxIter){
    double error = tol*10;
    int count = 1; 
    double d1 = 0; 
    double d2 = 0; 
    double x1 = guess; 
    double x2 = 0; 
    
    //if the function is within machine epsilon of zero Newton's method fails. 
    if(abs(function(deriv, guess)) < 1.1102e-16){
        cout << "Error, at you're initial guess the function has a zero derivative.  Choose a value closer to the root." << endl; 
        return 0; 
    }
    
    // we do one iteration out of the while loop just so during the while loop we can compare the error between each step.
    x2 = x1 - function(f, x1)/function(deriv, x1);
    error = abs(x2 - x1); 
    x1 = x2; 
    
    //this while loop iterates Newton's method. 
    while(count < maxIter and tol < error){
        x2 = x1 - function(f, x1)/function(deriv, x1);
        count++;
        
        //here we check that the error is decreasing at each step.  
        if (error < abs(x2- x1)){
            cout << "Error, choose an intial guess closer to the root you are interested in. The sequence is not contained in a sufficiently small interval around the root. " << endl;
            return 0; 
        }
        
        //here we check that the derivative at the new sequence point is not within machine precision of zero.
        if(abs(function(deriv, x2)) < 1.1102e-16){  
            cout << "Error, choose a value closer to the root the derivative is zero at one of the points in the sequence." << endl; 
            return 0; 
        }
        
        //reassign value of the error and the new sequence point. 
        error = abs(x2 - x1);
        x1 = x2;
    }
    
    cout << "Number of iterations for Newtons Method: " << count << endl; 
    cout << "This is approximately where the root of the function is: " << x2 << endl; 

    
}
int main(){

    cout << endl << "For f1: " << endl; 
    
    newtonMethod_withChecks("f1", "df1", 1.5, .00001, 1000000);
    
    cout << endl << "For f2: " << endl; 
    
    newtonMethod_withChecks("f2", "df2", .5, .00001, 1000000);
    
    cout << endl << "For f3: " << endl; 
    
    newtonMethod_withChecks("f3", "df3", .01, 0.00001, 1000000);
    
    cout << endl; 
   
}


```

**Results:** 
```C++

For f1: 
Number of iterations for Newtons Method: 4
This is approximately where the root of the function is: 1.5708

For f2: 
Number of iterations for Newtons Method: 6
This is approximately where the root of the function is: -9.38962e-14

For f3: 
Number of iterations for Newtons Method: 4
This is approximately where the root of the function is: -2.22207e-17
```
These results agree with the true value of the roots we see from plotting the functions in a computer algebra software. 

**Last Modification Date:** 9/28/17

**Author:** Thomas Hill
