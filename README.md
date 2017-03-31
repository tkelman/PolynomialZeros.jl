# PolynomialZeros

Methods to find zeros (roots) of polynomials over given domains

[![Build Status](https://travis-ci.org/jverzani/PolynomialZeros.jl.svg?branch=master)](https://travis-ci.org/jverzani/PolynomialZeros.jl)

[![Coverage Status](https://coveralls.io/repos/jverzani/PolynomialZeros.jl/badge.svg?branch=master&service=github)](https://coveralls.io/github/jverzani/PolynomialZeros.jl?branch=master)

[![codecov.io](http://codecov.io/github/jverzani/PolynomialZeros.jl/coverage.svg?branch=master)](http://codecov.io/github/jverzani/PolynomialZeros.jl?branch=master)


This package provides the method `poly_zeros` to find zeros of univariate polynomial functions over the complex numbers, the real numbers, the rationals, the integers, and $Z_p$.

The basic interface is

```
poly_zeros(f, domain)
```

Where `f` is in `Poly{T}` (from the `Polynomials.jl` package) or can be converted into `Poly{T}`. The domain is specified by `Over.C`, `Over.R`, `Over.Q`, `Over.Z`, or `over.Zp{p}`. Not all polynomials will have such a factorization.


Examples:

```
julia> poly_zeros(x ->x^4 - 1, Over.C)  # uses `roots` from `Polynomials.jl`
4-element Array{Complex{Float64},1}:
 -1.0+4.44089e-16im 
   5.55112e-17+1.0im
  -2.76424e-17-1.0im
           1.0+0.0im

julia> poly_zeros(x ->x^4 - 1, Over.R)  
2-element Array{Any,1}:
  1.0
 -1.0

julia> poly_zeros(x ->x^4 - 1, Over.Q) # uses `PolynomialFactors.jl`
2-element Array{Rational{Int64},1}:
 -1//1
  1//1

julia> poly_zeros(x ->x^4 - 1, Over.Z) # uses `PolynomialFactors.jl`
2-element Array{Int64,1}:
 -1
  1

julia> poly_zeros(x ->x^4 - 1, Over.Zp{5}) # uses `PolynomialFactors.jl`
4-element Array{Int64,1}:
 4
 1
 3
 2
```


## Motivation

The `Polynomials` package provides a `roots` command to find the roots of a polynomial by finding the eigenvalues of the companion matrix, but it has a few limitations:

* for technical reasons, it doesn't work with "big" values
* it will be slow for very high degree polynomials
* it can have numeric issues when there are multiplicities
* it may return complex values near an actual real root, rather than real values

This package uses the `PolynomialZeros` package to find roots over the complex numbers; it uses `PolynomialFactors` to return roots over the rationals and integers; and it provides an algorithm to find all real roots and an algorithm to find roots when there are expected multiplicities. In addition, it plans to provide a fast alogrithm for high-degree polynomials.


## Other possible useful methods

The package also provides

* `agcd` for computing an *approximate* GCD of polynomials `p` and `q` over `Poly{Float64}`.

* `multroot` for finding roots of `p` in `Poly{Float64}` over `Complex{Float64}` which has some advantage if `p` has high multiplicities. The `roots` function from the Polynomials package will find all the roots of a polynomial. Its performance degrades when the polynomial has high multiplicities. The multroot function is provided to handle this case a bit better. The function follows algorithms due to Zeng, "Computing multiple roots of inexact polynomials", Math. Comp. 74 (2005), 869-903.

