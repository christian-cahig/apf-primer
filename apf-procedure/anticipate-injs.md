(apf-prc:antic-injs)=
# Anticipating generator injections

We describe three methods for accomplishing the first stage of the APF procedure.
In the following, we use $\operatorname{ReLU} \! \left( \cdot \right)$ as the shorthand for the operation $\max \left( 0, \cdot \right)$ .
To avoid explicit summations and multiplying by $\boldsymbol{1}^{ \! \mathsf{T} }$,
we use $\operatorname{sum} \! \left( \cdot \right)$ to denote the sum of all the elements in an array.

(apf-prc:antic-injs:mol)=
## The method of Lateef

Let the snapshot data $\mathcal{S}$ contain
generator injections,
$\widetilde{ \boldsymbol{p} }_{ \mathsf{g} } + j \widetilde{ \boldsymbol{q} }_{ \mathsf{g} }$ ,
and
total system loss,
$\widetilde{ p }_{ \mathsf{l} } + j \widetilde{ q }_{ \mathsf{l} }$ .
Let the dispatch data $\mathcal{D}$ have
expected load draws,
$\boldsymbol{p}_{ \mathcal{d} } + j \boldsymbol{q}_{ \mathcal{d} }$ ,
and
scheduled bounds on generator injections,
$\overline{ \boldsymbol{p} }_{ \mathsf{g} } + j \overline{ \boldsymbol{q} }_{ \mathsf{g} }$ .

The change in total power draw is

```{math}
:label: mol-change-total-d
\begin{aligned}
    \Delta p_{ \mathsf{d} }
    & =
    \operatorname{sum} \! \left( \boldsymbol{p}_{ \mathsf{d} } \right)
    -
    \operatorname{sum} \! \left( \widetilde{ \boldsymbol{p} }_{ \mathsf{g} } \right)
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{d} }
    & =
    \operatorname{sum} \! \left( \boldsymbol{q}_{ \mathsf{d} } \right)
    -
    \operatorname{sum} \! \left( \widetilde{ \boldsymbol{q} }_{ \mathsf{g} } \right)
    +
    \widetilde{ q }_{ \mathsf{l} }
    \, .
\end{aligned}
```

The anticipated changes in the total generator injections can then be approximated *naively* as

```{math}
:label: mol-change-in-sum-c-naive
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & \approx
    \Delta p_{ \mathsf{d} }
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{g} }
    & \approx
    \Delta q_{ \mathsf{d} }
    +
    \widetilde{ q }_{ \mathsf{l} }
    \, ,
\end{aligned}
```

where the addition of the previous-snapshot loss acts as a heuristic buffer,
that is,
a change in total demand does not imply the same change in the required total supply.
A *conservative* approach would be to at least maintain the total supply, *i.e.*,

```{math}
:label: mol-change-in-sum-c-conserv
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & \approx
    \operatorname{ReLU} \! \left(
        \Delta p_{ \mathsf{d} }
        +
        \widetilde{ p }_{ \mathsf{l} }
    \right)
    \, , \\
    \Delta q_{ \mathsf{g} }
    & \approx
    \operatorname{ReLU} \! \left(
        \Delta q_{ \mathsf{d} }
        +
        \widetilde{ q }_{ \mathsf{l} }
    \right)
    \, .
\end{aligned}
```

A *strictly-increasing* flavour of {eq}`mol-change-in-sum-c-naive` can even be used when it is certain that total demand will increase:

```{math}
:label: mol-change-in-sum-c-strict
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & \approx
    \operatorname{ReLU} \! \left(
        \Delta p_{ \mathsf{d} }
    \right)
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{g} }
    & \approx
    \operatorname{ReLU} \! \left(
        \Delta q_{ \mathsf{d} }
    \right)
    +
    \widetilde{ q }_{ \mathsf{l} }
    \, .
\end{aligned}
```

Then the anticipated control variables are calculated as

```{math}
:label: mol-anticipated-c
\begin{aligned}
    \boldsymbol{p}_{ \mathsf{g} }
    & \approx
    \frac{ 
        \operatorname{sum} \! \left( \widetilde{ \boldsymbol{p} }_{ \mathsf{g} } \right)
        +
        \Delta p_{ \mathsf{g} }
    }{
        \operatorname{sum} \! \left( \overline{ \boldsymbol{p} }_{ \mathsf{g} } \right)
    }
    \thinspace
    \overline{\boldsymbol{p}}_{ \mathsf{g} }
    \, , \\
    \boldsymbol{q}_{ \mathsf{g} }
    & \approx
    \frac{
        \operatorname{sum} \! \left( \widetilde{ \boldsymbol{q} }_{ \mathsf{g} } \right)
        +
        \Delta q_{ \mathsf{g} }
    }{
        \operatorname{sum} \! \left( \overline{ \boldsymbol{q} }_{ \mathsf{g} } \right)
    }
    \thinspace
    \overline{\boldsymbol{q}}_{ \mathsf{g} }
    \, . 
\end{aligned}
```

Equation {eq}`mol-change-total-d` and the "distributing among the generator units in proportion to their scheduled capacities" in Equation {eq}`mol-anticipated-c`
are based on the experimental setup detailed in Section 2.4 of Omer Lateef's dissertation,
[*Measurement-Based Parameter Estimation and Analysis of Power System*](http://hdl.handle.net/1853/63600).

(apf-prc:antic-injs:moj)=
## The method of Jacobians

Suppose that the following are among the snapshot data $\mathcal{S}$:

* bus voltage magnitudes, $ \widetilde{ \boldsymbol{v} } $ ,
  and phase angles, $ \widetilde{ \boldsymbol{\delta} } $ ,
* load draws, $\widetilde{ \boldsymbol{p} }_{ \mathsf{d} } + j \widetilde{ \boldsymbol{q} }_{ \mathsf{d} }$ ,
* generator injections, $\widetilde{ \boldsymbol{p} }_{ \mathsf{g} } + j \widetilde{ \boldsymbol{q} }_{ \mathsf{g} }$ ,
  and
* total system loss, $\widetilde{ p }_{ \mathsf{l} } + j \widetilde{ q }_{ \mathsf{l} }$ .

As with {ref}`the method of Lateef<apf-prc:antic-injs:mol>`,
we assume that the
expected load draws,
$\boldsymbol{p}_{ \mathcal{d} } + j \boldsymbol{q}_{ \mathcal{d} }$ ,
and
the scheduled bounds on generator injections,
$\overline{ \boldsymbol{p} }_{ \mathsf{g} } + j \overline{ \boldsymbol{q} }_{ \mathsf{g} }$ ,
belong to the dispatch data $\mathcal{D}$.

Central to this method is the treatment of vector differences as approximate gradients.
Hence the change in the {ref}`bus-level aggregate of the load draws <prelim:control-vars-and-load-profile:bus-level>`
can be treated as the gradient w.r.t. $\boldsymbol{d}^{ \prime }$
of some scalar $\ell$ evaluated at the snapshot measurements, *i.e.*,

```{math}
:label: moj-change-d-prime
\nabla_{ \! \boldsymbol{d}^{ \prime } } \ell \! \left( 
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
=
\partial_{ \! \boldsymbol{d}^{ \prime } }^{ \mathsf{T} } \ell \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\approx
\Delta \boldsymbol{d}^{ \prime }
\triangleq
\boldsymbol{d}^{ \prime }
-
\widetilde{ \boldsymbol{d} }^{ \prime }
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{d} } \thinspace \boldsymbol{p}_{ \mathsf{d} } \, \\
    \boldsymbol{C}_{ \mathsf{d} } \thinspace \boldsymbol{q}_{ \mathsf{d} } \, \\
\end{matrix} \right]
-
\left[ \begin{matrix}
    \widetilde{ \boldsymbol{C} }_{ \mathsf{d} } \thinspace \widetilde{ \boldsymbol{p} }_{ \mathsf{d} } \, \\
    \widetilde{ \boldsymbol{C} }_{ \mathsf{d} } \thinspace \widetilde{ \boldsymbol{q} }_{ \mathsf{d} } \, \\
\end{matrix} \right]
\in \mathbb{R}^{2N} \, .
```

From the power flow equations, we can treat $\boldsymbol{s}$ as an implicitly defined function of $\boldsymbol{d}^{ \prime }$.
Then, by the chain rule, we have

$$
\nabla_{ \! \boldsymbol{d}^{ \prime } } \ell \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
=
\partial_{ \! \boldsymbol{d}^{ \prime } }^{ \mathsf{T} } \boldsymbol{s} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\,
\nabla_{ \! \boldsymbol{s} } \ell \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\, ,
$$

where the gradient w.r.t. $\boldsymbol{s}$ is treated as the approximate change in $\boldsymbol{s}$
in response to the change in $\boldsymbol{d}^{ \prime }$,
*i.e.*,
$\nabla_{ \! \boldsymbol{s} } \ell \! \left( \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} } \right) \approx \Delta \boldsymbol{s}$.
Hence, we solve the square linear system

```{math}
:label: moj-change-s
\partial_{ \! \boldsymbol{d}^{ \prime } }^{ \mathsf{T} } \boldsymbol{s} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\,
\Delta \boldsymbol{s}
=
\Delta \boldsymbol{d}^{\prime}
\, .
```

Now, the power flow equations also allow us to think of $\boldsymbol{c}$
as an implicitly defined function of $\boldsymbol{s}$.
We then have, by the chain rule,

$$
\nabla_{ \! \boldsymbol{c} } \ell \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
=
\partial_{ \! \boldsymbol{c} }^{ \mathsf{T} } \boldsymbol{s} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\,
\nabla_{ \! \boldsymbol{s} } \ell \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\, .
$$

As before, the gradient w.r.t. $\boldsymbol{c}$ is taken to be the approximate change in $\boldsymbol{c}$
in response to the change in $\boldsymbol{s}$,
leaving us with

```{math}
:label: moj-change-c
\Delta \boldsymbol{c}
=
\partial_{ \! \boldsymbol{c} }^{ \mathsf{T} } \boldsymbol{s} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\,
\Delta \boldsymbol{s}
\, .
```

We discuss the coefficient matrix in Equation {eq}`moj-change-s` and the matrix multiplier in Equation {eq}`moj-change-c` later.

The "effective" approximate changes in the total generator injections will have "contributions" from $\Delta \boldsymbol{c}$ and $\widetilde{ p }_{ \mathsf{l} } + j \widetilde{ q }_{ \mathsf{l} }$.
Then, in a manner similar to {ref}`the method of Lateef<apf-prc:antic-injs:mol>`,
the anticipated changes in total generator injections can be approximated *naively* as

```{math}
:label: moj-change-in-sum-c-naive
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & \approx
    \operatorname{sum} \! \left( \Delta \boldsymbol{p}_{ \mathsf{g} } \right)
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{g} }
    & \approx
    \operatorname{sum} \! \left( \Delta \boldsymbol{q}_{ \mathsf{g} } \right)
    +
    \widetilde{ q }_{ \mathsf{l} }
    \, , \\
\end{aligned}
```

*conservatively* as

```{math}
:label: moj-change-in-sum-c-conserv
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & \approx
    \operatorname{ ReLU } \! \left(
        \operatorname{sum} \! \left( \Delta \boldsymbol{p}_{ \mathsf{g} } \right)
        +
        \widetilde{ p }_{ \mathsf{l} }
    \right)
    \, , \\
    \Delta q_{ \mathsf{g} }
    & \approx
    \operatorname{ ReLU } \! \left(
        \operatorname{sum} \! \left( \Delta \boldsymbol{q}_{ \mathsf{g} } \right)
        +
        \widetilde{ q }_{ \mathsf{l} }
    \right)
    \, , \\
\end{aligned}
```

or in a *strictly increasing* manner as

```{math}
:label: moj-change-in-sum-c-strict
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & \approx
    \operatorname{ ReLU } \! \left(
        \operatorname{sum} \! \left( \Delta \boldsymbol{p}_{ \mathsf{g} } \right)
    \right)
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{g} }
    & \approx
    \operatorname{ ReLU } \! \left(
        \operatorname{sum} \! \left( \Delta \boldsymbol{q}_{ \mathsf{g} } \right)
    \right)
    +
    \widetilde{ q }_{ \mathsf{l} }
    \, , \\
\end{aligned}
```

where $\Delta \boldsymbol{p}_{ \mathsf{g} }$ and $\Delta \boldsymbol{q}_{ \mathsf{g} }$
are subvectors of $\Delta \boldsymbol{c}$ {eq}`pg-qg-2-c`.
Then the anticipated control variables are calculated as

```{math}
:label: moj-anticipated-c
\begin{aligned}
    \boldsymbol{p}_{ \mathsf{g} }
    & \approx
    \frac{ 
        \operatorname{sum} \! \left( \widetilde{ \boldsymbol{p} }_{ \mathsf{g} } \right)
        +
        \Delta p_{ \mathsf{g} }
    }{
        \operatorname{sum} \! \left( \overline{ \boldsymbol{p} }_{ \mathsf{g} } \right)
    }
    \thinspace
    \overline{\boldsymbol{p}}_{ \mathsf{g} }
    \, , \\
    \boldsymbol{q}_{ \mathsf{g} }
    & \approx
    \frac{
        \operatorname{sum} \! \left( \widetilde{ \boldsymbol{q} }_{ \mathsf{g} } \right)
        +
        \Delta q_{ \mathsf{g} }
    }{
        \operatorname{sum} \! \left( \overline{ \boldsymbol{q} }_{ \mathsf{g} } \right)
    }
    \thinspace
    \overline{\boldsymbol{q}}_{ \mathsf{g} }
    \, . 
\end{aligned}
```

To calculate the Jacobian of $\boldsymbol{s}$ w.r.t. $\boldsymbol{d}^{ \prime }$,
we can take the former to be an implicitly defined function solely of the latter
through the power flow equations, and invoke the implicit function theorem.
Evaluating this at the snapshot measurements, we have the linear matrix equation

$$
\partial_{ \boldsymbol{s} } \boldsymbol{\phi} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\,
\partial_{ \boldsymbol{d}^{ \prime } } \boldsymbol{s} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
=
-
\partial_{ \boldsymbol{d}^{ \prime } } \boldsymbol{\phi} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\, .
$$

From Equation {eq}`jac-phi-wrt-s`, the leftmost term is given by
$\partial_{ \boldsymbol{s} } \boldsymbol{e} \! \left( \widetilde{ \boldsymbol{s} } \right)$.
From Equation {eq}`d-prime` and {eq}`s-c-d-2-phi`, the Jacobian of $\boldsymbol{\phi}$
w.r.t. $\boldsymbol{d}^{ \prime }$ is $\boldsymbol{I}_{ 2N }$.
Therefore, the above expression simplifies to

$$
\partial_{ \boldsymbol{s} } \boldsymbol{e} \! \left( \widetilde{ \boldsymbol{s} } \right)
\,
\partial_{ \boldsymbol{d}^{ \prime } } \boldsymbol{s} \! \left( \widetilde{ \boldsymbol{s} } \right)
=
- \boldsymbol{I}_{ 2N }
\, ,
$$ (Jac-s-wrt-d-prime)

with which we can solve for
$\partial_{ \boldsymbol{d}^{ \prime } } \boldsymbol{s} \! \left( \widetilde{ \boldsymbol{s} } \right)$,
whose transpose is the desired coefficient matrix in Equation {eq}`moj-change-s`.

The Jacobian of $\boldsymbol{s}$ w.r.t. $\boldsymbol{c}$ also starts from interpreting the former
as an implicitly defined function solely of the latter, by virtue of the power flow equations.
Invoking the implicit function theorem and evaluating at the snapshot data,

$$
\partial_{ \boldsymbol{s} } \boldsymbol{\phi} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\,
\partial_{ \boldsymbol{c} } \boldsymbol{s} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
=
-
\partial_{ \boldsymbol{c} } \boldsymbol{\phi} \! \left(
    \widetilde{ \boldsymbol{c} }, \widetilde{ \boldsymbol{d} }, \widetilde{ \boldsymbol{s} }
\right)
\, .
$$

We now know that the leftmost term is
$\partial_{ \boldsymbol{s} } \boldsymbol{e} \! \left( \widetilde{ \boldsymbol{s} } \right)$.
From Equation {eq}`jac-phi-wrt-c`, the right-hand side term is
$\partial_{ \boldsymbol{c} } \boldsymbol{e}$.
The above linear matrix equation simplifies to

$$
\partial_{ \boldsymbol{s} } \boldsymbol{e} \! \left( \widetilde{ \boldsymbol{s} } \right)
\,
\partial_{ \boldsymbol{c} } \boldsymbol{s} \! \left( \widetilde{ \boldsymbol{s} } \right)
=
\partial_{ \boldsymbol{c} } \boldsymbol{e}
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
\, ,
$$ (Jac-s-wrt-c)

from which we solve for
$\partial_{ \boldsymbol{c} } \boldsymbol{s} \! \left( \widetilde{ \boldsymbol{s} } \right)$,
whose transpose is the multiplier for $\Delta \boldsymbol{s}$ in Equation {eq}`moj-change-c`.

(apf-prc:antic-injs:pfm)=
## The power factor method

Consider the case where a projected value of the total active generation,
$p_{ \mathsf{g} }$ , is avaialble (*e.g.*, by a scheduling routine).
The anticipated active injections can be estimated by distributing $p_{ \mathsf{g} }$
among the generator units in proportion to the units' scheduled limits,
*i.e.*,

```{math}
:label: pfm-anticipated-pg
\boldsymbol{p}_{ \mathsf{g} }
\approx
\frac{ p_{ \mathsf{g} } }{
    \operatorname{sum} \! \left( \overline{ \boldsymbol{p} }_{ \mathsf{g} } \right)
}
\thinspace
\overline{\boldsymbol{p}}_{ \mathsf{g} }
\, .
```

The anticipated total reactive generation $q_{ \mathsf{g} }$ is estimated with the use of
the aggregate power factor at the previous snapshot, *i.e.*,

```{math}
:label: pfm-anticipated-qg-sum
q_{ \mathsf{g} }
\approx
p_{ \mathsf{g} }
\tan \! \left( \operatorname{ arccos } \! \left( \operatorname{pf} \! \left(
    \operatorname{sum} \! \left(
        \widetilde{ \boldsymbol{p} }_{ \mathsf{g} }
        + j \widetilde{ \boldsymbol{q} }_{ \mathsf{g} }
    \right)
\right) \right) \right)
\, ,
```

where $\operatorname{sum} \! \left( \cdot \right)$ denotes the power factor of some complex power quantity.
Then the anticipated reactive injections are estimated as

```{math}
:label: pfm-anticipated-qg
\boldsymbol{q}_{ \mathsf{g} }
\approx
\frac{ q_{ \mathsf{g} } }{
    \operatorname{sum} \! \left( \overline{ \boldsymbol{q} }_{ \mathsf{g} } \right)
}
\thinspace
\overline{\boldsymbol{q}}_{ \mathsf{g} }
\, .
```
