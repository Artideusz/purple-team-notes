# Templates
Templates are used when we want multiple types and amounts of arguments to be used in one function without rewriting an overloaded version of the function. The form is the following: `template <template-parameters> function-declaration`. Here's an example:

This code:

```C++
#include <stdio.h>

template <class T>
T sum(T a, T b) {
	T result;
	result = a + b;
	return result;
}

  

int main(int argc, char *argv[]) {
	double dba = 3.14;
	double dbb = 6.66;

	printf("Float: %f\n", sum<double>(dba, dbb));
	printf("Int: %d\n", sum<int>(22,47));
	return 0;
}
```

Is equivalent to:
```C++
#include <stdio.h>

// sum<int>(a, b)
int sum(int a, int b) {
	int result;
	result = a + b;
	return result;
}

// sum<double>(a, b)
double sum(double a, double b) {
    double result;
    result = a + b;
    return result;
}

int main(int argc, char *argv[]) {
	double dba = 3.14;
	double dbb = 6.66;

	printf("Float: %f\n", sum(dba, dbb));
	printf("Int: %d\n", sum(22,47));
	return 0;
}
```

## Resource
- [cplusplus](https://cplusplus.com/doc/tutorial/functions2/)