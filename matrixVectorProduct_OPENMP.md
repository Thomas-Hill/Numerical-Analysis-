# Computational Mathematics Software Manual

## **Routine Name:** matrixVectorProduct

**Description:** This routine computes the product of a matrix and vector. In the example
below we use openmp to parallelize the operation.  

**Input:**  It takes as arguements a vector of vectors of doubles and a vector of doubles

**Output:** It outputs a vector of doubles representing the matrix vector product. 

**Code Example:** In this example we generate matrices of size 1, 1001, 2001, ..., 19001.
We output the time it takes to compute the operation and the size of the matrix.  
(In the homework assignment this is relationship is plotted).  

```C++
vector <double> matrixVectorProduct(vector<vector<double>> matrix, vector<double> vect){
    vector <double> ans; 
    for(int i = 0; i < vect.size(); i ++){
        ans.push_back(0); 
    }
    
    #pragma omp parallel for  
    for(int i = 0; i < matrix.size(); i++){
        for(int j = 0; j < vect.size(); j++){
            ans[i] += matrix[i][j]*vect[j];
        }
        //cout << ans[i] << endl; 
    }
    
    return ans; 
}

int main(){

 for (int M = 1; M < 10000; M +=1000){
        A.resize(M);
        v.resize(M);
        for (int i = 0; i < M; i++){
            A[i].resize(M); 
            v[i] = rand()%101;
            for(int j = 0; j < M; j++){
                A[i][j] = rand()%101; 
            }
        }
        
        high_resolution_clock::time_point t1 = high_resolution_clock::now();
        matrixVectorProduct(A,v);
        high_resolution_clock::time_point t2 = high_resolution_clock::now();
        duration<double> time_span = duration_cast<duration<double>>(t2 - t1);
        
        cout << time_span.count() << " " << M << endl; 
    }
    
}

```

**Results:**  The code below is the result of running the operation using open mp on 8 threads. 
```C++
3.37e-05 1
0.0194125 1001
0.0707405 2001
0.149104 3001
0.290569 4001
0.452626 5001
0.610342 6001
0.754378 7001
1.05209 8001
1.46462 9001
1.83825 10001
2.18885 11001
5.19888 12001
8.61841 13001
23.7013 14001
49.458 15001
66.6863 16001
92.7493 17001
123.104 18001
146.041 19001


```

**Last Modification Date:** 10/18/17

**Author:** Thomas Hill
