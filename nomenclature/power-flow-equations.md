(nom:pow-flow-eqns)=
# Power flow equations

The *power flow equations* embody the nodal power balance requirements[^about-bim],
that is,
the value of $\boldsymbol{p} + j \boldsymbol{q}$
calculated from $\boldsymbol{v}$ and $\boldsymbol{\delta}$
must match that calculated from
$\boldsymbol{p}_{ \mathsf{g} } + j \boldsymbol{q}_{ \mathsf{g} }$
and
$\boldsymbol{p}_{ \mathsf{d} } + j \boldsymbol{q}_{ \mathsf{d} }$ :

$$
\begin{aligned}
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
    & =
    \boldsymbol{C}_{ \mathsf{g} } \, \boldsymbol{p}_{ \mathsf{g} }
    -
    \boldsymbol{C}_{ \mathsf{d} } \, \boldsymbol{p}_{ \mathsf{d} }
    \, ,  \\
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
    & =
    \boldsymbol{C}_{ \mathsf{g} } \, \boldsymbol{q}_{ \mathsf{g} }
    -
    \boldsymbol{C}_{ \mathsf{d} } \, \boldsymbol{q}_{ \mathsf{d} }
    \, .
\end{aligned}
$$

The *factored formulation* of the power flow equations restates the above nonlinear expression
into two linear systems that are coupled by an elementary nonlinear map[^about-fac-pfe]:

$$
\left\{ \begin{aligned}
    \boldsymbol{E} \boldsymbol{y} & = \boldsymbol{e} \\
    \boldsymbol{C} \boldsymbol{x} & = \operatorname{ sigma } \! \left( \boldsymbol{y} \right)
\end{aligned} \right.
\, .
$$

[^about-bim]: Strictly speaking, this is the "bus injection model" of the power flow equations.
Another prominent (equivalent) formulation is the "branch flow model"
(see Section 5.2 of
[*An introduction to optimal power flow: Theory, formulation, and examples*](https://doi.org/10.1080/0740817X.2016.1189626)).

[^about-fac-pfe]: The idea is based on Equations (27) and (28) of
[*Factorized Load Flow*](https://doi.org/10.1109/TPWRS.2013.2265298).

The *nodal power residuals* are the 'mismatches' between
the values of $\boldsymbol{e}$ calculated from $\boldsymbol{y}$
and that from $\boldsymbol{c}$ and $\boldsymbol{d}$.
We define the *full nodal residual vector* $\boldsymbol{\phi}$ as

$$
\boldsymbol{\phi}
\triangleq
\boldsymbol{E} \boldsymbol{y} - \boldsymbol{e} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)
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
\, .
$$

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
\, ,
$$

which can be less informally defined as

$$
\boldsymbol{\psi}
\triangleq
\boldsymbol{Z} \boldsymbol{y} - \boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)
=
\boldsymbol{Z} \boldsymbol{y}
\, -
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{g}, \mathsf{v} }
    &
    \boldsymbol{0}_{ N_{ \mathsf{v} }, G }
    \, \\
    \boldsymbol{C}_{ \mathsf{g}, \mathsf{q} }
    &
    \boldsymbol{0}_{ N_{ \mathsf{q} }, G }
    \, \\
    \boldsymbol{0}_{ N_{ \mathsf{v} }, G }
    &
    \boldsymbol{C}_{ \mathsf{g}, \mathsf{v} }
    \, \\
    \boldsymbol{0}_{ N_{ \mathsf{q} }, G }
    &
    \boldsymbol{C}_{ \mathsf{g}, \mathsf{q} }
    \, \\
\end{matrix} \right]
\boldsymbol{c}
\, +
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{d}, \mathsf{v} }
    &
    \boldsymbol{0}_{ N_{ \mathsf{v} }, D }
    \, \\
    \boldsymbol{C}_{ \mathsf{d}, \mathsf{q} }
    &
    \boldsymbol{0}_{ N_{ \mathsf{q} }, D }
    \, \\
    \boldsymbol{0}_{ N_{ \mathsf{v} }, D }
    &
    \boldsymbol{C}_{ \mathsf{d}, \mathsf{v} }
    \, \\
    \boldsymbol{0}_{ N_{ \mathsf{q} }, D }
    &
    \boldsymbol{C}_{ \mathsf{d}, \mathsf{q} }
    \, \\
\end{matrix} \right]
\boldsymbol{d}
\, \, \in \mathbb{R}^{ 2N - 2N_{ \mathsf{s} } }
\, ,
$$

following {ref}`the partitioning scheme <nom:bus-indexing:mats-nr>`
for matrices with $N$ rows.
