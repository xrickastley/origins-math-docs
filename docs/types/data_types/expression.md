---
title: Expression (Data Type)
---

# Expression

[Data Type](../data_types.md)

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

### Random functions
| Name		      							| Function	 	| Syntax        		| Description   
|-------------------------------------------|---------------|-----------------------|---------------
| Random uniform continuous distribution	| `rUni()`		| `rUni(a, b)`			| Returns a random real number between `a` and `b`, inclusive, with equal probability for all numbers in the interval `[a, b]`.
| Random uniform discrete distribution		| `rUnid()`		| `rUnid(a, b)`			| Returns a random integer between `a` and `b`, inclusive, with equal probability for all numbers in the specified interval.
| Normal (Gaussian) continuous distribution	| `rNor()`		| `rNor(μ, σ)`			| Returns a real number drawn from `N(μ, σ)`; values are clustered around `m` with their spread determined by `s`.
| Random from given list					| `rList()`		| `rList(a, b, ..., z)`	| Returns a random number from the provided list of numbers `a, b, ..., z`, with equal probability for all numbers in the list.

### Random generators
| Name		      							| Function	| Description   
|-------------------------------------------|-----------|---------------------------------------
| Random integer							| `[Int]`	| Returns a random integer between `-2^31` and `2^31 - 1`, inclusive, with equal probability for all numbers in the specified interval.
| Random bounded integer					| `[IntX]`	| Returns a random integer between `-(10^X)` and `10^X`, inclusive, with equal probability for all numbers in the specified interval.
| Random natural number	(incl `0`)			| `[nat]`	| Returns a random integer between `0` and `2^31 - 1`, inclusive, with equal probability for all numbers in the specified interval.
| Random natural number	(incl `0`)			| `[natX]`	| Returns a random integer between `0` and `10^X`, inclusive, with equal probability for all numbers in the specified interval.
| Random natural number	(excl `0`)			| `[Nat]`	| Returns a random integer between `1` and `2^31 - 1`, inclusive, with equal probability for all numbers in the specified interval.
| Random natural number	(excl `0`)			| `[NatX]`	| Returns a random integer between `1` and `10^X`, inclusive, with equal probability for all numbers in the specified interval.
| Random uniform continuous distribution	| `[Uni]`	| Returns a random integer between `0` and `1`, inclusive, with equal probability for all numbers in the specified interval.
| Normal (Gaussian) continuous distribution	| `[Nor]`	| Returns a random integer between `-2^31` and `2^31 - 1`, inclusive, following the normal distribution `N(μ = 0, σ = 1)`.

For a full list of possible mathematical operations, operators, and more, you may look at the tutorial for [mXparser](https://mathparser.org/mxparser-tutorial/), the Math parser **Origins: Math** uses in evaluating mathematical expressions.

!!! warning
	A bug prevents some Unicode math symbols such as `⨉`, `√`, `π`, and etc., from working. Make sure that the resource returns a proper value. In this case, `NaN` automatically defaults to `0`, so try looking for Resource powers that have a value of `0` when they shouldn't. 