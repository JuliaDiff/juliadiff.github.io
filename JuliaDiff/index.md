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
One option is to explicitly write down a function which computes the exact derivatives by using the rules that we know from calculus. However, this quickly becomes an error-prone and tedious exercise. **There is another way!** The field of [automatic differentiation]("https://en.wikipedia.org/wiki/Automatic_differentiation") provides methods for automatically computing _exact_ derivatives (up to floating-point error) given only the function $ f $ itself. Some methods use many fewer evaluations of $ f $ than would be required when using finite differences. In the best case, **the exact gradient of $ f $ can be evaluated for the cost of $ O(1) $ evaluations of $ f $ itself**.  The caveat is that $ f $ cannot be considered a black box; instead, we require either access to the source code of $ f $ or a way to plug in a special type of number using operator overloading.

## What is JuliaDiff?
JuliaDiff is an informal organization which aims to unify and document packages written in [Julia](https://julialang.org) for evaluating derivatives. The technical features of Julia, namely, multiple dispatch, source code via reflection, JIT compilation, and first-class access to expression parsing make implementing and using techniques from automatic differentiation easier than ever before (in our biased opinion).


## The Big List
This is a big list of Julia AutoDiff packages and related tooling.
As you can see there is a lot going on here.
As with any such big lists it rapidly becomes out-dated.
When (not if) you notice something that is out of date, or just plain wrong, please [submit a PR](https://github.com/JuliaDiff/juliadiff.github.io).

This list aims to be comprehensive in coverage.
By necessity, this means it is not comprehensive in detail.
It is worth investigating each package yourself to really understand its ins and outs, and pros and cons of its competitors.

### Reverse Mode:
- [ReverseDiff.jl](https://github.com/JuliaDiff/ReverseDiff.jl): Operator overloading AD. Very-well established.
- [Nabla.jl](https://github.com/invenia/Nabla.jl/): Operator overloading reverse-mode AD. Primarily used are in (it's maintainer) Invenia's systems. 
- [Tracker.jl](https://github.com/FluxML/Tracker.jl): Operator overloading reverse-mode AD. Most well-known for no longer being [Flux's](https://github.com/FluxML/Flux.jl) AD. But still used in several places about the ecosystem.
- [AutoGrad.jl](https://github.com/denizyuret/AutoGrad.jl): Originally a port of the python AutoGrad package. Primarily used in [Knet](https://github.com/denizyuret/Knet.jl/).

- [Zygote.jl](https://github.com/FluxML/Zygote.jl): IR-level source to source reverse-mode AD. Very widely used. Particularly notable for use in [Flux](https://github.com/FluxML/Flux.jl). Also features a secret experimental source to source forwards mode AD.
- [Yota.jl](https://github.com/dfdx/Yota.jl): IR-level source to source reverse-mode AD.
- [XGrad.jl](https://github.com/dfdx/XGrad.jl): AST level source to source reverse-mode AD. Not currently in active development.
- [ReversePropagation.jl](https://github.com/dpsanders/ReversePropagation.jl): Scalar, tracing-based source to source reverse-mode AD.
- [Enzyme.jl](https://github.com/wsmoses/Enzyme.jl): Scalar, LLVM source to source reverse-mode AD. Experimental.


### Forward mode
- [ForwardDiff.jl](https://github.com/JuliaDiff/ForwardDiff.jl): scalar operator overloading forward-mode AD. Very stable. Very well-established.
- [ForwardDiff2](https://github.com/YingboMa//ForwardDiff2.jl): experimental non-scalar hybrid operator-overloading/source-to-source forward-mode AD. Not currently in development.

### Symbolic:
- [ModelingToolKit.jl](https://github.com/JuliaDiffEq/ModelingToolkit.jl): A pure Julia computer algebra system. While its docs focus on some particular domain use-case it is a fully general purpose system.

### Exotic
- [TaylorSeries.jl](https://github.com/JuliaDiff/TaylorSeries.jl) computes polynomial expansions; which is the generalization of forward-mode AD to n-th order derivatives.
- [NiLang.jl](https://github.com/GiggleLiu/NiLang.jl): Reversible computing DSL, where everything is differentiable, by reversing.

### Finite Differencing
(yes, we said at the start to stop approximating deriviatives, but these packages are faster and more accurate than you would expect finite differencing to every achieve.)

- [FiniteDifferences.jl](https://github.com/JuliaDiff/FiniteDifferences.jl): High-accuracy finite differencing with support for almost any type (not just the usual arrays and numbers): [FiniteDiff](https://github.com/JuliaDiff/FiniteDiff.jl): High-accuracy finite differencing with support for efficient calculation of spares Jacobians via coloring vectors.
- [Calculus.jl](https://github.com/JuliaMath/Calculus.jl): Largely deprecated, legacy package. New users should look to FiniteDifferences and FiniteDiff instead.

### Rulesets:
- [ChainRules.jl](https://www.juliadiff.org/ChainRulesCore.jl/stable/): extensible AD independent rule-set.
  - [ChainRulesCore.jl](https://github.com/JuliaDiff/ChainRulesCore.jl): Core API for user to extend to add rules to their package.
  - [ChainRules.jl](https://github.com/JuliaDiff/ChainRules.jl/): the rules for Base and other standard libraries.
  - [ChainRulesTestUtils.jl](https://github.com/JuliaDiff/ChainRulesTestUtils.jl/): testing tools to check rules defined with ChainRulesCore.
- [DiffRules.jl](https://github.com/JuliaDiff/ChainRulesCore.jl): Original multi-AD system rule-set. Largely deprecated in-favor of ChainRules.
- [ZygoteRules.jl](https://github.com/FluxML/ZygoteRules.jl/blob/master/src/ZygoteRules.jl): for adding rules to Zygote. Largely deprecated in-favour of ChainRules.

### Sparsity
- [SparsityDetection.jl](https://github.com/SciML/SparsityDetection.jl): automatic Jacobian and Hessian sparsity pattern detection.
- [SparseDiffTools.jl](https://github.com/JuliaDiff/SparseDiffTools.jl): exploiting sparsity to speed up FiniteDiff and ForwardDiff; as well as other algorithms.


## Links for discussion and more information
Discussions on JuliaDiff and its uses may be directed to the [Julia Discourse forum](https://discourse.julialang.org/)
The [autodiff.org](http://www.autodiff.org/) site serves as a portal for the academic community, though it is often out-of-date.
The ChainRules project maintains a [list of recommend-reading/watching](https://www.juliadiff.org/ChainRulesCore.jl/stable/FAQ.html#Where-can-I-learn-more-about-AD-?) for those after more information.
Finally, automatic differentiation techniques have been implemented in a variety of languages.
If you would prefer not to use Julia, see the [wikipedia page](http://en.wikipedia.org/wiki/Automatic_differentiation") for a comprehensive list of available packages.

