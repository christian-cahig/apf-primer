(prelim:pow-flow-eqns)=
# Power flow equations

(prelim:pow-flow-eqns:balance)=
## Nodal power balance

The *power flow equations* embody the nodal power balance requirements[^about-bim],
that is,
the value of the net nodal injections calculated from the system state variables,
$\boldsymbol{e} \! \left( \boldsymbol{s} \right)$,
must match that from the control variables and the load profile,
$\boldsymbol{e} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$,
*i.e.*,

$$
\left[ \begin{matrix}
    \operatorname{ diag } \! \left( \boldsymbol{v}_{ \mathsf{r} } \right)
    \left(
        \boldsymbol{G} \boldsymbol{v}_{ \mathsf{r} }
        -
        \boldsymbol{B} \boldsymbol{v}_{ \mathsf{i} }
    \right)
    -
    \operatorname{ diag } \! \left( \boldsymbol{v}_{ \mathsf{i} } \right)
    \left(
        \boldsymbol{B} \boldsymbol{v}_{ \mathsf{r} }
        +
        \boldsymbol{G} \boldsymbol{v}_{ \mathsf{i} }
    \right)
    \\
    \operatorname{ diag } \! \left( \boldsymbol{v}_{ \mathsf{i} } \right)
    \left(
        \boldsymbol{G} \boldsymbol{v}_{ \mathsf{r} }
        -
        \boldsymbol{B} \boldsymbol{v}_{ \mathsf{i} }
    \right)
    -
    \operatorname{ diag } \! \left( \boldsymbol{v}_{ \mathsf{r} } \right)
    \left(
        \boldsymbol{B} \boldsymbol{v}_{ \mathsf{r} }
        +
        \boldsymbol{G} \boldsymbol{v}_{ \mathsf{i} }
    \right)
    \\
\end{matrix} \right]
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{g} } \, \boldsymbol{p}_{ \mathsf{g} }
    -
    \boldsymbol{C}_{ \mathsf{d} } \, \boldsymbol{p}_{ \mathsf{d} }
    \\
    \boldsymbol{C}_{ \mathsf{g} } \, \boldsymbol{q}_{ \mathsf{g} }
    -
    \boldsymbol{C}_{ \mathsf{d} } \, \boldsymbol{q}_{ \mathsf{d} }
    \\[1em]
\end{matrix} \right]
= \,
\boldsymbol{C}_{ \mathsf{G} } \, \boldsymbol{c}
-
\boldsymbol{C}_{ \mathsf{D} } \, \boldsymbol{d}
$$

In the *factored formulation* of the power flow equations,
the focus is for the net nodal injections calculated from the canonicalized state vector,
$\boldsymbol{e} \! \left( \boldsymbol{y} \right)$,
to match
$\boldsymbol{e} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$.
This restatement gives us two linear systems coupled by an elementary nonlinear map[^about-fac-pfe]:

$$
\left\{ \begin{aligned}
    \boldsymbol{E} \boldsymbol{y}
    & =
    \boldsymbol{C}_{ \mathsf{G} } \, \boldsymbol{c} - \boldsymbol{C}_{ \mathsf{D} } \, \boldsymbol{d}
    \\
    \boldsymbol{C} \boldsymbol{x} & = \operatorname{ sigma } \! \left( \boldsymbol{y} \right)
\end{aligned} \right.
\, .
$$

```{note}
The requirement
$\boldsymbol{E} \boldsymbol{y} = \boldsymbol{C}_{ \mathsf{G} } \, \boldsymbol{c} - \boldsymbol{C}_{ \mathsf{D} } \, \boldsymbol{d}$
subsumes
$\boldsymbol{z} \! \left( \boldsymbol{y} \right) \triangleq \boldsymbol{Z} \boldsymbol{y} = \boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$.
```

[^about-bim]: Strictly speaking, this is the "bus injection model" of the power flow equations.
Another prominent (equivalent) formulation is the "branch flow model"
(see Section 5.2 of
[*An introduction to optimal power flow: Theory, formulation, and examples*](https://doi.org/10.1080/0740817X.2016.1189626)).

[^about-fac-pfe]: The idea is based on Equations (27) and (28) of
[*Factorized Load Flow*](https://doi.org/10.1109/TPWRS.2013.2265298).

(prelim:pow-flow-eqns:residuals)=
## Nodal power residuals

The *nodal power residuals* are the 'mismatches' between
$\boldsymbol{e} \! \left( \boldsymbol{y} \right)$
and
$\boldsymbol{e} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$.
We define the *full nodal residual vector* $\boldsymbol{\phi}$ as

$$
\boldsymbol{\phi} \! \left( \boldsymbol{y}, \boldsymbol{c}, \boldsymbol{d} \right)
\triangleq
\boldsymbol{e} \! \left( \boldsymbol{y} \right)
- \boldsymbol{e} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)
=
\boldsymbol{E} \boldsymbol{y}
\, -
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{g} }
    &
    \boldsymbol{0}_{ N,G }          \, \\
    \boldsymbol{0}_{ N,G }
    &
    \boldsymbol{C}_{ \mathsf{g} }   \, \\
\end{matrix} \right]
\boldsymbol{c}
\, +
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{d} }
    &
    \boldsymbol{0}_{ N,D }          \, \\
    \boldsymbol{0}_{ N,D }
    &
    \boldsymbol{C}_{ \mathsf{d} }   \, \\
\end{matrix} \right]
\boldsymbol{d}
\, \, \in \mathbb{R}^{ 2N }
\, ,
$$ (y-c-d-2-phi)

or, in terms of
$\boldsymbol{e} \! \left( \boldsymbol{s} \right)$
and
$\boldsymbol{e} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$,

$$
\begin{aligned}
    \boldsymbol{\phi} \! \left( \boldsymbol{s}, \boldsymbol{c}, \boldsymbol{d} \right)
    & \triangleq
    \boldsymbol{e} \! \left( \boldsymbol{s} \right)
    - \boldsymbol{e} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)
    \\
    & =
    \left[ \begin{matrix}
        \operatorname{ diag } \! \left( \boldsymbol{v}_{ \mathsf{r} } \right)
        \left(
            \boldsymbol{G} \boldsymbol{v}_{ \mathsf{r} }
            -
            \boldsymbol{B} \boldsymbol{v}_{ \mathsf{i} }
        \right)
        -
        \operatorname{ diag } \! \left( \boldsymbol{v}_{ \mathsf{i} } \right)
        \left(
            \boldsymbol{B} \boldsymbol{v}_{ \mathsf{r} }
            +
            \boldsymbol{G} \boldsymbol{v}_{ \mathsf{i} }
        \right)
        \\
        \operatorname{ diag } \! \left( \boldsymbol{v}_{ \mathsf{i} } \right)
        \left(
            \boldsymbol{G} \boldsymbol{v}_{ \mathsf{r} }
            -
            \boldsymbol{B} \boldsymbol{v}_{ \mathsf{i} }
        \right)
        -
        \operatorname{ diag } \! \left( \boldsymbol{v}_{ \mathsf{r} } \right)
        \left(
            \boldsymbol{B} \boldsymbol{v}_{ \mathsf{r} }
            +
            \boldsymbol{G} \boldsymbol{v}_{ \mathsf{i} }
        \right)
        \\
    \end{matrix} \right]
    -
    \left[ \begin{matrix}
        \boldsymbol{C}_{ \mathsf{g} }
        &
        \boldsymbol{0}_{ N,G }          \, \\
        \boldsymbol{0}_{ N,G }
        &
        \boldsymbol{C}_{ \mathsf{g} }   \, \\
    \end{matrix} \right]
    \boldsymbol{c}
    \, +
    \left[ \begin{matrix}
        \boldsymbol{C}_{ \mathsf{d} }
        &
        \boldsymbol{0}_{ N,D }          \, \\
        \boldsymbol{0}_{ N,D }
        &
        \boldsymbol{C}_{ \mathsf{d} }   \, \\
    \end{matrix} \right]
    \boldsymbol{d}
    \, .
\end{aligned}
$$ (s-c-d-2-phi)

The *reduced nodal residual vector* $\boldsymbol{\psi}$ is formed by simply dropping the components of
$\boldsymbol{\phi}$ corresponding to the slack bus, *i.e.*,

$$
\boldsymbol{\psi}
=
\left[ \begin{matrix}
    \boldsymbol{\phi}_{ N_{ \mathsf{s} } \, + \, 1 \, : \, N }    \, \\
    \boldsymbol{\phi}_{ N + N_{ \mathsf{s} } \, + \, 1 \, : }     \, \\
\end{matrix} \right]
\in \mathbb{R}^{ 2N - 2N_{ \mathsf{s} } }
\, .
$$ (phi-2-psi)

In other words, one can get
$\boldsymbol{\psi} \! \left( \boldsymbol{y}, \boldsymbol{c}, \boldsymbol{d} \right)$
by removing the first $N_{ \mathsf{s} }$
and the $\left( N + 1 \right)$-th through $\left( N + N_{ \mathsf{s} } \right)$-th elements of
$\boldsymbol{\phi} \! \left( \boldsymbol{y}, \boldsymbol{c}, \boldsymbol{d} \right)$.
The same procedure applies for getting
$\boldsymbol{\psi} \! \left( \boldsymbol{s}, \boldsymbol{c}, \boldsymbol{d} \right)$
from
$\boldsymbol{\phi} \! \left( \boldsymbol{s}, \boldsymbol{c}, \boldsymbol{d} \right)$.
