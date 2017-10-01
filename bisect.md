# Computational Mathematics Software Manual

## **Routine Name:** bisect

**Description:** This function uses the bisection method to approximate the roots of a given function.  

**Input:**  This function takes as arguments 2 doubles representing the endpoints of an interval, a string f which should represent some function stored in the function program, a double tol which represents the acceptable tolerance, and an int representing the maximum number of iterations.

This function requires a supplementary routine specifying which function you wish to evaluate.  This function takes a string and a point x where we will evaluate the function.
 

**Output:** We output the maximum error and the approximate location a root (if there is guaranteed one by IVT).  

**Code Example:** In this example we find the approximate values of the roots of x*exp(-x), and 3.0*x*sin(10.0*x)/100.

```C++
double function(string f, double x){
    if(f == "f1"){
         return 3.0*x*sin(10.0*x);
    }
    if(f == "f2"){
        return x*exp(-x);
    }
    return 0.0; 
}

 double bisect(double a, double b, string f, double tol, int maxIter){
        double error = 10.0*tol;
       
        double fA = function(f, a);
        double fB = function(f, b);
        
        
            if(fA*fB > 0){
            cout << "Error. The intermediate value theorem doesn't guarantee a root on this interval. " << endl; 
            return 0;
        }
        
        //the number of iterations needed to be within our specified tolerance level. 
        int n = log((b-a)/tol)/log(2) + 1; 
        
        double c = 0; //initialize the midpoint of interval, called c. 
        
        for(int i=0; i < n and i < maxIter; i++){
            //in the following if blocks we check that the endpoints are non-zero.
                if(fA*fB == 0){
                if(fA == 0){
                    c = a;
                    error = 0;
                    cout << "This is the error: " << error << endl; 
                  cout << "This is where the root of the function is: " << c << endl; 
                    return c;
                }
                if(fB == 0){
                   c = b; 
                   error = 0;
                   cout << "This is the error: " << error << endl; 
                   cout << "This is where the root of the function is: " << c << endl; 
                    return c;
                }
            }
            //Here we calculate the midpoint of the interval
            c = 0.5*(a + b);
            double fC = function(f,c);
            
            if(fA*fC < 0){
                b = c; 
                fB = fC;
            }
            else{
                a = c; 
                fA = fC;
            }
        }
        
     error = b-a;
     cout << "This is the error: " << error << endl; 
     cout << "This is approximately where the root of the function is: " << c << endl; 
     return c; 
     }

int main(){
   
   cout << endl << "-----------For the function f(x) = 3.0*x*sin(10.0*x) on 0 to 7-----------" << endl;
   
   bisect(0, 7, "f1", 0.00001, 1000); //the root is at the left endpoint of the specified interval. 
   
   cout << endl << "-----------For the function f(x) = x*exp(-1) on -infinity to infinity-----------" << endl;
   
   bisect(-1000,1001,"f2", 0.000001, 100000); // here we choose a large but non-symmetric interval.  A symmetric interval will find the root as it is the midpoint. 
    
}


```

**Results:** 
```C++
-----------For the function f(x) = 3.0*x*sin(10.0*x) on 0 to 7-----------
This is the error: 0
This is where the root of the function is: 0

-----------For the function f(x) = x*exp(-1) on -infinity to infinity-----------
This is the error: 9.31788e-07
This is approximately where the root of the function is: -3.6275e-07

```

**Last Modification Date:** 10/1/17

**Author:** Thomas Hill
