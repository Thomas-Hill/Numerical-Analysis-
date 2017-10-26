# Problem 4
Problems 17 and 18 at the end of Chapter 5.  

17. Write a MATLAB function that solves tridiagonal systems of equations of size n. Assume
that no pivoting is needed, but do not assume that the tridiagonal matrix A is symmetric. Your
program should expect as input four vectors of size n (or n − 1): one right-hand-side b and
the three nonzero diagonals of A. It should calculate and return x = A^−1 b using a Gaussian
elimination variant that requires O(n) flops and consumes no additional space as a function of
n (i.e., in total 5n storage locations are required).

18. Apply your program from Exercise 17 to the problem described in Example 4.17 using the
second set of boundary conditions, v(0) = v'(1) = 0, for g(t) = (π/2)^2 sin( π/2 t) and N = 100.
Compare the results to the vector u composed of u(ih) = sin(π/2 ih), i = 1,..., N, by recording |v−u|∞.

## Solution: 
Below is the code required with explanatory comments for both the example in 17 and in 18.  

```C++
#include <iostream>
#include <string>
#include <vector>
#include <cmath>

using namespace std;

//Below is the code used to calculate the vector infintiy norm.  
//This was written in Homework 5.  It will be used to answer problem 18.
double maxEntry(vector <double> vect){
    double ans = max(abs(vect[0]), abs(vect[1])); 
    for(int i = 2; i < vect.size(); i++){
        ans = max(ans, abs(vect[i]));
    }
    return ans;
}

double vectorInfNorm(vector <double> vect){
    double ans = 0; 
    int size = vect.size();
    ans = maxEntry(vect);
    return ans; 
    
}

double vectorErrInf(vector <double> vect1, vector <double> vect2){
    vector <double> ans; 
    for(int i = 0; i < vect1.size(); i++){
        ans.push_back(vect1[i] - vect2[i]);
    }
    return vectorInfNorm(ans);
}

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
    cout << "This is the solution to the problem specfied in problem 17. " << endl; 
    vector <double> solution = triDiagSystem(subD, mD, supD, b); 
    for (int i = 0; i < solution.size(); i++){
        cout << "x" << i+1 << " = " << solution[i] << endl; 
    }
    
    cout << endl << endl; 
    
    //Now lets do the problem specified in problem 18.  
    //Lets create the vector represting super, sub and main diagonal.
    vector <double> super; 
    vector <double> sub; 
    vector <double> mainD; 
    
    for(int i = 0; i < 99; i++){
        super.push_back(-1);
        sub.push_back(-1);
        mainD.push_back(2); 
    }
    
    mainD.push_back(2); //push one more 2 on to the main-diagonal. 
    
    
    //Now lets create the vector g.  We will use 3.141592653589793 as an approximation for pi.  
    
    double pi = 3.141592653589793; 
    
    vector <double> g; 
    
    for (int i = 1; i <= 100; i++){
        double x = (pi*pi/4)*(sin(pi*i*0.01/2));
        g.push_back(x); 
    }
    
    //Now we run the triDiagSystem solver and shove the answer into the vector v.
    vector <double> v; 
    v = triDiagSystem(sub, mainD, super, g);
    
    cout<< "This is the solution v of Av = g given in problem 18. " << endl; 
    for (int i = 0; i < v.size(); i++){
        cout << "v" << i+1 << " = " << v[i] << endl; 
    }
    
    //Below we create the vector u
    
    vector <double> u; 
    
    for(int i = 1; i <= 100; i++){
        double x = sin(pi*0.02*i/2); 
        u.push_back(x);
    }
    
    //Now we compare the results of u and v by calculating |u-v|_infinity. 
    cout << "\n\nThis is |u-v|_infinity: " << vectorErrInf(u,v) << endl;  
    
    
}
```
## Results 

```C++
This is the solution to the problem specfied in problem 17. 
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


This is the solution v of Av = g given in problem 18. 
v1 = 58.0767
v2 = 116.115
v3 = 174.075
v4 = 231.919
v5 = 289.608
v6 = 347.104
v7 = 404.368
v8 = 461.36
v9 = 518.044
v10 = 574.38
v11 = 630.329
v12 = 685.855
v13 = 740.918
v14 = 795.481
v15 = 849.506
v16 = 902.954
v17 = 955.789
v18 = 1007.97
v19 = 1059.47
v20 = 1110.24
v21 = 1160.25
v22 = 1209.45
v23 = 1257.83
v24 = 1305.33
v25 = 1351.92
v26 = 1397.57
v27 = 1442.24
v28 = 1485.89
v29 = 1528.49
v30 = 1570.01
v31 = 1610.4
v32 = 1649.64
v33 = 1687.7
v34 = 1724.53
v35 = 1760.1
v36 = 1794.39
v37 = 1827.35
v38 = 1858.96
v39 = 1889.18
v40 = 1917.98
v41 = 1945.34
v42 = 1971.21
v43 = 1995.57
v44 = 2018.38
v45 = 2039.63
v46 = 2059.27
v47 = 2077.28
v48 = 2093.63
v49 = 2108.28
v50 = 2121.23
v51 = 2132.42
v52 = 2141.85
v53 = 2149.48
v54 = 2155.28
v55 = 2159.23
v56 = 2161.31
v57 = 2161.48
v58 = 2159.73
v59 = 2156.03
v60 = 2150.35
v61 = 2142.68
v62 = 2132.99
v63 = 2121.26
v64 = 2107.47
v65 = 2091.6
v66 = 2073.62
v67 = 2053.51
v68 = 2031.27
v69 = 2006.86
v70 = 1980.27
v71 = 1951.48
v72 = 1920.48
v73 = 1887.24
v74 = 1851.76
v75 = 1814.01
v76 = 1773.98
v77 = 1731.65
v78 = 1687.02
v79 = 1640.07
v80 = 1590.78
v81 = 1539.15
v82 = 1485.16
v83 = 1428.8
v84 = 1370.05
v85 = 1308.92
v86 = 1245.39
v87 = 1179.45
v88 = 1111.1
v89 = 1040.32
v90 = 967.112
v91 = 891.465
v92 = 813.377
v93 = 732.84
v94 = 649.85
v95 = 564.405
v96 = 476.499
v97 = 386.131
v98 = 293.298
v99 = 197.999
v100 = 100.233


This is |u-v|_infinity: 2160.5
```

These results have been verified in MAPLE, so we can be confident that the routine is 
working as intended. 

As required we see that because there are no nested for-loops in this routine, this 
Gaussian elimination variant only requires O(n) flops.  
