(apf-prc:solve-volts)=
# Solving for the bus voltages

At this stage, we have
the expected load draws $\boldsymbol{p}_{ \mathsf{d} } + j \boldsymbol{p}_{ \mathsf{d} }$
and the anticipated generator injections $\boldsymbol{p}_{ \mathsf{g} } + j \boldsymbol{p}_{ \mathsf{g} }$.
Forming the "factored" power flow equations {eq}`pf-eqns-fac` for the non-slack buses,
*i.e.*,

$$
\left\{ \begin{aligned}
    \boldsymbol{Z} \boldsymbol{y}
    & =
    \boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)
    \\
    \boldsymbol{C} \boldsymbol{x} & = \operatorname{ sigma } \! \left( \boldsymbol{y} \right)
\end{aligned} \right.
\, ,
$$ (pf-eqns-fac-reduced)

we solve for the corresponding system state variables $\boldsymbol{v}$ and $\boldsymbol{\delta}$
via the *factored Newton-Raphson*,
a method which was earlier introduced in
[*Factorized Load Flow*](https://doi.org/10.1109/TPWRS.2013.2265298)
and
[*Factored solution of nonlinear equation systems*](https://doi.org/10.1098/rspa.2014.0236).
The process is summarized in {prf:ref}`alg-solve-for-s`.

```{prf:algorithm} Solving for the bus voltages via the factored Newton-Raphson
:label: alg-solve-for-s

**Inputs**
$\widetilde{ \boldsymbol{s} }$,
$\boldsymbol{d}$, $\boldsymbol{c}$,
$\boldsymbol{C}$, $\boldsymbol{Z}$,
$\delta_{ \mathsf{ref} }$,
$\epsilon > 0$,
$K \in \mathbb{Z}_{ + }$

**Returns**
$\boldsymbol{v}$ and $\boldsymbol{\delta}$

1. Calculate $\boldsymbol{y}$ from $\widetilde{ \boldsymbol{s} }$,
   and $\boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$.
2. $k \gets 0$
3. $\boldsymbol{\psi} \gets \boldsymbol{Z} \boldsymbol{y} - \boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$
4. If $\lVert \boldsymbol{\psi} \rVert_{ \infty } \leq \epsilon$, go to Step 14.
5. Solve for $\widehat{ \boldsymbol{\mu} }$ in
   $\boldsymbol{Z} \boldsymbol{Z}^{ \mathsf{T} } \widehat{ \boldsymbol{\mu} } = \boldsymbol{\psi} $
6. $\widehat{ \boldsymbol{y} } \gets \boldsymbol{y} - \boldsymbol{Z}^{ \mathsf{T} } \widehat{ \boldsymbol{\mu} }$
7. Calculate $\partial_{ \boldsymbol{y} }^{ -1 } \operatorname{sigma} \! \left( \widehat{ \boldsymbol{y} } \right)$
8. Calculate $\boldsymbol{U} \! \left( \widehat{ \boldsymbol{y} } \right)$
9. $\boldsymbol{M} \gets \boldsymbol{C}^{ \mathsf{T} } \boldsymbol{U} \! \left( \widehat{ \boldsymbol{y} } \right)$,
   $\boldsymbol{N} \gets \boldsymbol{Z} \, \partial_{ \boldsymbol{y} }^{ -1 } \operatorname{sigma} \! \left( \widehat{ \boldsymbol{y} } \right)$,
   $\boldsymbol{H} \gets \boldsymbol{N} \boldsymbol{C}$,
   $\widehat{ \boldsymbol{u} } \gets \operatorname{sigma} \! \left( \widehat{ \boldsymbol{y} } \right)$
10. Calculate $\boldsymbol{x}$ by solving

    $$
        \left[ \begin{matrix}
            \boldsymbol{M} \boldsymbol{C} & \boldsymbol{H}^{ \mathsf{T} } \, \\
            \boldsymbol{H} & \boldsymbol{0} \,
        \end{matrix} \right]
        \left[ \begin{matrix}
            \boldsymbol{x} \, \\
            \boldsymbol{\mu} \,
        \end{matrix} \right]
        =
        \left[ \begin{matrix}
            \boldsymbol{M} \widehat{ \boldsymbol{u} } + \boldsymbol{H}^{ \mathsf{T} } \widehat{ \boldsymbol{\mu} }
            \, \\
            \boldsymbol{N} \widehat{ \boldsymbol{u} } \,
        \end{matrix} \right]
    $$

11. $k \gets k + 1$
12. $\boldsymbol{y} \gets \operatorname{arcsigma} \! \left( \boldsymbol{C} \boldsymbol{x} \right)$
13. $\boldsymbol{\psi} \gets \boldsymbol{Z} \boldsymbol{y} - \boldsymbol{z} \! \left( \boldsymbol{c}, \boldsymbol{d} \right)$,
    and go to Step 4
14. Calculate $\boldsymbol{v}$ and $\boldsymbol{\delta}$ using
    $\delta_{ \mathsf{ref} }$ and $\boldsymbol{x}$
```
