# Overview

***Anticipatory power flow (APF)*** is an extension of the standard AC power flow problem[^std-acpf].
Given a set of *previous-snapshot data*
(*e.g.*, bus voltages, load draws, generator injections, and total losses),
and
a set of *present-dispatch data*
(*e.g.*, expected load draws, scheduled limits on generator injections),
we "anticipate" generator injections that are feasible.
Feasibility in this sense is based on whether or not such generator injections
lead to a solution when we solve for the bus voltages
from the power flow equations defined by the present-dispatch data.

The procedure consists of three stages.
First, we estimate the anticipated generator injections using the previous-snapshot and the present-dispatch data.
Then, we form the power flow equations at the non-slack buses, and from them solve for the corresponding bus voltages.
Lastly, we adjust our estimate of slack-bus generator injections according to the computed bus voltages.

There are two highlights in this work.
First, we describe methods for estimating the anticipated generator injections using the snapshot and dispatch data.
Second, we present a method for solving the bus voltages through a slightly modified version of the *factored Newton-Raphson* method[^fac-nr].

```{tip}
It is best to think of APF as a heuristic procedure whose outcome is an input to some other procedure in some workflow.
In fact, it was initially conceived as a method for obtaining an initial estimates of the generator injections and the bus voltages
for a standard AC optimal power flow task[^std-acopf] such that the estimates already satisfy the power flow equations.
```

[^std-acpf]: See Section 4.1 of
[*MATPOWER User's Manual version 7.1*](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).

[^fac-nr]: This is based on the works
[*Factorized Load Flow*](https://doi.org/10.1109/TPWRS.2013.2265298)
and
[*Factored solution of nonlinear equation systems*](https://doi.org/10.1098/rspa.2014.0236).

[^std-acopf]: See Section 6.1 of
[*MATPOWER User's Manual version 7.1*](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).
