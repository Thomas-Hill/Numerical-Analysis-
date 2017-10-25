# Problem 1
Problems 21 and 23 at the end of Chapter 3.  

Write a MATLAB program to minimize a smooth, scalar function in one variable φ(x) over a
given interval. The specifications are similar to those in Exercise 13. Your program should first
find and display all critical points, i.e., zeros of φ'(x). Then it should determine which of these
correspond to a local minimum by checking the sign of φ'', and display the local minimum
points. Finally, it should determine the global minimum point(s), value, and arguments by
minimizing over the local minimum values.

Use your program from Exercise 21 to minimize
(a) φ(x) = sin(x) and
(b) φ(x) = −sinc(x) = −sin(x)/x
over the interval `[-10,10]` with tol = 10e-8.  
## Solution: 
Below is the code for this example along with explanatory comments.  The required examples
given in problem 23 are tested in the main function.  

```C++
#include <iostream>
#include <string>
#include <vector>
#include <cmath>

using namespace std;

//this function is for evaluting a function at a particular value
double function(string f, double x){
    if(f == "f1"){
         return sin(x);
    }
    
    //we define -sin(x)/x piecewise, since in the limit as x approaches 0 the function value approaches -1.
    //we similarly do this for the derivatives of this function.  
    if(f == "f2"){
        if (abs(x) <=  1.1102e-16) return -1; //when the function value is near 0 we return -1 (the limit).
        else return -sin(x)/x;
    }
    
    if(f == "df1"){
        return cos(x); 
    }
    
    if(f == "df2"){
        if(abs(x) <=  1.1102e-16) return 0;//when the function value is near 0 we return 0 (the limit).
        else return -cos(x)/x + sin(x)/(x*x);
    }
    
    if(f == "ddf1"){
        return -sin(x);
    }
    
    if(f == "ddf2"){
        if(abs(x) <=  1.1102e-16) return 2.0/3.0; //when the function value is near 0 we return 2/3 (the limit).
        return sin(x)/x + 2*cos(x)/(x*x) - 2*sin(x)/(x*x*x);
    }
}

//below is the routine for the bisection method from previous homework assignments.  
//this code only iterates the bisetion method 3 times.  
 vector<double> bisectVECT(double a, double b, string f){
        
        //evaluate the specified function at the endpoints of the interval.  
        double fA = function(f, a);
        double fB = function(f, b);
        
        
       //if the product of the function values is positive then mean value theorem doesn't gaurantee a root, we fill the vector with error values of -4
        if(fA*fB > 0){
            vector<double> zero;
            zero.push_back(-4);
            return zero;
        }
        
        
        int n = 3;//number of times to iterate bisection
        
        double c = 0; //intialize the midpoint of interval, called c. 
        
        for(int i=0; i < n; i++){
            //in the following if blocks we check that the endpoints are non-zero.
            //if they are then we break the for loop and return those as our root values.  
            if(fA*fB == 0){
                if(fA == 0){
                    c = a;
                    vector<double> zero;
                    zero.push_back(0);
                    return zero ;
                }
                if(fB == 0){
                    c = b; 
                    vector<double> zero;
                    zero.push_back(0);
                    return zero;
                }
            }
            
            //Here we calculate the midpoint of the interval and find the function value at this point. 
            c = 0.5*(a + b);
            double fC = function(f,c);
            
            //if the product of function values at the right endpt and the midpt are negative we 
            // use this interval for the proceeding calculations, otherwise we use the right half of the interval.
            if(fA*fC < 0){
                b = c; 
                fB = fC;
            }
            else{
                a = c; 
                fA = fC;
            }
            
        }
        
        vector<double> ans; 
        ans.push_back(a);
        ans.push_back(b); 
        ans.push_back(c);
        return ans;  
        
 }
 
 //below is the code for the hybrid Newton and Bisection method, from a previous homework assignment.  
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
        if (ans.size() == 1) return -4; 
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
            if (ans.size() == 1) return -4; 
            a = ans[0];
            b = ans[1];
            x1 = ans[2];
            ct1++; 
        }
        
        //here we check that the derivative at the new sequence point is not within machine precision of zero.
        while(abs(function(deriv, x2)) < 1.1102e-16 and ct1 < maxIter){
            vector <double> ans = bisectVECT(a,b,f);
            if (ans.size() == 1) return -4; 
            a = ans[0];
            b = ans[1];
            x1 = ans[2];
            ct1++; 
        }
        
        //reassign value of the error and the new sequence point. 
        error = abs(x2 - x1);
        x1 = x2;
        
    }
    
    return x2; 
    
}

//Below is a routine that finds all the roots in an interval.  It takes as input two strings, representing 
// the function and its derivative.  Also it takes two doubles representing the endpoints of the interval,
// a double representing the tolerance, and an integer representing how many times we would like to subdivide 
// the given interval in search of roots.  We expect the user to input a sufficently small subdivision as 
// to gaurantee that this method finds all the roots in the specified interval.

vector <double> findAllRoots(string funct, string dfunct, double a, double b, double tol, int subdiv){
    double intLength = b-a; //calculate the length of the interval
    double subLengths= intLength/subdiv; //determine the length of the subdivisions
    
    //store the endpoints of each subdivided interval into vector 
    vector <double> endpts; 
    for (int i = 0; i < subdiv + 1; i++){
        double endpt = a + i*subLengths; 
        endpts.push_back(endpt);
    }
    
    //use the midpoint of each interval as an intial guess, for later use in Newton's method.  
    vector <double> initialGuess;
    for (int i = 0; i < subdiv; i++){
        double guess = (endpts[i] + endpts[i+1])/2.0;
        initialGuess.push_back(guess);
    }
    
    
    //create a vector to hold the roots of the function.  These roots are found by the hybrid method given above. 
    vector <double> roots; 
    for (int i = 0; i < subdiv; i += 1){
        double a = endpts[i];
        double c = initialGuess[i];
        double b = endpts[i+1];
        double fA = function(funct, a);
        double fB = function(funct, b);
        
        double root = hybrid_newton_bisect(funct, dfunct, a, b, c, tol, 10000);
        bool inlist = false; 
        for (int i = 0; i < roots.size(); i++){
                if(root == roots[i]){
                    inlist = true; 
                    break; 
                }
            }
        //if the hybrid method fails to find a root it returns -4, representing an error. 
        // We verify that -4 is not actually a root and store the appropriate values in the roots vector. 
        if(root!=-4 and !inlist){
            roots.push_back(root);
        }
        else {
            if(function(funct, -4) == 0) roots.push_back(root); 
            else;
        }
    }
    
    //return the vector representing the roots of the function. 
    return roots; 
}



//The routine below calculates the global minimum of a function.  It takes as arguments strings
// representing the function, and its first and second derivatives, as well as two doubles representing the 
// endpoints of the interval of interest, a double representing our desired tolerance, and an integer 
// which represents the number of intervals we are going to divide our interval into. The function returns 
// nothing, but prints the critcal points, the locations of local minima, and values and locations of global minima.  
void findMin(string funct, string dfunct, string ddfunct, double a, double b, double tol, int subdiv){
    
    //find and print the critcal points of the function
    vector <double> criticalPts; 
    criticalPts = findAllRoots(dfunct, ddfunct, a, b, tol, subdiv);
    cout << "These are the critical points of the function: "; 
    for (int i = 0; i < criticalPts.size(); i++){
        cout << criticalPts[i] << " "; 
    }
    cout << endl << endl; 
    
    
    //store the critical points which represent minima of the function into a vector called min. 
    vector <double> min;
    for (int i = 0; i < criticalPts.size(); i++){
        double s; 
        s = function(ddfunct,criticalPts[i]); 
        if (s > 0){
            min.push_back(criticalPts[i]); 
        }
    }
    //print the locations of the local minima.  
    cout << "The function has local minima at the points: "; 
    for (int i = 0; i < min.size(); i++){
        cout << min[i] << " "; 
    }
    cout << endl << endl; 
    
    //determine which points above are the global minima
    vector <double> globalMin; 
    double f; 
    globalMin.push_back(min[0]);
    for(int i = 1; i < min.size(); i++){
        f = function(funct, min[i]);
        
        if (f < function(funct, globalMin[i-1])){
            globalMin[i-1] = min[i]; 
        }
        
        else if(f == function(funct, globalMin[i-1])){
            globalMin.push_back(min[i]);
        }
        
    }
    
    //print the global minima and their location.  
    for (int i = 0; i < globalMin.size(); i++){
        cout << "The function has global minima of ";
        cout << function(funct,globalMin[i]) << " at the point x = " << globalMin[i] << ".\n";
    }
    cout << endl; 
    
    
}

int main(){
    
    // Below we find the critical points, locations of local minima, and value and location of global minima 
    //for the function sin(x), as requested in problem 23.
    cout << "Problem 23 (a) f(x) = sin(x), over the interval [-10,10] with tol = 10^-8. " << endl; 
    findMin("f1", "df1", "ddf1", -10, 10, 1e-8, 10); 
     
     cout << endl;
     
     
    // Below we find the critical points, locations of local minima, and value and location of global minima 
    //for the function -sin(x)/x, as requested in problem 23.  
    cout << "Problem 23 (b) f(x) = -sin(x)/x, over the interval [-10,10] with tol = 10^-8. " << endl; 
     findMin("f2", "df2", "ddf2", -10, 10, 1e-8, 9);
    
     cout << endl; 
}
```
## Results 

```C++
Problem 23 (a) f(x) = sin(x), over the interval [-10,10] with tol = 10^-8. 
These are the critical points of the function: -7.85398 -4.71239 4.71239 -1.5708 1.5708 7.85398 

The function has local minima at the points: -7.85398 4.71239 -1.5708 

The function has global minima of -1 at the point x = -7.85398.
The function has global minima of -1 at the point x = 4.71239.
The function has global minima of -1 at the point x = -1.5708.


Problem 23 (b) f(x) = -sin(x)/x, over the interval [-10,10] with tol = 10^-8. 
These are the critical points of the function: -7.72525 -4.49341 0 4.49341 7.72525 

The function has local minima at the points: -7.72525 0 7.72525 

The function has global minima of -1 at the point x = 0.

```

The results obtained above are easily verifiable either by hand, or in a computer algebra software package like MAPLE.  



