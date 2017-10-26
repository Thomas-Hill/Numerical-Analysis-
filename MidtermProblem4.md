# Problem 4
Problems 17 and 18 at the end of Chapter 5.  

Write a MATLAB function that solves tridiagonal systems of equations of size n. Assume
that no pivoting is needed, but do not assume that the tridiagonal matrix A is symmetric. Your
program should expect as input four vectors of size n (or n − 1): one right-hand-side b and
the three nonzero diagonals of A. It should calculate and return x = A^−1 b using a Gaussian
elimination variant that requires O(n) flops and consumes no additional space as a function of
n (i.e., in total 5n storage locations are required).

Apply your program from Exercise 17 to the problem described in Example 4.17 using the
second set of boundary conditions, v(0) = v'(1) = 0, for g(t) = (π/2)^2 sin( π/2 t) and N = 100.
Compare the results to the vector u composed of u(ih) = sin(π/2 ih), i = 1,..., N, by recording |v−u|∞.

## Solution: 
```C++
#include <iostream>
#include <string>
#include <vector>
#include <cmath>

using namespace std;

vector <double> triDiagSystem(vector <double> subDiagonal, vector <double> mainDiagonal, vector <double> superDiagonal, vector <double> b){
    int n = mainDiagonal.size();
    int m = subDiagonal.size();
    //Clear the entries in the sub-diagonal position (we dont set them to zero, but we record the effects of this operation 
    // on the mainDiagonal and b).
    

    for(int i = 0; i < subDiagonal.size(); i++){
        double l = subDiagonal[i]/mainDiagonal[i];
        mainDiagonal[i+1] -= l*superDiagonal[i];
        b[i+1] -=  l*b[i];
    }
    
   

    //Clear the entries in the super-diagonal position (we dont sent them to zero, but record the effects of this operation 
    // on the the vector b).
    for(int i = mainDiagonal.size() - 1; i > 0; i--){
        double l = superDiagonal[i-1]/mainDiagonal[i];
        b[i-1] -= l*b[i];  
    }
    
    vector <double> solution; 
    double x; 
    for (int i = 0; i < mainDiagonal.size(); i++){
        x = b[i]/mainDiagonal[i]; 
        solution.push_back(x);
    }
    
    return solution; 
}

int main(){
    
    //In the lines below we construct the vectors corresponding
    // to the sub-diagonal, main diagonal, and super diagonal vectors
    // specified problem 17. 
    vector <double> mD;  
    vector <double> supD; 
    vector <double> subD; 
    vector <double> b; 
    
    for (int i = 1; i < 11; i++){
        mD.push_back(3*i);
        b.push_back(i);
    }
    
    for (int i = 1; i < 11; i++){
        supD.push_back(-i);
        subD.push_back(-i);
    }
    
    //Below we run the program on the specified matrix and print solution 
    // vectors components the x_i's. 
    vector <double> solution = triDiagSystem(subD, mD, supD, b); 
    for (int i = 0; i < solution.size(); i++){
        cout << "x" << i+1 << " = " << solution[i] << endl; 
    }
    
}
```
## Results 

```C++
x1 = 0.558733
x2 = 0.6762
x3 = 0.749233
x4 = 0.796898
x5 = 0.828771
x6 = 0.848794
x7 = 0.855739
x8 = 0.839679
x9 = 0.770265
x10 = 0.564413
```

The results obtained above are verifiable in a computer algebra software package like MAPLE.  

As required we see that because there are no nested for-loops in this routine, this 
Gaussian elimination variant only requires O(n) flops.  
