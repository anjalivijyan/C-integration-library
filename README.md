# Numerical Integration Toolkit

A small header-only C++17 library and demo program for approximating definite integrals of 1D and 2D functions using several classic numerical quadrature methods, with error comparison against known exact (analytical) values.

## Overview

This project implements a common `Solver` interface with several concrete numerical integration strategies, plus a matching `Function` interface for the integrands themselves. `main.cpp` exercises all of them against a handful of test functions with known closed-form integrals, printing each method's approximation and its error relative to the exact value.

### Implemented methods (1D)

| Solver | Header | Method |
|---|---|---|
| `TrapezoidSolver` | `solvers/trapezoidsolver.hpp` | Composite trapezoidal rule |
| `SimpsonSolver` | `solvers/simpsonsolver.hpp` | Composite Simpson's rule |
| `GaussianSolver` | `solvers/gaussiansolver.hpp` | Composite 2-point Gauss-Legendre quadrature |
| `MonteCarloSolver` | `solvers/MonteCarloSolver.hpp` | Monte Carlo integration (fixed RNG seed for reproducibility) |

### 2D integration

| Solver | Header | Method |
|---|---|---|
| `Repeated1DSolver2D` | `solvers/repeated1dsolver2d.hpp` | Reduces a 2D integral to nested 1D integrals: an inner 1D solver (any `Solver`) integrates over `y` for fixed `x`, and the outer composite trapezoidal rule integrates the resulting values over `x` |

### Test functions

| Function | Header | Domain | Exact integral on `[0, 1]` |
|---|---|---|---|
| `XSquaredCos` — `x^2 cos(x)` | `functions/function.hpp` | 1D | `2cos(1) - sin(1)` |
| `XPowerTen` — `x^10` | `functions/function.hpp` | 1D | `1/11` |
| `InvSqrt` — `x^(-1/2)` | `functions/function.hpp` | 1D | `2` |
| `LogX` — `log(x)` | `functions/function.hpp` | 1D | `-1` |
| `XYFunction` — `x * y` | `functions/function.hpp` | 2D, `[0,1] x [0,1]` | `0.25` |
| `SinPiSinPi` — `sin(pi x) sin(pi y)` | `functions/function.hpp` | 2D | *(available, not exercised in `main.cpp`)* |

All functions derive from the abstract base classes `Function` (1D) or `Function2D` (2D), so adding a new integrand just means subclassing one of these and implementing `operator()`.

## Project structure

```
.
├── main.cpp
├── Makefile
├── functions/
│   └── function.hpp        # Function, Function2D and all test integrands
└── solvers/
    ├── solver.hpp           # abstract 1D Solver interface
    ├── solver2d.hpp         # abstract 2D Solver2D interface
    ├── trapezoidsolver.hpp
    ├── simpsonsolver.hpp
    ├── gaussiansolver.hpp
    ├── MonteCarloSolver.hpp
    └── repeated1dsolver2d.hpp
```

