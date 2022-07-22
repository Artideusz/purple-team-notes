# Function overloading
Function overloading allows the developer to use the same name of a method, but do different stuff depending on the amount and/or types of arguments provided to the function or method. Here's an example:

```C++

int operate(int a, int b) {
	return a + b;
}

int operate(int a) {
	return a * 100;
}

int operate(int a, int b, int c) {
	return a + b / c;
}

double operate(double a, double b) {
	return a / b;
}


int a = 2;
double b = 3.14;
int c = 10;

operate(c);        // result is 10 * 100 = 1000
operate(a, c);     // result is 2 + 10 = 12
operate(a, 20, c); // result is 2 + 20 / 10 = 4
```