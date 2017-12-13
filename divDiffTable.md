# Computational Mathematics Software Manual

## **Routine Name:** divDiffTable

**Description:** This is a routine for computing the coefficients in the divided 
difference table from a given data set.  

**Input:**  The function takes as an arguement a dataset, input to the function as a pair
of vectors.  We assume that the user will input vectors of the same length.  

**Output:** The function outputs a vector holding the coefficients of the Newton form. 

**Code Example:** Below we do two examples.  Example one is from a dataset given in class.
We verify that we obtain the same results.  In the second example we input a longer and more
complicated dataset. 

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
    
    int k = 1; 
    for(int i = 2; i < n + 1; i++){
        for(int j = k; j < n; j++){
                table[j][i] = (table[j][i-1] - table[j-1][i-1])/(table[j][0] - table[j-k][0]);
        }
        k++; 
    }
    

    vector <double> solution(n,0); 
    
    for(int i = 0; i < n; i ++){
        solution[i] = table[i][i+1];
    }
    
    return solution;
}


int main(){
    vector <double> x = {1, 2, 4};
    vector <double> y = {1, 3, 3}; 
    
    vector <double> ans = divDiffTable(x,y);
    
    cout << "Example 1 of the Divided Difference Table: ";
    for(int i = 0; i < x.size(); i++){
        cout << ans[i] << " "; 
    }
    
    cout << "\n\n"; 
    
    vector <double> x1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13};
    vector <double> y1 = {-1.3, -1, 2, 3, -4, 7, 10, 3, 2, 90, 11, 12, 13}; 
    
    vector <double> ans1 = divDiffTable(x1,y1);
    
    cout << "Example 2 of the Divided Difference Table: ";
    for(int i = 0; i < x1.size(); i++){
        cout << ans1[i] << " "; 
    }
       
}
```

**Results:** Below are the results of running the main function.  We print the coefficients
calculated by the routine on examples 1 and 2. 

```C++

Example 1 of the Divided Difference Table: 1 2 -0.666667 

Example 2 of the Divided Difference Table: -1.3 0.3 0.6 -0.32963 0.0741609 -0.00302606 -0.00317732 
0.00119765 -0.000264736 4.42404e-05 -6.12768e-06 7.41995e-07 -8.14767e-08 

```

**Last Modification Date:** 11/30/2017

**Author:** Thomas Hill
