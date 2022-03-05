(prelim:net-nodal-injs)=
# Net nodal injections

The *net nodal injections* refer to two $N$-vectors:
the vector of net active power injections at the buses,
$\boldsymbol{p}$,
and that of the corresponding net reactive injections,
$\boldsymbol{q}$.
In code, we use `p` and `q` as the respective variable names for
$\boldsymbol{p}$ and $\boldsymbol{q}$.

(prelim:net-nodal-injs:e-z)=
## Net nodal injection vectors

Concatenating $\boldsymbol{p}$ and $\boldsymbol{q}$ gives
the *full net nodal injection vector* $\boldsymbol{e}$
*i.e.*,

$$
\boldsymbol{e}
= \left[ \begin{matrix}
    \boldsymbol{p}  \, \\
    \boldsymbol{q}  \, \\
\end{matrix} \right]
\in \mathbb{R}^{2N}
\, .
$$

Removing from $\boldsymbol{e}$ the net active and reactive injections at the slack buses yields
the *reduced net nodal injection vector* $\boldsymbol{z}$,
*i.e.*,

$$
\boldsymbol{z}
=
\left[ \begin{matrix}
    \boldsymbol{e}_{ N_\mathsf{s} \, +\, 1 \, : \, N }      \, \\
    \boldsymbol{e}_{ N + N_\mathsf{s} \, +\, 1 \, : }       \, \\
\end{matrix} \right]
=
\left[ \begin{matrix}
    \boldsymbol{p}_{ \mathsf{v} }     \, \\
    \boldsymbol{p}_{ \mathsf{q} }     \, \\
    \boldsymbol{q}_{ \mathsf{v} }     \, \\
    \boldsymbol{q}_{ \mathsf{q} }     \, \\
\end{matrix} \right]
\in \mathbb{R}^{ 2N - 2N_{ \mathsf{s} } }
\, ,
$$

following {ref}`the partitioning scheme <prelim:bus-indexing:vecs-n>` for $N$-vectors.
We reserve the variable names `e` and `z` for $\boldsymbol{e}$ and for $\boldsymbol{z}$, respectively.

(prelim:net-nodal-injs:ez-from-cd)=
## Calculating $\boldsymbol{e}$ from $\boldsymbol{c}$ and $\boldsymbol{d}$

We can compute the net power injections at the buses directly
as the algebraic sum of the generator-induced injections and the load-induced draws at the buses, *i.e.*,

$$
\boldsymbol{p} + j \boldsymbol{q}
=
\left(
    \boldsymbol{C}_{ \mathsf{g} } \, \boldsymbol{p}_{ \mathsf{g} }
    -
    \boldsymbol{C}_{ \mathsf{d} } \, \boldsymbol{p}_{ \mathsf{d} }
\right)
+
j
\left(
    \boldsymbol{C}_{ \mathsf{g} } \, \boldsymbol{q}_{ \mathsf{g} }
    -
    \boldsymbol{C}_{ \mathsf{d} } \, \boldsymbol{q}_{ \mathsf{d} }
\right)
\, ,
$$

or, in terms of $\boldsymbol{c}$, and $\boldsymbol{d}$,

$$
\boldsymbol{e} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)
=
\boldsymbol{c}^{\prime} - \boldsymbol{d}^{\prime}
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{g} }
    &
    \boldsymbol{0}_{ N,G }          \, \\
    \boldsymbol{0}_{ N,G }
    &
    \boldsymbol{C}_{ \mathsf{g} }   \, \\
\end{matrix} \right]
\boldsymbol{c}
\, -
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{d} }
    &
    \boldsymbol{0}_{ N,D }          \, \\
    \boldsymbol{0}_{ N,D }
    &
    \boldsymbol{C}_{ \mathsf{d} }   \, \\
\end{matrix} \right]
\boldsymbol{d}
=
\boldsymbol{C}_{ \mathsf{G} } \, \boldsymbol{c}
-
\boldsymbol{C}_{ \mathsf{D} } \, \boldsymbol{d}
\, .
$$

To get $\boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$,
simply drop the first $N_{ \mathsf{s} }$
and the $\left( N + 1 \right)$-th through $\left( N + N_{ \mathsf{s} } \right)$-th
elements of $\boldsymbol{e} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$.

(prelim:net-nodal-injs:ez-from-s)=
## Calculating $\boldsymbol{e}$ from $\boldsymbol{v}$ and $\boldsymbol{\delta}$

The net nodal injections can also be computed from the system state variables through the system bus admittance matrix,
*i.e.*,

$$
\boldsymbol{p} + j \boldsymbol{q}
=
\operatorname{ diag } \! \left( \mathbf{v} \right)
\operatorname{ conj } \! \left( \mathbf{Y} \mathbf{v} \right)
\,
$$

where
$\mathbf{v} = \operatorname{ diag } \! \left( \boldsymbol{v} \right) \exp \! \left( j \boldsymbol{\delta} \right) \in \mathbb{C}^{ 2N }$
is the vector of bus voltage phasors,
and
$\operatorname{ conj } \! \left( \cdot \right)$
denotes point-wise complex conjugation.
Equivalently, in terms of $\boldsymbol{G}$, $\boldsymbol{B}$, $\boldsymbol{v}$, and $\boldsymbol{\delta}$,

$$
\begin{aligned}
    \boldsymbol{p}
    & =
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
    \, , \\
    \boldsymbol{q}
    & =
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
    \, ,
\end{aligned}
$$

with
$\boldsymbol{v}_{ \mathsf{r} } = \operatorname{ diag } \! \left( \boldsymbol{v} \right) \cos \! \left( \boldsymbol{\delta} \right)$
and
$\boldsymbol{v}_{ \mathsf{i} } = \operatorname{ diag } \! \left( \boldsymbol{v} \right) \sin \! \left( \boldsymbol{\delta} \right)$.
Hence, we have

$$
\boldsymbol{e} \! \left( \boldsymbol{v}, \boldsymbol{\delta} \right)
=
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
\, .
$$

To get $\boldsymbol{z} \! \left( \boldsymbol{v}, \boldsymbol{\delta} \right)$,
simply drop the first $N_{ \mathsf{s} }$
and the $\left( N + 1 \right)$-th through $\left( N + N_{ \mathsf{s} } \right)$-th
elements of $\boldsymbol{e} \! \left( \boldsymbol{v}, \boldsymbol{\delta} \right)$.

(prelim:net-nodal-injs:ez-from-y)=
## Calculating $\boldsymbol{e}$ from $\boldsymbol{y}$

With judicious algebraic manipulation,
the net nodal injections are a linear function of $\boldsymbol{y}$[^about-E],
*i.e.*,

$$
\boldsymbol{e} \! \left( \boldsymbol{y} \right) = \boldsymbol{E} \boldsymbol{y} \, .
$$

Neglecting the slack-nodal injections,
we can express $\boldsymbol{z}$ as a linear function of $\boldsymbol{y}$, too:

$$
\boldsymbol{z} \! \left( \boldsymbol{y} \right) = \boldsymbol{Z} \boldsymbol{y} \, ,
$$

which is equivalent to dropping the first $N_{ \mathsf{s} }$
and the $\left( N + 1 \right)$-th through $\left( N + N_{ \mathsf{s} } \right)$-th
elements of $\boldsymbol{e} \! \left( \boldsymbol{y} \right)$.

[^about-E]: As a hint, start with the bus injection model
(*e.g.*, see Section 5.1 of
[*An introduction to optimal power flow: Theory, formulation, and examples*](https://doi.org/10.1080/0740817X.2016.1189626)),
and try to transform the explicit summations into a matrix-vector expression.
