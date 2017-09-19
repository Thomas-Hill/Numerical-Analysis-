# Computational Mathematics Software Manual

## **Routine Name:** fixPointIteration

**Description:** This routine represents a method for finding roots of a function.
It uses an auxilary function called function for evaluating functions at a given point.  

**Input:**  It takes as an argument a string specifying the function,
a double represting our intitial guess, a tolerance and a maximum number of iterations. 

**Output:** The routine outputs a double representing the approximate location of the root. 

**Code Example:** In this example we find the approximate values of the roots of x*exp(-x), and 3.0*x*sin(10.0*x)/100.

```C++
//This function takes a string and a double.  The string represents which function 
// we wish to evaluate and x tells at which point we will evaluate the function. 
double function(string f, double x){
    if(f == "f1"){
         return 3.0*x*sin(10.0*x)/100;
    }
    if(f == "f2"){
        return x*exp(-x);
    }
    //We can use this easy function to test how we are doing.  
    if(f == "f3"){
        return x - 1; 
    }
}


double fixPointIteration(string f, double x0, double tolerance, int maxIter){
    int counter = 1; 
    double g = x0 - function(f, x0);
    double error = abs(x0 - g); 
    //This while loop runs while our tolerance is below the error and we haven't exceeded the
    // maximum interations. 
    while (tolerance < error and counter < maxIter){
        //reassign x0 to g to proceed in our iteration and calculate the new g
        x0 = g; 
        g = x0 - function(f, x0);
        error = abs(x0 - g);
        counter ++; 
    }
    
    cout << "The root is approximately at: " << g << endl; 
    
}

int main(){
    \\ we call the function below to find the root of the function 3.0*x*sin(10.0*x)/100.
    fixPointIteration("f1", 1, .000001, 10000); 
    
    \\ we call the function below to find the root of the function given in part x*exp(-x). 
    fixPointIteration("f2", 1, .000001, 10000);
}

```

**Results:** 
```C++
The root is approximately at: 1.25664
The root is approximately at: 8.90987e-19
```

**Last Modification Date:** 9/19/17
**Author:** Thomas Hill
