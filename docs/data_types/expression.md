# Expression

[Data Type](../types/data_types.md)

A [String](https://origins.readthedocs.io/en/latest/types/data_types/string/) representing a mathematical expression.

### Operators
| Operation      			| Operator(s) 	| Syntax        | Description   
|---------------------------|:-------------:|---------------|---------------
| Addition       			| `+`			| `a + b`		| Adds `a` by `b`.  
| Subtraction    			| `-`			| `a - b`		| Subtracts `a` from `b`. 
| Multiplication 			| `*`, `×`		| `a × b`		| Multiplies `a` by `b`.
| Division       			| `/`, `÷`		| `a ÷ b`		| Divides `a` by `b`.
| Fraction       			| `_`			| `a_b`			| Represents a fraction of `a / b`.
| Mixed number     			| `_`			| `c_a_b`		| Represents a fraction of `a / b` with a whole number `c`, or `c + (a / b)`.
| Exponentiation		    | `^`    		| `a^b`			| Raises `a` by the power of `b`.
| Factorial				    | `!`    		| `a!`			| Returns the factorial of `a`.
| Modulo				    | `#`    		| `a # b`		| Returns the modulo of `a` by `b`.
| Percentage			    | `%`    		| `a%`			| Makes `a` a percentage, then turns it into it's decimal format: `(a / 100)`.
| Negation				    | `-`    		| `-a`			| Returns the negative value of `a`.
| Tetration	or hyper-4	    | `^^`    		| `a^^b`		| Raises `a` by itself `b` times.
| Integer division		    | `\`    		| `a\b`			| Returns the integer part of a division operation.
| Implied multiplication    | `()`    		| `a(b)`		| Multiplies `a` by `b`.
| Square Root			    | `sqrt()` 		| `sqrt(a)`		| Returns the square root of `a`.
| Absolute value            | `abs()`       | `abs(a)`      | Returns the absolute value of `a`.

### Functions
| Name		      					| Function	 								| Syntax        | Description   
|-----------------------------------|-------------------------------------------|---------------|---------------
| Trigonometric sine				| `sin()`									| `sin(a)`		| Returns the sine of `a`.
| Trigonometric cosine				| `cos()`									| `cos(a)`		| Returns the cosine of `a`.
| Trigonometric tangent				| `tan()`, `tg()`							| `tan(a)`		| Returns the tangent of `a`.
| Trigonometric secant				| `sec()`									| `sec(a)`		| Returns the secant of `a`.
| Trigonometric cosecant			| `csc()`, `cosec()`						| `csc(a)`		| Returns the cosecant of `a`.
| Trigonometric cotangent			| `cot()`, `ctg()`, `ctan()`				| `cot(a)`		| Returns the cotangent of `a`.
| Inverse trigonometric sine		| `asin()`, `arsin()`, `arcsin()`			| `asin(a)`		| Returns the inverse sine of `a`.
| Inverse trigonometric cosine		| `acos()`, `arcos()`, `arccos()`			| `acos(a)`		| Returns the inverse cosine of `a`.
| Inverse trigonometric tangent		| `atan()`, `atg()`, `arctg()`, `arctan()`	| `atan(a)`		| Returns the inverse tangent of `a`.
| Natural logarithm (base e)		| `ln()`									| `ln(a)`		| Returns the natural logarithm of `a`.
| Binary logarithm (base 2)			| `log2()`									| `log2(a)`		| Returns the binary logarithm of `a`.
| Common logarithm (base 10)		| `lg()`, `log10()`							| `log10(a)`	| Returns the common logarithm of `a`.


For a full list of possible mathematical operations, operators, and more, you may look at the tutorial for [mXparser](https://mathparser.org/mxparser-tutorial/), the Math parser **Origins: Math** uses in evaluating mathematical expressions.

!!! warning
	A bug prevents some Unicode math symbols such as `⨉`, `√`, `π`, and etc., from working. Make sure that the resource returns a proper value. In this case, `NaN` automatically defaults to `0`, so try looking for Resource powers that have a value of `0` when they shouldn't. 