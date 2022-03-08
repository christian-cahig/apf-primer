(apf-prc:antic-injs)=
# Anticipating generator injections

We describe two methods for accomplishing the first stage of the APF procedure.
One method is a simple algebraic procedure that calculates a change in total generator injection,
and distributes such aggregate change among the individual units according to their scheduled bounds.
The other method is more involved as it involves computing first-order information to approximate sensitivities.

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

Define
$p_{ \mathsf{d} } + j q_{ \mathsf{d} } \triangleq \boldsymbol{1}^{ \! \mathsf{T} } \! \left( \boldsymbol{p}_{ \mathsf{d} } + j \boldsymbol{q}_{ \mathsf{d} } \right)$
and
$\widetilde{ p }_{ \mathsf{g} } + j \widetilde{ q }_{ \mathsf{g} } \triangleq \boldsymbol{1}^{ \! \mathsf{T} } \! \left( \widetilde{ \boldsymbol{p} }_{ \mathsf{g} } + j \widetilde{ \boldsymbol{q} }_{ \mathsf{g} } \right)$ .
The change in total power draw is

```{math}
:label: mol-change-total-d
\begin{aligned}
    \Delta p_{ \mathsf{d} }
    & =
    p_{ \mathsf{d} } - \widetilde{ p }_{ \mathsf{g} } + \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{d} }
    & =
    q_{ \mathsf{d} } - \widetilde{ q }_{ \mathsf{g} } + \widetilde{ q }_{ \mathsf{l} }
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
    \, ,
\end{aligned}
```

where
$\operatorname{ReLU} \! \left( \cdot \right) \triangleq \max \left( 0, \cdot \right)$.
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

Let $\overline{ \boldsymbol{p} }_{ \mathsf{g} }, \overline{ \boldsymbol{q} }_{ \mathsf{g} } \in \mathbb{R}^{G}$ be the scheduled upper bounds on the generator injections,
with $\overline{ p }_{ \mathsf{g}, i }$ and $\overline{ q }_{ \mathsf{g}, i }$ being the bounds for unit $i$;
define $\overline{ p }_{g} = \sum \overline{ p }_{ \mathsf{g}, i }$ and $\overline{ q }_{g} = \sum \overline{ q }_{ \mathsf{g}, i }$ as the sums of the bounds.
Then the anticipated control variables are calculated as

```{math}
:label: mol-anticipated-c
\begin{aligned}
    \boldsymbol{p}_{ \mathsf{g} }
    & \approx
    \frac{ \widetilde{ p }_{ \mathsf{g} } + \Delta p_{ \mathsf{g} } }{ \overline{ p }_{g} }
    \thinspace
    \overline{\boldsymbol{p}}_{ \mathsf{g} }
    \, , \\
    \boldsymbol{q}_{ \mathsf{g} }
    & \approx
    \frac{ \widetilde{ q }_{ \mathsf{g} } + \Delta q_{ \mathsf{g} } }{ \overline{ q }_{g} }
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

We discuss the coefficient matrix in Equation {eq}`moj-change-s`
and the matrix multiplier in Equation {eq}`moj-change-c`
in {ref}`a later subsection <apf-prc:antic-injs:moj:cal-jac>`.

The "effective" approximate changes in the total generator injections will have "contributions" from $\Delta \boldsymbol{c}$ and $\widetilde{ p }_{ \mathsf{l} } + j \widetilde{ q }_{ \mathsf{l} }$.
Let $\operatorname{ sum } \! \left( \cdot \right)$ denote the sum of all the elements in an array.
Then, in a manner similar to {ref}`the method of Lateef<apf-prc:antic-injs:mol>`,
the anticipated changes in total generator injections can be approximated *naively* as

```{math}
:label: moj-change-in-sum-c-naive
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & \approx
    \operatorname{ sum } \! \left( \Delta \boldsymbol{p}_{ \mathsf{g} } \right)
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{g} }
    & \approx
    \operatorname{ sum } \! \left( \Delta \boldsymbol{q}_{ \mathsf{g} } \right)
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
        \operatorname{ sum } \! \left( \Delta \boldsymbol{p}_{ \mathsf{g} } \right)
        +
        \widetilde{ p }_{ \mathsf{l} }
    \right)
    \, , \\
    \Delta q_{ \mathsf{g} }
    & \approx
    \operatorname{ ReLU } \! \left(
        \operatorname{ sum } \! \left( \Delta \boldsymbol{q}_{ \mathsf{g} } \right)
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
        \operatorname{ sum } \! \left( \Delta \boldsymbol{p}_{ \mathsf{g} } \right)
    \right)
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{g} }
    & \approx
    \operatorname{ ReLU } \! \left(
        \operatorname{ sum } \! \left( \Delta \boldsymbol{q}_{ \mathsf{g} } \right)
    \right)
    +
    \widetilde{ q }_{ \mathsf{l} }
    \, , \\
\end{aligned}
```

where $\Delta \boldsymbol{p}_{ \mathsf{g} }$ and $\Delta \boldsymbol{q}_{ \mathsf{g} }$
are subvectors of $\Delta \boldsymbol{c}$ {eq}`pg-qg-2-c`.
Then the anticipated control variables are calculated as

```{margin}
Although the last steps in the methods of Lateef and of Jacobians have the same notational forms
(compare {eq}`mol-anticipated-c` and {eq}`moj-anticipated-c`),
they have fundamentally different approaches to calculating $\Delta p_{ \mathsf{g} }$ and $\Delta q_{ \mathsf{g} }$ .
```

```{math}
:label: moj-anticipated-c
\begin{aligned}
    \boldsymbol{p}_{ \mathsf{g} }
    & \approx
    \frac{ \widetilde{ p }_{ \mathsf{g} } + \Delta p_{ \mathsf{g} } }{ \overline{ p }_{g} }
    \thinspace
    \overline{\boldsymbol{p}}_{ \mathsf{g} }
    \, , \\
    \boldsymbol{q}_{ \mathsf{g} }
    & \approx
    \frac{ \widetilde{ q }_{ \mathsf{g} } + \Delta q_{ \mathsf{g} } }{ \overline{ q }_{g} }
    \thinspace
    \overline{\boldsymbol{q}}_{ \mathsf{g} }
    \, .
\end{aligned}
```

(apf-prc:antic-injs:moj:cal-jac)=
### Calculating the Jacobians

To calcualte the Jacobian of $\boldsymbol{s}$ w.r.t. $\boldsymbol{d}^{ \prime }$,
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
