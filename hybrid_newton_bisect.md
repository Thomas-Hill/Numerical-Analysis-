# Computational Mathematics Software Manual

## **Routine Name:** hybrid_newton_bisect

**Description:** This routine represents a hybrid of Newton's method and the bisection 
method.  It intially runs one iteration of Newton's method.  If for some reason this fails,
either because the derivative is too close to zero, or because the sequence of Newton 
method approximations doesn't stay within an a neighborhood of the root of interest, 
we run 4 iterations of the bisection method, and then attempt Newtons method again.  
This process continues until we are within the given tolerance of the root. 


**Input:**  This function takes as arguments a string representing the function, 
a string representing the functions derivative, two doubles representing the left
and right endpoint of an interval containing the root, a double representing an 
intial guess of the roots location, a double representing tolerance, and an integer 
representing the maximum number of iterations we wish to run.  

**Output:** The routine outputs a double representing the approximate location of the root. 

**Code Example:** In this example we find the approximate values of the roots of x*exp(-x), and 3.0*x*sin(10.0*x)/100.

```C++
double function(string f, double x){
    if(f == "f1"){
         return 3.0*x*sin(10.0*x);
    }
    if(f == "f2"){
        return x*exp(-x);
    }
    
    if(f == "f3"){
        return x*cos(10*x); 
    }
    
    if(f == "df1"){
        return 3*sin(10*x) + 30*x*cos(10*x);
    }
    
    if(f == "df2"){
        return exp(-x)-x*exp(-x);
    }
    
    if(f == "df3"){
        return cos(10*x) - 10*x*cos(10*x); 
    }
}

double hybrid_newton_bisect(string f, string deriv, double endpt1, double endpt2, double guess, double tol, int maxIter){
    double error = tol*10;
    int count = 1; 
    double d1 = 0; 
    double d2 = 0; 
    double x1 = guess; 
    double x2 = 0; 
    double a = endpt1;
    double b = endpt2; 
    double ct1 = 0; 
    
    
    //if the function is within machine epsilon of zero Newton's method fails, so we try bisection  
    while(abs(function(deriv, x1)) < 1.1102e-16 and ct1 < maxIter){
        vector <double> ans = bisectVECT(a,b,f);
        if (ans.size() == 1) return 0; 
        a = ans[0];
        b = ans[1];
        x1 = ans[2];
        ct1++; 
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
        ct1 = 0; 
        while(error < abs(x2- x1) and ct1 < maxIter){
            vector <double> ans = bisectVECT(a,b,f);
            if (ans.size() == 1) return 0; 
            a = ans[0];
            b = ans[1];
            x1 = ans[2];
            ct1++; 
        }
        
        //here we check that the derivative at the new sequence point is not within machine precision of zero.
        while(abs(function(deriv, x2)) < 1.1102e-16 and ct1 < maxIter){
            vector <double> ans = bisectVECT(a,b,f);
            if (ans.size() == 1) return 0; 
            a = ans[0];
            b = ans[1];
            x1 = ans[2];
            ct1++; 
        }
        
        //reassign value of the error and the new sequence point. 
        error = abs(x2 - x1);
        x1 = x2;
        
    }
    
    cout << "Number of iterations for Hybrid Method: " << count << endl; 
    cout << "This is approximately where the root of the function is: " << x2 << endl; 

    
}

 
 int main(){
 
    cout << endl; 

 
    hybrid_newton_bisect("f1", "df1", 1.3, 1.8, 1.5, 0.0001, 1000);
    
    cout << endl<< endl; 
    
    hybrid_newton_bisect("f2", "df2", -10, 11, 1, 0.0001, 1000);

 }

```

**Results:** 
```C++
Number of iterations for Hybrid Method: 3
This is approximately where the root of the function is: 1.5708


Number of iterations for Hybrid Method: 5
This is approximately where the root of the function is: -5.4215e-09
```

**Last Modification Date:** 9/19/17

**Author:** Thomas Hill
