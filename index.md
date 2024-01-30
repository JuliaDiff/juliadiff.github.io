@def title = "JuliaDiff"
@def tags = ["JuliaLang", "AutoDiff", "AD"]

@@topmatter
# JuliaDiff
Differentiation tools in Julia
@@

## Computing derivatives

Derivatives are required by many numerical algorithms, even for functions $f$ given as chunks of computer code rather than simple mathematical expressions.
There are various ways to compute the gradient, Jacobian or Hessian of such functions without tedious manual labor:

- Symbolic differentiation, which uses [computer algebra](https://en.wikipedia.org/wiki/Computer_algebra) to work out explicit formulas
- [Numerical differentiation](https://en.wikipedia.org/wiki/Numerical_differentiation), which relies on variants of the finite difference approximation $f'(x) \approx \frac{f(x+\varepsilon) - f(x)}{\varepsilon}$
- [Automatic differentiation](https://en.wikipedia.org/wiki/Automatic_differentiation) (AD), which reinterprets the code of $f$ either in "forward" or "reverse" mode

In machine learning, automatic differentiation is probably the most widely used paradigm, especially in reverse mode.
However, each method has its own upsides and tradeoffs, which are detailed in the following papers, along with implementation techniques like "operator overloading" and "source transformation":

> [_Automatic differentiation in machine learning: a survey_](https://jmlr.org/papers/v18/17-468.html), Baydin et al. (2018)

> [_A review of automatic differentiation and its efficient implementation_](https://arxiv.org/abs/1811.05031), Margossian (2019)

## What is JuliaDiff?

JuliaDiff is an informal [GitHub organization](https://github.com/JuliaDiff/) which aims to unify and document packages written in [Julia](https://julialang.org) for evaluating derivatives.
The technical features of Julia[^1] make implementing and using differentiation techniques easier than ever before (in our biased opinion).

Discussions on JuliaDiff and its uses may be directed to the [Julia Discourse forum](https://discourse.julialang.org/).
The ChainRules project maintains a [list of recommended reading](https://www.juliadiff.org/ChainRulesCore.jl/stable/FAQ.html#Where-can-I-learn-more-about-AD-?) for those after more information.
The [autodiff.org](http://www.autodiff.org/) site serves as a portal for the academic community, though it is often out of date.

## The Big List

What follows is a big list of Julia differentiation packages and related tooling, last updated in January 2024.
If you notice something inaccurate or outdated, please [open an issue](https://github.com/JuliaDiff/juliadiff.github.io/issues) to signal it.
The packages marked as inactive are those which have had no release in 2023.

The list aims to be comprehensive in coverage.
By necessity, this means it is not comprehensive in detail.
It is worth investigating each package yourself to really understand its ins and outs, and the pros and cons of its competitors.

### Reverse mode automatic differentiation

- [JuliaDiff/ReverseDiff.jl](https://github.com/JuliaDiff/ReverseDiff.jl): Operator overloading AD backend
- [FluxML/Zygote.jl](https://github.com/FluxML/Zygote.jl): Source transformation AD backend
- [EnzymeAD/Enzyme.jl](https://github.com/EnzymeAD/Enzyme.jl): LLVM-level source transformation AD backend
- [dfdx/Yota.jl](https://github.com/dfdx/Yota.jl): Source transformation AD backend
- [FluxML/Tracker.jl](https://github.com/FluxML/Tracker.jl): Operator overloading AD backend (mostly deprecated in favor of Zygote.jl)

### Forward mode automatic differentiation

- [JuliaDiff/ForwardDiff.jl](https://github.com/JuliaDiff/ForwardDiff.jl): Operator overloading AD backend
- [JuliaDiff/PolyesterForwardDiff.jl](https://github.com/JuliaDiff/PolyesterForwardDiff.jl): Multithreaded version of ForwardDiff.jl
- [JuliaDiff/Diffractor.jl](https://github.com/JuliaDiff/Diffractor.jl): Source transformation AD backend (experimental)

### Symbolic differentiation

- [JuliaSymbolics/Symbolics.jl](https://github.com/JuliaSymbolics/Symbolics.jl): Pure Julia computer algebra system with support for fast and sparse analytical derivatives
- [brianguenter/FastDifferentiation.jl](https://github.com/brianguenter/FastDifferentiation.jl): Generate efficient executables for symbolic derivatives

### Numeric differentiation

- [JuliaDiff/FiniteDifferences.jl](https://github.com/JuliaDiff/FiniteDifferences.jl): Finite differences with support for arbitrary types and higher order schemes
- [JuliaDiff/FiniteDiff.jl](https://github.com/JuliaDiff/FiniteDiff.jl): Finite differences with support for caching and sparsity

### Higher order

- [JuliaDiff/TaylorSeries.jl](https://github.com/JuliaDiff/TaylorSeries.jl): Taylor polynomial expansions in one or more variables
- [JuliaDiff/TaylorDiff.jl](https://github.com/JuliaDiff/TaylorDiff.jl): Higher order directional derivatives (experimental)

### Rulesets

These packages define derivatives for basic functions, and enable users to do the same:

- [JuliaDiff/ChainRules](https://www.juliadiff.org/ChainRulesCore.jl/stable/): Ecosystem for backend-agnostic forward and reverse rules.
  - [JuliaDiff/ChainRulesCore.jl](https://github.com/JuliaDiff/ChainRulesCore.jl): Core API for users to add rules to their package.
  - [JuliaDiff/ChainRules.jl](https://github.com/JuliaDiff/ChainRules.jl/): Rules for Julia Base and standard libraries.
  - [JuliaDiff/ChainRulesTestUtils.jl](https://github.com/JuliaDiff/ChainRulesTestUtils.jl/): Tools for testing rules defined with ChainRulesCore.jl.
- [ThummeTo/ForwardDiffChainRules.jl](https://github.com/ThummeTo/ForwardDiffChainRules.jl): Translate rules from ChainRulesCore.jl to make them compatible with ForwardDiff.jl
- [JuliaDiff/DiffRules.jl](https://github.com/JuliaDiff/DiffRules.jl): Scalar rules used by e.g. ForwardDiff.jl, ReverseDiff.jl, Zygote.jl, Tracker.jl, and Symbolics.jl
- [FluxML/ZygoteRules.jl](https://github.com/FluxML/ZygoteRules.jl): Some rules used by Zygote.jl (mostly deprecated in favor of ChainRules.jl).

### Interface

- [AbstractDifferentiation.jl](https://github.com/JuliaDiff/AbstractDifferentiation.jl): Backend-agnostic interface for algorithms that rely on derivatives, gradients, Jacobians, Hessians, etc.

### Exotic

- [JuliaDiff/SparseDiffTools.jl](https://github.com/JuliaDiff/SparseDiffTools.jl): Exploit sparsity to speed up FiniteDiff.jl and ForwardDiff.jl, as well as other algorithms.
- [gaurav-arya/StochasticAD.jl](https://github.com/gaurav-arya/StochasticAD.jl): Differentiation of functions with stochastic behavior (experimental)

### Differentiating through more stuff

Some complex algorithms are not natively differentiable, which is why derivatives have been implemented in the following packages:

- [SciML](https://github.com/SciML): For a lot of different domains of scientific machine learning: differential equations, linear and nonlinear systems, optimization problems, etc.
- [gdalle/ImplicitDifferentiation.jl](https://github.com/gdalle/ImplicitDifferentiation.jl): For generic algorithms specified by output conditions, thanks to the implicit function theorem
- [jump-dev/DiffOpt.jl](https://github.com/jump-dev/DiffOpt.jl): For convex optimization problems
- [axelparmentier/InferOpt.jl](https://github.com/axelparmentier/InferOpt.jl): For combinatorial optimization problems

### Inactive packages

- [denizyuret/AutoGrad.jl](https://github.com/denizyuret/AutoGrad.jl)
- [dfdx/XGrad.jl](https://github.com/dfdx/XGrad.jl)
- [dpsanders/ReversePropagation.jl](https://github.com/dpsanders/ReversePropagation.jl)
- [GiggleLiu/NiLang.jl](https://github.com/GiggleLiu/NiLang.jl)
- [invenia/Nabla.jl](https://github.com/invenia/Nabla.jl/)
- [JuliaDiff/DualNumbers.jl](https://github.com/JuliaDiff/DualNumbers.jl)
- [JuliaMath/Calculus.jl](https://github.com/JuliaMath/Calculus.jl)
- [rejuvyesh/PyCallChainRules.jl](https://github.com/rejuvyesh/PyCallChainRules.jl)
- [SciML/SparsityDetection.jl](https://github.com/SciML/SparsityDetection.jl)
- [YingboMa/ForwardDiff2](https://github.com/YingboMa//ForwardDiff2.jl)

[^1]: namely, multiple dispatch, source code via reflection, just-in-time compilation, and first-class access to expression parsing
