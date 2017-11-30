# Computational Mathematics Software Manual

## **Routine Name:** InterPoly

**Description:** This is a routine for evaluating the polynomial at a point not in the 
dataset.  

**Input:**  This function takes as an arguement the independent variables of a dataset
as well as a vector holding the coefficients from the divided difference table. 

**Output:** The function outputs a double representing the approximation of the function
by evaluating the interpolating polynomial. 

**Code Example:** Below we do two examples.  Example one is from a dataset given in class.
We verify that we obtain the same results.  In the second example we observe Runge's 
phenomenon.  We ask the number of times we would like to subdivide the interval  -1 to 1
and calculate the error between the true function value and the value given by evaluating 
the interpolating polynomial.  

```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <math.h>

using namespace std;

vector <double> divDiffTable(vector <double> x, vector <double> y){
    int n = x.size();
    vector <vector <double>> table(n , vector <double> (n + 1,0));
    
    for(int i = 0; i < n; i++){
        table[i][0] = x[i];
        table[i][1] = y[i]; 
    }
    
    for(int i = 2; i < n + 1; i++){
        for(int k = 1; k < n; k++){
            for(int j = k; j < n; j++){
                table[j][i] = (table[j][i-1] - table[j-1][i-1])/(table[j][0] - table[j-k][0]);
            }
        }
    }
    

    vector <double> solution(n,0); 
    
    for(int i = 0; i < n; i ++){
        solution[i] = table[i][i+1];
    }
    
    return solution;
}

double InterPoly(vector <double> x, vector <double> c, double a){
    
    int n = x.size();
    double temp; 
    double ans = 0.0; 
    for(int i = 0; i < n; i++){
        temp = c[i];
        for(int j = 0; j < i; j++){
            temp *= (a - x[j]); 
        }
        ans += temp; 
    }
    
    return ans; 
}

int main(){
    vector <double> x = {1, 2, 4};
    vector <double> y = {1, 3, 3}; 
    
    vector <double> ans = divDiffTable(x,y);
    
    cout << "Example 1 of the Divided Difference Table: ";
    for(int i = 0; i < x.size(); i++){
        cout << ans[i] << " "; 
    }
     
    cout << endl << InterPoly(x,ans,1.5);
    
    //here is our example for Runge's phenomena
    
    //ask the user how many subitervals and the value we want to evaluate the polynomial at
    int n; 
    double a;
    cout << endl << endl << "What is n?: "; 
    cin >> n; 
    cout << endl; 
    
    cout << "What is the value you want to evaluate at?: "; 
    cin >> a; 
    cout << endl; 
    
    //We create the dataset
    vector <double> X(n+1); 
    vector <double> Fx(n+1);
    for(int i = 0.0; i <= n; i++){
        X[i] = (2.0*i)/n - 1.0; 
        Fx[i] = 1/(1 + 25*(X[i])*(X[i]));
    }
    
    cout << endl; 
    
    //we calculate the divided difference table for the dataset
    vector <double> C = divDiffTable(X,Fx);
    
    double approx = InterPoly(X,C,a);
    
    cout << "Approximation by interpolating polynomial: " << approx << endl; 
    
    double trueval = 1/(1 + 25*a*a); 
    
    cout << "True value of the function: " << trueval << endl; 
    
    double error = abs(approx - trueval);
  
    
    cout << endl << "This is the error in our approximation using the interpolating polynomial: " << error << endl; 
    
    
}
```

**Results:** Below are the results of running the main function. The first number is 
the value of the interpolating polynomial for a dataset given in class. For
Runge's phenomenon we see that the value of the interpolating polynomial and 
the true value of the function are quite different.  The error for 200 subdivisions
is 8.55944e+17.  


```C++

2.16667

What is n?: 200

What is the value you want to evaluate at?: .25


Approximation by interpolating polynomial: 8.55944e+17
True value of the function: 0.390244

This is the error in our approximation using the interpolating polynomial: 8.55944e+17



```

**Last Modification Date:** 11/30/2017

**Author:** Thomas Hill
