(prelim:some-jacobians)=
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

Consider Equations {eq}`y-c-d-2-phi` and {eq}`s-c-d-2-phi` :

$$
\boldsymbol{\phi} \! \left( \boldsymbol{y}, \boldsymbol{c}, \boldsymbol{d} \right)
\triangleq
\boldsymbol{E} \boldsymbol{y}
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
\, ,
$$

$$
\boldsymbol{\phi} \! \left( \boldsymbol{s}, \boldsymbol{c}, \boldsymbol{d} \right)
\triangleq
\underbrace{
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
}_{
    \boldsymbol{e} \left( \boldsymbol{s} \right)
}
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

```{margin}
For the full expression of
$\partial_{ \boldsymbol{s} } \boldsymbol{e} \! \left( \boldsymbol{s} \right)$,
see Equations (27) through (32) in Secion 4.2.1 of
[*MATPOWER Technical Note 2 Revision 2*](https://matpower.org/docs/TN2-OPF-Derivatives.pdf).
```

The Jacobian of $\boldsymbol{\phi}$ w.r.t. $\boldsymbol{s}$ is as the Jacobian of $\boldsymbol{e}$ w.r.t. $\boldsymbol{s}$,
where the latter can be computed given $\boldsymbol{s}$.
Hence, we have

```{math}
:label: jac-phi-wrt-s
\partial_{ \boldsymbol{s} } \boldsymbol{\phi} \! \left( \boldsymbol{s} \right)
=
\partial_{ \boldsymbol{s} } \boldsymbol{e} \! \left( \boldsymbol{s} \right)
\in \mathbb{R}^{ 2N \times 2N } \, .
```

In code, we use `de_ds` for both $\partial_{ \boldsymbol{s} } \boldsymbol{e}$
and $\partial_{ \boldsymbol{s} } \boldsymbol{\phi}$.

The Jacobian of $\boldsymbol{\phi}$ w.r.t. $\boldsymbol{c}$ is the negative of the Jacobian of $\boldsymbol{e}$ w.r.t. $\boldsymbol{c}$,
*i.e.*,

```{math}
:label: jac-phi-wrt-c
\partial_{ \boldsymbol{c} } \boldsymbol{\phi}
=
- \partial_{ \boldsymbol{c} } \boldsymbol{e}
=
- \!
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{g} }
    &
    \boldsymbol{0}_{ N,G }          \, \\
    \boldsymbol{0}_{ N,G }
    &
    \boldsymbol{C}_{ \mathsf{g} }   \, \\
\end{matrix} \right]
\in \mathbb{R}^{ 2N \times 2G } \, .
```

In code, we use `de_dc` for $\partial_{ \boldsymbol{c} } \boldsymbol{e}$,
*i.e.*,
`-de_dc` refers to $\partial_{ \boldsymbol{c} } \boldsymbol{\phi}$.

Similarly, the Jacobian of $\boldsymbol{\phi}$ w.r.t. $\boldsymbol{d}$ is the negative of the Jacobian of $\boldsymbol{e}$ w.r.t. $\boldsymbol{d}$,
*i.e.*,

```{math}
:label: jac-phi-wrt-d
\partial_{ \boldsymbol{d} } \boldsymbol{\phi}
=
- \partial_{ \boldsymbol{d} } \boldsymbol{e}
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{d} }
    &
    \boldsymbol{0}_{ N,D }          \, \\
    \boldsymbol{0}_{ N,D }
    &
    \boldsymbol{C}_{ \mathsf{d} }   \, \\
\end{matrix} \right]
\in \mathbb{R}^{ 2N \times 2D } \, .
```

In code, we use `de_dd` for $\partial_{ \boldsymbol{d} } \boldsymbol{e}$,
*i.e.*,
`-de_dd` refers to $\partial_{ \boldsymbol{d} } \boldsymbol{\phi}$.

(prelim:some-jacobians:jac-of-u)=
## Jacobians of $\boldsymbol{u}$

Let
$\partial_{ \boldsymbol{y} } \boldsymbol{u} \! \left( \boldsymbol{y} \right) = \partial_{ \boldsymbol{y} } \operatorname{sigma} \! \left( \boldsymbol{y} \right) \in \mathbb{R}^{ \left( N + 2E \right) \times \left( N + 2E \right) }$
be the Jacobian of
$\boldsymbol{u} = \operatorname{sigma} \! \left( \boldsymbol{y} \right)$
w.r.t. $\boldsymbol{y}$, evaluated at some $\boldsymbol{y}$.
{ref}`The definitions <prelim:state-vars:y-u>` of $\boldsymbol{u}$ and $\boldsymbol{y}$
give $\partial_{ \boldsymbol{y} } \operatorname{sigma} \! \left( \boldsymbol{y} \right)$
a very nice sparsity structure:
it has $N$ diagonal elements corresponding to the $\boldsymbol{a}$-to-$\boldsymbol{\alpha}$ map,
*i.e.*,

```{math}
:label: jac-alp-wrt-a
\frac{\partial \alpha_{i}}{\partial a_{i}} = \frac{1}{a_{i}},
\qquad \qquad i = 1, 2, \ldots, N
\, ,
```

and
a block diagonal of $E$ $2 \times 2$ matrices
corresponding to the map transforming $\boldsymbol{k}$ and $\boldsymbol{r}$ to
$\boldsymbol{\kappa}$ and $\boldsymbol{\rho}$,
*i.e.*,

```{math}
:label: jac-kaprho-wrt-kr
\left[ \begin{matrix}
    \dfrac{\partial \kappa_{i}}{\partial k_{i}}
    &
    \dfrac{\partial \kappa_{i}}{\partial r_{i}}
    \\[1.1em]
    \dfrac{\partial \rho_{i}}{\partial k_{i}}
    &
    \dfrac{\partial \rho_{i}}{\partial r_{i}}
    \\[0.74em]
\end{matrix} \right]
=
\frac{1}{k^{2} + r^{2}}
\left[ \begin{matrix}
    2 k_{i} & 2 r_{i} \\
    -r_{i} & k_{i}    \\
\end{matrix} \right],
\qquad \qquad i = 1, 2, \ldots, E
\, .
```

Its inverse,
$\partial_{ \boldsymbol{y} }^{-1} \boldsymbol{u} \! \left( \boldsymbol{y} \right) = \partial_{ \boldsymbol{y} }^{-1} \operatorname{sigma} \! \left( \boldsymbol{y} \right) \in \mathbb{R}^{ \left( N + 2E \right) \times \left( N + 2E \right) }$,
therefore inherists the same sparsity structure
consisting of $N$ diagonal elements, *i.e.*,

```{math}
:label: inv-jac-alp-wrt-a
\left(
    \frac{\partial \alpha_{i}}{\partial a_{i}}
\right)^{\!-1} = a_{i},
\qquad \qquad i = 1, 2, \ldots, N
\, ,
```

and a block diagonal of $E$ $2 \times 2$ matrices, *i.e.*,

```{math}
:label: inv-jac-kaprho-wrt-kr
\left[ \begin{matrix}
    \dfrac{\partial \kappa_{i}}{\partial k_{i}}
    &
    \dfrac{\partial \kappa_{i}}{\partial r_{i}}
    \\[1.1em]
    \dfrac{\partial \rho_{i}}{\partial k_{i}}
    &
    \dfrac{\partial \rho_{i}}{\partial r_{i}}
    \\[0.74em]
\end{matrix} \right]^{-1}
=
\frac{1}{2}
\left[ \begin{matrix}
    k_{i} & -2r_{i}
    \\
    r_{i} & 2k_{i}
\end{matrix} \right],
\qquad \qquad i = 1, 2, \ldots, E \, .
```

For instance, with $N = 5$ and $E = 3$,
both
$\partial_{ \boldsymbol{y} } \boldsymbol{u}$
and
$\partial_{ \boldsymbol{y} }^{-1} \boldsymbol{u}$
will have the form

$$
\left[ \begin{matrix}
    \bullet & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
    0 & \bullet & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
    0 & 0 & \bullet & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
    0 & 0 & 0 & \bullet & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 & \bullet & 0 & 0 & 0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 & 0 & \bullet & \bullet & 0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 & 0 & \bullet & \bullet & 0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 & 0 & 0 & 0 & \bullet & \bullet & 0 & 0 \\
    0 & 0 & 0 & 0 & 0 & 0 & 0 & \bullet & \bullet & 0 & 0 \\
    0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \bullet & \bullet \\
    0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & \bullet & \bullet \\
\end{matrix} \right]
$$

In code, we use `dudy` and `inv_dudy` for
$\partial_{ \boldsymbol{y} } \boldsymbol{u}$
and
$\partial_{ \boldsymbol{y} }^{-1} \boldsymbol{u}$,
respectively.

We define
$\boldsymbol{U} \! \left( \boldsymbol{y} \right)$
as the product of the transpose of
$\partial_{ \boldsymbol{y} }^{-1} \operatorname{sigma} \! \left( \boldsymbol{y} \right)$
and itself, that is,

```{math}
:label: U
\boldsymbol{U} \! \left( \boldsymbol{y} \right)
=
\partial_{ \boldsymbol{y} }^{-\mathsf{T}} \operatorname{sigma} \! \left( \boldsymbol{y} \right)
\thinspace \thinspace
\partial_{ \boldsymbol{y} }^{-1} \operatorname{sigma} \! \left( \boldsymbol{y} \right)
\in \mathbb{R}^{ \left( N + 2E \right) \times \left( N + 2E \right) }.
```

It is straightforward to show that
$\boldsymbol{U} \! \left( \boldsymbol{y} \right)$
is diagonal and positive,
where the non-zero entries are given by the concatenation of
$\operatorname{diag} \! \left( \boldsymbol{a} \right) \, \boldsymbol{a}$
and the interweaving of
$\frac{1}{4} \left[ \operatorname{diag} \! \left( \boldsymbol{k} \right) \, \boldsymbol{k} + \operatorname{diag} \! \left( \boldsymbol{r} \right) \, \boldsymbol{r} \right]$
and
$\operatorname{diag} \! \left( \boldsymbol{k} \right) \, \boldsymbol{k} + \operatorname{diag} \! \left( \boldsymbol{r} \right) \, \boldsymbol{r}$.
Hence, with $N = 3$ and $E = 2$ ,

$$
\boldsymbol{U} \! \left( \boldsymbol{y} \right)
=
\operatorname{ diag } \! \left(
    \left[ \begin{matrix}
        a_{1}^{2} ,\, a_{2}^{2} ,\, a_{3}^{2} ,\,
        \dfrac{1}{4} \left( k_{1}^{2} + r_{1}^{2} \right) ,\,
        k_{1}^{2} + r_{1}^{2} ,\,
        \dfrac{1}{4} \left( k_{2}^{2} + r_{2}^{2} \right) ,\,
        k_{2}^{2} + r_{2}^{2} \,
    \end{matrix} \right]^{ \mathsf{T} }
\right)
\, .
$$

In code, we use `U` for $\boldsymbol{U} \! \left( \boldsymbol{y} \right)$.
