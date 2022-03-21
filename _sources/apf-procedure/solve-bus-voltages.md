(apf-prc:solve-volts)=
# Solving for the bus voltages

At this stage, we have
the expected load draws $\boldsymbol{p}_{ \mathsf{d} } + j \boldsymbol{q}_{ \mathsf{d} }$
and the anticipated generator injections $\boldsymbol{p}_{ \mathsf{g} } + j \boldsymbol{q}_{ \mathsf{g} }$.
Forming the "factored" power flow equations {eq}`pf-eqns-fac` for the non-slack buses,
*i.e.*,

$$
\left\{ \begin{aligned}
    \boldsymbol{Z} \boldsymbol{y}
    & =
    \boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)
    \\
    \boldsymbol{W} \boldsymbol{w} & = \operatorname{ sigma } \! \left( \boldsymbol{y} \right)
\end{aligned} \right.
\, ,
$$ (pf-eqns-fac-reduced)

we solve for the corresponding system state variables $\boldsymbol{v}$ and $\boldsymbol{\delta}$
via the *factored Newton-Raphson*,
a method which was introduced in prior works
[*Factorized Load Flow*](https://doi.org/10.1109/TPWRS.2013.2265298)
and
[*Factored solution of nonlinear equation systems*](https://doi.org/10.1098/rspa.2014.0236).
The process is summarized in {prf:ref}`alg-solve-for-s`.

```{prf:algorithm} Solving for the bus voltages via the factored Newton-Raphson
:label: alg-solve-for-s

**Inputs**
$\widetilde{ \boldsymbol{v} }$, $\widetilde{ \boldsymbol{\delta} }$,
$\boldsymbol{d}$, $\boldsymbol{c}$,
$\boldsymbol{W}$, $\boldsymbol{Z}$,
$\delta_{ \mathsf{ref} }$,
$\epsilon > 0$

**Returns**
$\boldsymbol{v}$ and $\boldsymbol{\delta}$

1. Calculate $\boldsymbol{y}$ from $\widetilde{ \boldsymbol{v} }$ and $\widetilde{ \boldsymbol{\delta} }$.
2. Calculate $\boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$.
3. $\boldsymbol{\psi} \gets \boldsymbol{Z} \boldsymbol{y} - \boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$
4. If $\lVert \boldsymbol{\psi} \rVert_{ \infty } \leq \epsilon$, go to Step 14.
5. Solve for $\widehat{ \boldsymbol{\mu} }$ in
   $\boldsymbol{Z} \boldsymbol{Z}^{ \mathsf{T} } \widehat{ \boldsymbol{\mu} } = \boldsymbol{\psi} $.
6. $\widehat{ \boldsymbol{y} } \gets \boldsymbol{y} - \boldsymbol{Z}^{ \mathsf{T} } \widehat{ \boldsymbol{\mu} }$
7. Calculate $\partial_{ \boldsymbol{y} }^{ -1 } \operatorname{sigma} \! \left( \widehat{ \boldsymbol{y} } \right)$.
8. Calculate $\boldsymbol{U} \! \left( \widehat{ \boldsymbol{y} } \right)$.
9. $\boldsymbol{M} \gets \boldsymbol{W}^{ \mathsf{T} } \thinspace \boldsymbol{U} \! \left( \widehat{ \boldsymbol{y} } \right)$,
   $\boldsymbol{N} \gets \boldsymbol{Z} \, \partial_{ \boldsymbol{y} }^{ -1 } \operatorname{sigma} \! \left( \widehat{ \boldsymbol{y} } \right)$,
   $\boldsymbol{H} \gets \boldsymbol{N} \boldsymbol{W}$,
   $\widehat{ \boldsymbol{u} } \gets \operatorname{sigma} \! \left( \widehat{ \boldsymbol{y} } \right)$
10. Solve for $\boldsymbol{m}$ in

    $$
    \left[ \begin{matrix}
        \boldsymbol{M} \boldsymbol{W} & \boldsymbol{H}^{ \mathsf{T} } \, \\
        \boldsymbol{H} & \boldsymbol{0} \,
    \end{matrix} \right]
    \left[ \begin{matrix}
        \boldsymbol{w} \\
        \boldsymbol{\mu}
    \end{matrix} \right]
    =
    \left[ \begin{matrix}
        \boldsymbol{M} \boldsymbol{W} & \boldsymbol{H}^{ \mathsf{T} } \, \\
        \boldsymbol{H} & \boldsymbol{0} \,
    \end{matrix} \right]
    \boldsymbol{m}
    =
    \left[ \begin{matrix}
        \boldsymbol{M} \widehat{ \boldsymbol{u} } + \boldsymbol{H}^{ \mathsf{T} } \widehat{ \boldsymbol{\mu} }
        \, \\
        \boldsymbol{N} \widehat{ \boldsymbol{u} } \,
    \end{matrix} \right]
    .
    $$

11. Get $\boldsymbol{w}$ from $\boldsymbol{m}$.
12. $\boldsymbol{y} \gets \operatorname{arcsigma} \! \left( \boldsymbol{W} \boldsymbol{w} \right)$
13. $\boldsymbol{\psi} \gets \boldsymbol{Z} \boldsymbol{y} - \boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$,
    and go to Step 4.
14. Calculate $\boldsymbol{v}$ and $\boldsymbol{\delta}$ using
    $\delta_{ \mathsf{ref} }$ and $\boldsymbol{w}$.
```

Recall that the quantities
$\widetilde{ \boldsymbol{v} }$ and $\widetilde{ \boldsymbol{\delta} }$
are snapshot data,
while
$\boldsymbol{ W }$, $\boldsymbol{ Z }$, and $\boldsymbol{ d }$
are dispatch data.
Here $\boldsymbol{c}$ refers to the anticipated generator injections.
The reference-bus phase angle $\delta_{ \mathsf{ref} }$ is usually set to zero.

There are a few things with which we can leverage for efficiency in implementation.
First, $\boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$ only needs to be computed once.
Second, since the coefficient matrix $\boldsymbol{Z} \boldsymbol{Z}^{ \mathsf{T} }$ of the linear system in Step 5 is fixed,
its Cholesky factor can be computed once and then reused in the succeeding iterations.
Third, due to their {ref}`regular sparsity patterns <prelim:some-jacobians:jac-of-u>`,
we can set up the data structures for $\partial_{ \boldsymbol{y} }^{ -1 } \operatorname{sigma}$ and $\boldsymbol{U}$
once at initialization,
and Steps 7 and 8 amount to simply updating the attributes corresponding to the nonzero elements.
