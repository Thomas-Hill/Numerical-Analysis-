# Computational Mathematics Software Manual

## **Routine Name:** hybrid_secant_bisect

**Description:** This routine represents a hybrid between the secant and bisection
methods.  First we attempt the secant method.  If for some reason we divide by zero, 
or we see that the error between the steps in growing, we do 4 iterations of the 
bisection method. 

**Input:**  The function takes as arguments a string representing the function,
two doubles representing the endpoints of an interval containing the root,
two doubles represnting intial guesses for the location of the root, 
a double represting an acceptable tolerance level,
an integer representing the maximum number of iterations we wish to run.  

**Output:**  We output the number of iterations we ran, and the approximate location of
the root. 


**Code Example:** 

```C++
double hybrid_secant_bisect(string f, double endpt1, double endpt2, double guess1, double guess2, double tol, int maxIter){
    double error = tol*10;
    int count = 1; 
    double d1 = 0; 
    double d2 = 0; 
    double x1 = guess1; 
    double x2 = guess2; 
    double x3 = 0;
    double a = endpt1;
    double b = endpt1; 
    double approx = 0; 
   
    double ct1 = 0;
    
    
    //if the function is within machine epsilon of zero Newton's method fails, so we try bisection  
    while(abs(function(f, guess1) - function(f, guess2)) < 1.1102e-16 and ct1 < maxIter){
        vector <double> ans = bisectVECT(a,b,f);
        a = ans[0];
        b = ans[1];
        x1 = ans[2];
        ct1++; 
    }
    
    // we do one iteration out of the while loop just so during the while loop we can compare the error between each step.
    approx = (x2 - x1)/(function(f,x2)- function(f, x1));
    x3 = x2 - function(f, x2)*approx;
    error = abs(x3 - x2); 
    x1 = x2;
    x2 = x3;
    
    //this while loop iterates Newton's method. 
    while(count < maxIter and tol < error){
        x3 = x2 - function(f, x2)*approx;
        count++;
        
        //here we check that the error is decreasing at each step.
        ct1 = 0; 
        while(error < abs(x2- x1) and ct1 < maxIter){
            vector <double> ans = bisectVECT(a,b,f);
            a = ans[0];
            b = ans[1];
            x1 = ans[2];
            ct1++; 
        }
        
        //here we check that the derivative at the new sequence point is not within machine precision of zero.
        while(abs(function(f, guess1) - function(f, guess2)) < 1.1102e-16 and ct1 < maxIter){
            vector <double> ans = bisectVECT(a,b,f);
            a = ans[0];
            b = ans[1];
            x1 = ans[2];
            ct1++; 
        }
        
        //reassign value of the error and the new sequence point.
        
        error = abs(x2 - x1);
        x1 = x2;
        x2 = x3; 
        
    }
    
    cout << "Number of iterations for Hybrid Secant/Bisection Method: " << count << endl; 
    cout << "This is approximately where the root of the function is: " << x3 << endl; 

    
}

int main(){
 
    hybrid_secant_bisect("f1", 1.3, 1.8, 1.5, 1.6, 0.0001, 10000);
    
    cout << endl<< endl; 
    
    hybrid_secant_bisect("f2", -10, 11, .1, -.1, 0.0001, 1000);
    
    cout << endl << endl;
    
}

```

**Results:** 
```C++
Number of iterations for Hybrid Secant/Bisection Method: 5
This is approximately where the root of the function is: 1.5708


Number of iterations for Hybrid Secant/Bisection Method: 5
This is approximately where the root of the function is: 1.8811e-11

```
These agree with the results we see from ploting the function in a computer algebra 
software. 

**Last Modification Date:** 10/1/17

**Author:** Thomas Hill
