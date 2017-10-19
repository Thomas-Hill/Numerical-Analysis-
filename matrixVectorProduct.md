# Computational Mathematics Software Manual

## **Routine Name:** matrixVectorProduct

**Description:** This routine computes the product of a matrix and vector.  

**Input:**  It takes as arguements a vector of vectors of doubles and a vector of doubles

**Output:** It outputs a vector of doubles representing the matrix vector product. 

**Code Example:** In this example we compute the matrix product for 4 different sets of matrices
and vectors.

```C++
vector <double> matrixVectorProduct(vector<vector<double>> matrix, vector<double> vect){
    vector <double> ans; 
    for(int i = 0; i < vect.size(); i ++){
        ans.push_back(0); 
    }
  
    for(int i = 0; i < matrix.size(); i++){
        for(int j = 0; j < vect.size(); j++){
            ans[i] += matrix[i][j]*vect[j];
        }
        //cout << ans[i] << endl; 
    }
    
    return ans; 
}

int main(){

    vector <double> A = {1,2,1};
    vector <double> B = {1,1,1};
    vector <double> V = {-10, 3.1415926535, .145, 37};
    vector <double> W = {1,2,3,4};
    
    vector<vector<double>> C = {{-5,2,1},{1,2,-9},{0,-4,12}};
    vector<vector<double>> D = {{1,2,3},{1,2,3},{1,2,3}};
    vector<vector<double>> E = {{-1,10,32,100},{1000,2893,-1999,34},{147.33333,-2.718,3.14159,0}, {1,2,3,4}};
    vector<vector<double>> F = {{0,1,3,9},{-8,2,-19,3},{14734.333, 0,-3.14159,0},{1,2,3,4}};
    
    matrixVectorProduct(C,A);
    cout << endl; 
    matrixVectorProduct(D,B);
    cout << endl;
    matrixVectorProduct(E,V);
    cout << endl; 
    matrixVectorProduct(F,W);
    cout << endl; 
}
```

**Results:** 
```C++
0
-4
4

6
6
6

3746.06
56.7725
-1481.42
144.718

47
-49
14724.9
30

```

**Last Modification Date:** 10/4/17

**Author:** Thomas Hill
