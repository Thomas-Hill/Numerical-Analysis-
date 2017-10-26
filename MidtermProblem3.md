# Problem 3
Problem 2 at the end of Chapter 5. 

(a) Write a code for this Gauss–Jordan procedure using, e.g., the same format as for
the one appearing in Section 5.2 for Gaussian elimination. You may assume that no
pivoting (i.e., no row interchanging) is required.

(b) Show that the Gauss–Jordan method requires n^3 + O(n^2) floating point operations for
one right-hand-side vector b—roughly 50% more than what’s needed for Gaussian elimination.
## Solution: 

Below is the code with explanatory comments for part (a). We also output for each of the 
examples below the number of floating point operatations.  We see because we have a 
3-nested for loop showing up twice it will be true that the method requires n^3 operatations
plus and additional O(n^2) operatations to calculate the solutions. 
For each example we output the number of floating point operatations needed for Gaussian
elimation, the number required for Gauss-Jordan elimination, and the percentage more needed
for Gauss-Jordan elimination.  

```C++
#include <iostream>
#include <string>
#include <vector>
#include <cmath>

using namespace std;

//Below is the funtion for Gauss-Jordan Elimination.  It takes as arguments a vector
// of vector of doubles, which we use to represent a matrix, and a vector of doubles 
// representing the right-hand-side of the system.  The program outputs a vector of 
// doubles representing the solution to the system. 
vector <double> elim_GaussJordan(vector <vector <double>> matrix, vector <double> b){
    
    //We use this integer to count the number of FLOPS throughout the routine. 
    int countFLOPS = 0; 
    
    
    //Here we record the sizes of the rows and columns of the given matrix, as well as
    // the size of the vector b.  
    int n = matrix.size(); 
    int m = matrix[0].size();
    int p = b.size(); 
    
    //First we do regular Gaussian Elimination.  We keep track of the number of flops
    // up to this point.  
    //As suggested in the problem statement, we assume that no pivoting strategy 
    // is required here. 
    for (int k = 0; k < n; k++){
        for(int i = k+1; i < n; i++){    
            double l = matrix[i][k]/matrix[k][k]; 
            //cout << "L : " << l << endl; 
            countFLOPS++;
            for(int j = k+1; j < m; j++){
                matrix[i][j] = matrix[i][j] - l*matrix[k][j];
                countFLOPS+=2;
            }
            b[i] = b[i] - l*b[k];
            //cout << "b[i] : " << b[i] << endl; 
            countFLOPS += 2;
        }
    }
    
    cout << "Count of FLOPS for Gaussian Elimination: " << countFLOPS << endl;
    double countG = countFLOPS; 
    
    //Now we preform the remaining steps for Gauss-Jordan elimination. We increment
    // the count of FLOPS to represent the additional steps up to this point. 
    for(int k = n - 1; k > 0; k--){
        for(int i = k-1; i >= 0; i--){
            double l = matrix[i][k]/matrix[k][k];
            countFLOPS++;
            b[i] = b[i] - l*b[k]; 
            countFLOPS += 2; 
        }
    }
    
    //Here we output the number of FLOPs up to this point.  
    cout << "Count of FLOPS for Gauss-Jordan Elimination: " << countFLOPS << endl;
    double countGJ = countFLOPS; 
     
    //Finally we calculate the solutions (x_i's) and store these in a vector.  
    vector <double> solution; 
    for(int i = 0; i < p; i++){
        double s = b[i]/matrix[i][i];
        //cout << "b[i] : " << b[i] << endl; 
        //cout << s << endl; 
        solution.push_back(s);
    }
    
    double ratio = (countGJ/countG - 1)*100; 
    
    cout << endl << "The Gauss-Jordan method in this example requried " << ratio << "% more FLOPS than Guassian Elimination." <<  endl; 
    
    return solution; 
}


int main(){
    //Lets try this on a simple matrix and vector b. 
    vector <vector <double>> A = {{1,2},{3,4}}; 
    vector <double> b = {1,1}; 
    
    vector <double> ans = elim_GaussJordan(A, b); 
    
    cout << "\nThis is the solution vector: " << endl; 
    for (int i = 0; i < ans.size(); i++){
        cout << ans[i] << endl; 
    }
    
    cout << endl << endl; 
    
    //now lets try a more complicated example 
    vector <vector <double>> C = {{1,2,3,10},{-1,-3,-9,40}, {3.14159, 2.718, 0, -1}, {1, 2, 3, 9}}; 
    vector <double> b1 = {4, 7, 10, 37}; 
    
    vector <double> ans1 = elim_GaussJordan(C, b1);
    
    cout << "\nThis is the solution vector: " << endl; 
    for (int i = 0; i < ans1.size(); i++){
        cout << ans1[i] << endl; 
    }
    
    cout << endl << endl; 
    
    //finally lets try a bigger example
    vector <vector <double>> D; 
    vector <double> b2; 
    
    int M = 30; 
    D.resize(M);
    b2.resize(M);
    
    srand(time(NULL)); 
    
    for (int i = 0; i < M; i++){
        D[i].resize(M); 
        b2[i] = rand()%101;
        for(int j = 0; j < M; j++){
            D[i][j] = rand()%101; 
        }
    }
        
    vector <double> ans2 = elim_GaussJordan(D, b2);
    
    cout << "\nThis is the solution vector: " << endl; 
    for (int i = 0; i < ans2.size(); i++){
        cout << ans2[i] << endl; 
    }
    
}

```
## Results 

Because our code above involves some psuedo-randomness to fill the last matrix and vector, 
the output will be slightly different on the last example each time the code is run.  
However, the number of FLOPS will be the same.  

```C++
Count of FLOPS for Gaussian Elimination: 5
Count of FLOPS for Gauss-Jordan Elimination: 8

The Gauss-Jordan method in this example requried 60% more FLOPS than Guassian Elimination.

This is the solution vector: 
-1
1


Count of FLOPS for Gaussian Elimination: 46
Count of FLOPS for Gauss-Jordan Elimination: 64

The Gauss-Jordan method in this example requried 39.1304% more FLOPS than Guassian Elimination.

This is the solution vector: 
-1604.31
1845.87
-584.479
-33


Count of FLOPS for Gaussian Elimination: 18415
Count of FLOPS for Gauss-Jordan Elimination: 19720

The Gauss-Jordan method in this example requried 7.08661% more FLOPS than Guassian Elimination.

This is the solution vector: 
-0.573563
0.0554633
-0.227453
1.16467
0.121073
-0.0354403
1.17272
0.415871
0.863544
-0.539446
-0.463522
-1.20997
-0.800288
0.895868
-1.0592
-0.182302
-1.0329
-0.504099
0.11748
-0.655038
-0.343061
0.125039
1.3099
-0.952728
1.25012
0.794622
-0.405216
-0.459113
1.22658
0.895803

```

These results can be (and have been) verified in MAPLE.  
We see that the large the matrix gets, the smaller the ratio between Gaussian and 
Gauss-Jordan becomes.  



