# Computational Mathematics Software Manual

## **Routine Name:** decomLU_scaledPP

**Description:** This routine calculates the LU decompsition of a square matrix 
using scaled partial pivoting.  

**Input:**  It takes as an input the matrix we wish to decompose.  

**Output:** It outputs a matrix with L and U superimposed on eachother.  We expect that 
the user is competent enough to understand how to reference the indices in an appropriate 
way to use this single matrix to represent both matrices seperately.  We also output the 
vector representing the permutation of the rows of the given matrix.   

**Code Example:** In this example we compute the LU factorization for 1 matrix.  
The decomposition has been verified in Maple.  We then compute the decomposition for 3 
matrices, a 5x5, 6x6 and 7x7, filled with random integers from 0 to 100.  


```C++
#include <iostream>
#include <vector>
#include <string>
#include <cmath>

using namespace std; 



double maxEntry(vector <double> vect){
    double ans = max(abs(vect[0]), abs(vect[1])); 
    for(int i = 2; i < vect.size(); i++){
        ans = max(ans, abs(vect[i]));
    }
    return ans;
}


void printMatrix(vector<vector<double>> a){
    for (int i=0; i<a.size(); i++){
        for (int j=0;j<a[i].size();j++){
            cout << a[i][j] << " ";
        }
        cout << endl;
    }
}


vector < vector < double > > decompLU_scaledPP(vector<vector<double>> A){
    
    vector <int> map; 
    int n = A.size(); 
    
    vector <double> s; 
    for (int i = 0; i < n; i++){
        s.push_back(maxEntry(A[i]));
    }
    
    for (int i = 0; i < n; i++){
       map.push_back(i); 
    }

    for (int k = 0; k < n; k++){
        int maxRow = k; 
        double maxVal = abs(A[k][k]/s[k]);
        for(int i = k+1; i < n; i++){
            double val = abs(A[i][k]/s[i]);
            if(val > maxVal){
                maxVal = val; 
                maxRow = i;
            }
        }
        swap(A[maxRow], A[k]);
        swap(map[maxRow], map[k]);
        
        
        for(int i = k+1; i < n; i++){
            double l = A[i][k]/A[k][k];
                for (int j = k+1; j< n; j++){
                    A[i][j] = A[i][j] - A[k][j]*l; 
                }
            A[i][k] = l;
        }
        
    }
    
    printMatrix(A);
    cout << endl; 
    
    cout << "this is the vector representing the row swaps made: " << endl; 
    for (int i = 0; i < n; i++){
        cout << map[i] + 1 << " ";
    }
    cout << endl << endl; 
    return A; 
    
}



int main(){
    vector < vector < double > > E; 
    E = {{0,1,1,-3},{-2,3,1,4},{0,0,0,1},{3,1,0,0}};
    
    decompLU_scaledPP(E);
    
    vector <vector <double>> A; 
    srand(time(NULL)); 
    
    for (int M = 5; M < 8; M ++){
        A.resize(M);
        for (int i = 0; i < M; i++){
            A[i].resize(M); 
            for(int j = 0; j < M; j++){
                A[i][j] = rand()%101; 
            }
        }
    decompLU_scaledPP(A);
    }
    
}

```

**Results:**   
```C++

3 1 0 0 
-0.666667 3.66667 1 4 
0 0.272727 0.727273 -4.09091 
0 0 0 1 

this is the vector representing the row swaps made: 
4 2 1 3 

88 9 82 84 63 
0.454545 72.9091 47.7273 -11.1818 56.3636 
0.329545 0.274782 -22.1373 5.39074 -5.24906 
0.193182 0.168173 -0.50289 44.3642 -23.289 
0.409091 0.49813 -0.617969 0.440393 -7.83656 

this is the vector representing the row swaps made: 
1 2 4 5 3 

93 87 53 88 9 37 
0.258065 36.5484 68.3226 35.2903 81.6774 17.4516 
0.774194 0.154457 -43.5852 -51.5799 56.4166 16.6593 
0.182796 0.659312 -0.00610209 58.3319 20.8482 -8.16783 
1.06452 -0.728155 -0.649994 -0.934432 129.045 62.5166 
0.483871 0.462489 -0.407395 0.258599 0.499538 32.6953 

this is the vector representing the row swaps made: 
4 1 3 5 6 2 

75 66 41 7 3 9 62 
0.44 -27.04 -17.04 5.92 37.68 14.04 21.72 
0.666667 -0.554734 38.214 47.6174 45.9024 77.7885 -5.28452 
1.25333 1.39497 0.768923 46.3544 -45.6179 -23.6787 -23.942 
0.893333 -0.482249 0.63212 0.463855 54.6354 43.5427 16.5339 
0.8 -1.11686 1.02498 0.479026 1.1254 -86.911 44.9365 
0.533333 0.0443787 0.598983 -0.399489 -0.365894 -0.615061 54.2585 

this is the vector representing the row swaps made: 
6 3 7 5 2 4 1 


```

**Last Modification Date:** 10/18/17

**Author:** Thomas Hill
