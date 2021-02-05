@def title = "JuliaDiff"
@def tags = ["JuliaLang", "AutoDiff", "AD"]

@@topmatter
# JuliaDiff
Differentiation tools in [Julia](https://julialang.org).
[JuliaDiff on GitHub](https://github.com/JuliaDiff/)
@@

## Stop approximating derivatives!

Derivatives are required at the core of many numerical algorithms. Unfortunately, they are usually computed _inefficiently_ and _approximately_ by some variant of the finite difference approach
$$ f'(x) \approx \frac{f(x+h) - f(x)}{h}, h \text{ small }. $$
This method is _inefficient_ because it requires $ \Omega(n) $ evaluations of $ f : \mathbb{R}^n \to \mathbb{R} $ to compute the gradient $ \nabla f(x) = \left( \frac{\partial f}{\partial x_1}(x), \cdots, \frac{\partial f}{\partial x_n}(x)\right) $, for example. It is _approximate_ because we have to choose some finite, small value of the step length $ h $, balancing floating-point precision with mathematical approximation error.


#### What can we do instead?
One option is to explicitly write down a function which computes the exact derivatives by using the rules that we know from Calculus. However, this quickly becomes an error-prone and tedious exercise. **There is another way!** The field of [automatic differentiation]("https://en.wikipedia.org/wiki/Automatic_differentiation") provides methods for automatically computing _exact_ derivatives (up to floating-point error) given only the function $ f $ itself. Some methods use many fewer evaluations of $ f $ than would be required when using finite differences. In the best case, **the exact gradient of $ f $ can be evaluated for the cost of $ O(1) $ evaluations of $ f $ itself**.  The caveat is that $ f $ cannot be considered a black box; instead, we require either access to the source code of $ f $ or a way to plug in a special type of number using operator overloading.

## What is JuliaDiff?
JuliaDiff is an informal organization which aims to unify and document packages written in [Julia](https://julialang.org) for evaluating derivatives. The technical features of Julia, namely, multiple dispatch, source code via reflection, JIT compilation, and first-class access to expression parsing make implementing and using techniques from automatic differentiation easier than ever before (in our biased opinion).


## Conclusion
Discussions on JuliaDiff and its uses may be directed to the [Julia Discourse forum](https://discourse.julialang.org/)
The [autodiff.org](http://www.autodiff.org/) site serves as a portal for the academic community, though it is often out-of-date.
The ChainRules project maintains a [list of recommend-reading/watching](https://www.juliadiff.org/ChainRulesCore.jl/stable/FAQ.html#Where-can-I-learn-more-about-AD-?) for those after more information.
Finally, automatic differentiation techniques have been implemented in a variety of languages.
If you would prefer not to use Julia, see the [wikipedia page](http://en.wikipedia.org/wiki/Automatic_differentiation") for a comprehensive list of available packages.

