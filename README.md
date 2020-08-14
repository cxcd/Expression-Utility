# Expression-Utility

A performant multivariable recursive descent parser for math expressions, supporting all simple operations and several common functions including sin, cos, tan, abs, exp, log, and sqrt. Allows for very easy expansion of function support. Uses the following grammar:

**e**xpression = t + e | t - e | t  
**t**erm = t \* f | t / f | f  
**f**actor = f ^ p | p  
**p**ower = function(v) | variable | v  
**v**alue = literal | (e)  

## Usage
Add both files to your project, and include the header in the relevant file. Then, create an object of the desired type like so:
```
ExpressionUtil::ExpressionFloat myExprFloat("sin(x) * cos(y)"); 
ExpressionUtil::ExpressionDouble myExprDouble("sin(x) * cos(y)");
```
On construction the expression is tokenized. If your expression includes variables, update the variables like so:
```
myExprFloat.variables["x"] = 5;
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
		std::cout << test.solve() << "\n";
	}
}
```
## Adding extra functions
Inside the header is the variable `std::unordered_map<std::string, FnPtr> funcMap`. Functions are simply defined as a relation between a string and a function pointer `typedef T(*FnPtr)(T);`. To add more functions, simply add a new line to the instantiation, e.g. `{"acos", acos}` will add support for arccosine by parsing `acos` as a call to the function of the same name. Adding custom functions can be done this way as well, e.g.`{"degToRad", degToRad}` where `degToRad()` is a user defined function that converts degrees to radians.
