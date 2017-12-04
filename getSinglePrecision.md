# Computational Mathematics Software Manual

## **Routine Name:** getSinglePrecision
**Description:** This code represents a self-contained method that will compute the single precision machine epsilon.

**Input:**  None.

**Output:** We output a decimal number which roughly represents the machine epsilon.  
We also output an integer which represents largest n for which 1 and 1 –  1/2^n   are still distinguishable by the machine.

**Code Example:** 

```C++
\\ This is the function for calculating the single precision machine
epsilon. It takes no arguments and returns the single precision value for
machine precision.  

void getSinglePrecision(){
    		float x = 1.0;
    		float a; 
    		int n = 0; 
    	
\\ this while loop terminates when the machine can no longer distinguish the difference between x and a.
 
    		while(x - a !=0){ 
        		n++;

\\ “a” will be a value approaching 1 for large n 
        		a = 1+1/(pow(2,n));
    		}
	
\\ We output a decimal number which roughly represents the machine epsilon.  We also output an integer which represents largest n for which 1 and 1 – (1/(2^n)) are still distinguishable by the machine. 

    		cout << "Single Precision: " << 1/(pow(2,n)) << endl; 
    		cout << "n: " << n; 
    		return;
}

int main(){

getSinglePrecision(); 
}
```

**Results:** The output of running the main function is the following:  

```C++
Single Precision: 5.96046e-08
n: 24

```
This result indicates that for single precision the machine will keep track of roughly 8 digits in base 10.

**Last Modification Date:** 12/4/2017

**Author:** Thomas Hill
