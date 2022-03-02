# Overview

````{margin}
```{admonition} AC power flow
See Section 4.1 of
[the MATPOWER User's Manual version 7.1](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).
```
````

*Anticipatory power flow (APF)* is an extension of the standard AC power flow problem.
Given a set of data pertaining to the preceding system snapshot
(*i.e.*, bus voltages, load draws, generator injections, and total losses),
and another set of data for the present (upcoming) dispatch
(*i.e.*, expected load draws,
scheduled limits on the generator injections,
and system parameters),
the goal is to "anticipate" generator injections that are feasible.
Feasibility in this sense is based on whether or not
a solution can be found when the power flow equations are solved for
the bus voltages given the present-dispatch data and
the anticipated generator injections.

The APF procedure consists of three parts:

1. estimating the anticipated generator injections using
   the previous-snapshot (or simply, snapshot) data
   and the present-dispatch (or simply, dispatch) data,
2. solving for the bus voltages from the power flow equations applied at
   the non-slack buses,
   and
3. refining the anticipated injections of the generators at the slack bus
   using the calculated voltages.

````{margin}
```{admonition} Factored Newton-Raphson
For an introduction, check out
[*Factorized Load Flow*](https://doi.org/10.1109/TPWRS.2013.2265298)
and
[*Factored solution of nonlinear equation systems*](https://doi.org/10.1098/rspa.2014.0236).
```
````

There are two key highlights in this work.
First, we describe methods for estimating the anticipated generator injections
using the snapshot and the dispatch data sets.
We also describe how to solve for the bus voltages
using a slightly modified version of *the factored Newton-Raphson* method.

````{margin}
```{admonition} AC optimal power flow
See Section 6.1 of
[the MATPOWER User's Manual version 7.1](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).
```
````

APF is best described as a heuristic procedure whose outcome is an input to some other procedure.
In fact, it was initially conceived as method for obtaining
an initial estimate of the generator injections and the bus voltages
in a standard AC optimal power flow task
such that the estimate already satisfies the power flow equations
for the given set of loads.

This document is a primer to the APF procedure.
It is assumed that the reader is acquainted with elementary power systems analysis
and steady-state modelling.
For questions, typos, and other concerns regarding this document,
please [open an issue](https://github.com/christian-cahig/Masterarbeit-APF/issues)
and use the `doc` label.

This website is powered by [Jupyter Book](https://jupyterbook.org/intro.html).
