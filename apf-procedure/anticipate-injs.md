(apf-prc:antic-injs)=
# Anticipating generator injections

```{caution}
This page is under construction
```

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

The change in total power draw is

```{math}
:label: mol-change-total-d
\begin{aligned}
    \Delta p_{ \mathsf{d} }
    & =
    \boldsymbol{1}^{ \! \mathsf{T} } \boldsymbol{p}_{ \mathsf{d} }
    -
    \boldsymbol{1}^{ \! \mathsf{T} } \widetilde{ \boldsymbol{p} }_{ \mathsf{g} }
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{d} }
    & =
    \boldsymbol{1}^{ \! \mathsf{T} } \boldsymbol{q}_{ \mathsf{d} }
    -
    \boldsymbol{1}^{ \! \mathsf{T} } \widetilde{ \boldsymbol{q} }_{ \mathsf{g} }
    +
    \widetilde{ q }_{ \mathsf{l} }
    \, ,
\end{aligned}
```

where the $\boldsymbol{1}$'s are vectors of ones appropriately sized so that the right-hand side expressions evaluate to scalars.
The anticiapted changes in total injections by the generators can then be approximated *naively* as

```{math}
:label: mol-change-in-c-naive
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & =
    \Delta p_{ \mathsf{d} }
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{g} }
    & =
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
:label: mol-change-in-c-conserv
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & =
    \operatorname{ReLU} \! \left(
        \Delta p_{ \mathsf{d} }
        +
        \widetilde{ p }_{ \mathsf{l} }
    \right)
    \, , \\
    \Delta q_{ \mathsf{g} }
    & =
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
A *strictly-increasing* flavour of {eq}`mol-change-in-c-naive` can even be used when it is certain that total demand will increase:

```{math}
:label: mol-change-in-c-strict
\begin{aligned}
    \Delta p_{ \mathsf{g} }
    & =
    \operatorname{ReLU} \! \left(
        \Delta p_{ \mathsf{d} }
    \right)
    +
    \widetilde{ p }_{ \mathsf{l} }
    \, , \\
    \Delta q_{ \mathsf{g} }
    & =
    \operatorname{ReLU} \! \left(
        \Delta q_{ \mathsf{d} }
    \right)
    +
    \widetilde{ q }_{ \mathsf{l} }
    \, .
\end{aligned}
```

The quantities
$\Delta p_{ \mathsf{g} }$ and $\Delta q_{ \mathsf{g} }$
are distributed among the generator units in proportion to the scheduled upper bounds on their injections.
This yields the approximate changes for the injections of the individual units,
so that the anticipated control variables are given by

```{math}
:label: mol-anticipated-c
\begin{aligned}
    \boldsymbol{p}_{ \mathsf{g} }
    & \approx
    \widetilde{ \boldsymbol{p} }_{ \mathsf{g} }
    +
    \frac{ \Delta p_{ \mathsf{g} } }{
        \boldsymbol{1}^{ \! \mathsf{T} } \, \overline{ \boldsymbol{p} }_{ \mathsf{g} }
    }
    \thinspace
    \overline{\boldsymbol{p}}_{ \mathsf{g} }
    \, , \\
    \boldsymbol{q}_{ \mathsf{g} }
    & \approx
    \widetilde{ \boldsymbol{q} }_{ \mathsf{g} }
    +
    \frac{ \Delta p_{ \mathsf{g} } }{
        \boldsymbol{1}^{ \! \mathsf{T} } \, \overline{ \boldsymbol{q} }_{ \mathsf{g} }
    }
    \thinspace
    \overline{\boldsymbol{q}}_{ \mathsf{g} }
    \, . 
\end{aligned}
```

Equation
{eq}`mol-change-total-d`
and the additive term in Equation
{eq}`mol-change-total-d`
are based on the experimental setup detailed in Section 2.4 of Omer Lateef's dissertation,
[*Measurement-Based Parameter Estimation and Analysis of Power System*](http://hdl.handle.net/1853/63600).
This explains the name given to the method.

(apf-prc:antic-injs:moj)=
## The method of Jacobians

Suppose that the following are among the snapshot data $\mathcal{S}$:

* bus voltage magnitudes, $ \widetilde{ \boldsymbol{v} } $ ,
  and phase angles, $ \widetilde{ \boldsymbol{\delta} } $ ,
* load draws, $\boldsymbol{p}_{ \mathsf{d} } + j \boldsymbol{q}_{ \mathsf{d} }$ ,
* generator injections, $\boldsymbol{p}_{ \mathsf{g} } + j \boldsymbol{q}_{ \mathsf{g} }$ ,
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
