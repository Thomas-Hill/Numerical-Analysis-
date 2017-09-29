# Computational Mathematics Software Manual

## **Routine Name:** secantMethod

**Description:** This routine represents a method for finding roots of a function using 
a variant of Newton's method, the secant method.  
This algorithm is known to converge superlinearly, assuming
we choose a suitable intial guess. We do not require any conditions of differentiablity
of the function.  To evalute functions we use an auxilary routine called "function".  
We require the user to know the derivative and appropriately modify "function" for its 
implementation in the secantMethod routine.  

**Input:**  It takes as an argument a string specifying the function, two doubles representing intitial guesses, 
a double representing tolerance and a integer representing maximum number of iterations. 

**Output:** The routine outputs a double representing the approximate location of the root, and an integer 
representing the number of iterations done.  If the point for some reason does not gaurantee 
convergence an informative error message is printed to the console.  

**Code Example:** In this example we find the approximate values of the roots of x*exp(-x), 3.0*x*sin(10.0*x), and x*cos(10*x). 
```C++
//This is the secant method function.  Here we do not include all the checks ensuring convergence 
// that we included for Newton's method.  We assume that the user is capable of choosing appropriate 
// initial guesses.  
double secantMethod(string f, double guess1, double guess2, double tol, int maxIter){
    double error = tol*10;
    int count = 0; 
    double ans = 0; 
    
    //this while loop iterates the steps of the secant method 
    while(count < maxIter and tol < error){
        //calculate the next sequence point, and error 
        ans = guess2 - function(f, guess2)*(guess2 - guess1)/(function(f, guess2)- function(f, guess1));
        count++;
        error = abs(guess2 - guess1);
        
        //reassign new variable names. 
        guess1 = guess2;
        guess2 = ans; 
    }
    cout << "Number of iterations for Secant Method: " << count << endl; 
    cout << "This is approximately where the root of the function is: " << ans << endl; 
}

int main(){
    
    cout << endl << "For f1: " << endl; 
    
    secantMethod("f1", 1.5, 1.7, .00001, 1000000);
    
    cout << endl << "For f2: " << endl; 
    
    secantMethod("f2", .1, .5, .00001, 1000000);
    
    cout << endl << "For f3: " << endl; 
    
    secantMethod("f3", .1, .01, 0.00001, 1000000);
}

```

**Results:** 
```C++
For f1: 
Number of iterations for Secant Method: 5
This is approximately where the root of the function is: 1.5708

For f2: 
Number of iterations for Secant Method: 7
This is approximately where the root of the function is: -2.03968e-17

For f3: 
Number of iterations for Secant Method: 4
This is approximately where the root of the function is: -1.15543e-18

```
These results agree with the true value of the roots we see from plotting the functions in a computer algebra software. 

**Last Modification Date:** 9/28/17

**Author:** Thomas Hill
