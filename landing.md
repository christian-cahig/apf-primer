# Overview

```{margin}
**Standard AC power flow**
See Section 4.1 of
[*MATPOWER User's Manual version 7.1*](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).
```

***Anticipatory power flow (APF)*** is an extension of the *standard AC power flow* problem.
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
Second, we present a method for solving the bus voltages through the *factored Newton-Raphson* method,
which was introduced in the works
[*Factorized Load Flow*](https://doi.org/10.1109/TPWRS.2013.2265298)
and
[*Factored solution of nonlinear equation systems*](https://doi.org/10.1098/rspa.2014.0236).

This website is the accompanying primer to the repository
[github.com/christian-cahig/Masterarbeit-APF](https://github.com/christian-cahig/Masterarbeit-APF).

## Contents

```{tableofcontents}
```

## Correspondence and citing

For questions, typos, and other concerns regarding this document,
please [open an issue](https://github.com/christian-cahig/Masterarbeit-APF/issues)
and use the `documentation` label.

To cite this document, please use

```bibtex
@Online{CCahigAPFPrimer,
  author = {Christian {Cahig}},
  title  = {A Primer on Anticipatory Power Flow},
  url    = {https://christian-cahig.github.io/apf-primer/},
}
```

## Acknowledgements

This work is supervised jointly by Dr. Marven E. Jabian and Abdul Aziz G. Mabaning,
and has been supported by the DOST-SEI ERDT program.
