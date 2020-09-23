# Expression Utility

A performant multivariable recursive descent parser for math expressions, supporting all simple operations and several common functions including sin, cos, tan, abs, exp, log, and sqrt. Allows for very easy user expansion of the supported functions.

## Usage
Add both files to your project, and include the header in the relevant file. Then, create an object of the desired type like so:
```
ExpressionUtil::ExpressionFloat myExprFloat("sin(x) * cos(y)"); 
ExpressionUtil::ExpressionDouble myExprDouble("sin(x) * cos(y)");
```
Alternatively you can construct the object without setting an expression, and set it later:
```
ExpressionUtil::ExpressionFloat myExprFloat(); 
myExprFloat.set("sin(x) * cos(y)");
```
The function `set()` can be used to reset the expression of an already constructed object if desired. When the expression is set, it is stripped of white space, made lowercase, and tokenized. Variables can be any length. If your expression includes variables, update the variables like so:
```
myExprFloat.variables["x"] = 5;
myExprFloat.variables["myvariable"] = 5;
```
From here, it is as simple as calling `solve()` on the object. This returns a solution of the objects type, either a float or a double:
```
float solutionFloat = myExprFloat.solve();
double solutionDouble = myExprFouble.solve();
```

Here is an example of solving a function while updating its variables:
```
ExpressionUtil::ExpressionFloat myExpr("sin(x) * cos(y)");
for (int i = 0; i < 10; i++) {
	for (int j = 0; j < 10; j++) {
		myExpr.variables["x"] = i;
		myExpr.variables["y"] = j;
		std::cout << myExpr.solve() << "\n";
	}
}
```

## Adding extra functions
Inside the header is the variable `std::unordered_map<std::string, FnPtr> funcMap`. Functions are simply defined as a relation between a string and a function pointer `typedef T(*FnPtr)(T);`. To add more functions, simply add a new line to the instantiation, e.g. `{"acos", acos}` will add support for arccosine by parsing `acos` as a call to the function of the same name. Adding custom functions can be done this way as well, e.g.`{"degToRad", degToRad}` where `degToRad()` is a user defined function that converts degrees to radians.

## Benchmark
Using the expression `x^2/sin(2*pi/y)-x/2` with an object of type float over 10,000 executions, I recorded an average of 54.15 ns.
