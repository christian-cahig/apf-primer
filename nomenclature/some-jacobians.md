(nom:some-jacobians)=
# Some Jacobians

As a notational convenience, we will use
$\partial_{ \boldsymbol{w} } \boldsymbol{f}$
to denote the Jacobian of some $L$-vector $\boldsymbol{f}$ with respect to (w.r.t.)
some other $M$-vector $\boldsymbol{w}$.
In general, $\partial_{ \boldsymbol{w} } \boldsymbol{f}$ is an $L \times M$ matrix.
Its transpose will be written as
$\partial_{ \boldsymbol{w} }^{ \mathsf{T} } \boldsymbol{f}$,
its inverse as
$\partial_{ \boldsymbol{w} }^{-1} \boldsymbol{f}$,
and the transpose of its inverse as
$\partial_{ \boldsymbol{w} }^{ -\mathsf{T} } \boldsymbol{f}$.
Appending $\left( \boldsymbol{t} \right)$ evokes that the Jacobian, its transpose, its inverse, or the transpose of its inverse,
is evaluated at some quantity $\boldsymbol{t}$.

## Jacobians of $\boldsymbol{\phi}$

Consider
$\boldsymbol{\phi} \! \left( \boldsymbol{s}, \boldsymbol{c}, \boldsymbol{d} \right)$
from {ref}`nom:pow-flow-eqns` :

$$
\boldsymbol{\phi} \! \left( \boldsymbol{s}, \boldsymbol{c}, \boldsymbol{d} \right)
\triangleq
\boldsymbol{e} \! \left( \boldsymbol{s} \right)
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
$$

Then, the Jacobians w.r.t. $\boldsymbol{s}$ of $\boldsymbol{\phi}$ and of $\boldsymbol{e}$ are equal for any $\boldsymbol{s}$:

$$
\partial_{ \boldsymbol{s} } \boldsymbol{\phi} \! \left( \boldsymbol{s} \right)
=
\partial_{ \boldsymbol{s} } \boldsymbol{e} \! \left( \boldsymbol{s} \right)
\in \mathbb{R}^{ 2N \times 2N } \, .
$$

From Equations (27) through (32) in Secion 4.2.1 of
[*MATPOWER Technical Note 2 Revision 2*](https://matpower.org/docs/TN2-OPF-Derivatives.pdf),
$\partial_{ \boldsymbol{s} } \boldsymbol{e} \! \left( \boldsymbol{s} \right)$
can be computed given $\boldsymbol{s}$ and $\mathbf{Y}$,
and is therefore possibly sparse.
