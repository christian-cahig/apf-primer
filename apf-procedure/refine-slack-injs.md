(apf-prc:refine-slack-injs)=
# Refining slack-bus generator injections

Since, the bus voltages are solved using only the power flow equations at the non-slack buses,
the anticipated injections of the generator units at the slack bus must be refined
so that the aggregate of their nodal injections at the said bus
matches that which can be inferred from the state variables.

Following {ref}`the bus indexing convention <prelim:bus-indexing>` for $N$-vectors and for $N \times N$ matrices,
the net injections at the slack bus is given by

```{margin}
One may think of Equation {eq}`vbus-to-pq-slack` as a "slice" of Equation {eq}`vbus-2-pq`.
```

$$
\boldsymbol{p}_{ \mathsf{s} } + j \boldsymbol{q}_{ \mathsf{s} }
=
\operatorname{ diag } \! \left( \mathbf{v}_{ \mathsf{s} } \right)
\,
\operatorname{ conj } \! \left(
    \mathbf{Y}_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : }
    \thinspace
    \mathbf{v}
\right)
\, .
$$ (vbus-to-pq-slack)

From the nodal power balance, $\boldsymbol{p}_{ \mathsf{s} } + j \boldsymbol{q}_{ \mathsf{s} }$ must be equal to
the difference between the total generator injections and the total load draws at the slack bus.
Therefore,

$$
\begin{aligned}
\boldsymbol{1}^{ \! \mathsf{T} }
\left[ \boldsymbol{C}_{ \mathsf{g} } \right]_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : \, }
\boldsymbol{p}_{ \mathsf{g} }
& =
\boldsymbol{1}^{ \! \mathsf{T} } \boldsymbol{p}_{ \mathsf{s}}
\, +
\boldsymbol{1}^{ \! \mathsf{T} }
\left[ \boldsymbol{C}_{ \mathsf{d} } \right]_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : \, }
\boldsymbol{p}_{ \mathsf{d}}
\, , \\
\boldsymbol{1}^{ \! \mathsf{T} }
\left[ \boldsymbol{C}_{ \mathsf{g} } \right]_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : \, }
\boldsymbol{q}_{ \mathsf{g} }
& =
\boldsymbol{1}^{ \! \mathsf{T} } \boldsymbol{q}_{ \mathsf{s}}
\, +
\boldsymbol{1}^{ \! \mathsf{T} }
\left[ \boldsymbol{C}_{ \mathsf{d} } \right]_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : \, }
\boldsymbol{q}_{ \mathsf{d}}
\, .
\end{aligned}
$$ (c-slack-total)

The aggregate generation injection at the slack bus,
$\boldsymbol{1}^{ \! \mathsf{T} }
\left[ \boldsymbol{C}_{ \mathsf{g} } \right]_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : \, } \left( \boldsymbol{p}_{ \mathsf{g} } + j \boldsymbol{q}_{ \mathsf{g} } \right)$,
is distributed among the units at that bus in proportion to their maximum allowable injections for the dispatch.
Let $\mathcal{G}_{ \mathcal{s} }$ be the set of generator units at the slack bus.
Then the adjusted anticipated injection for slack-bus unit $i$
is

$$
p_{ \mathsf{g}, i }
\, = \,
\frac{ \overline{p}_{ \mathsf{g}, i } }{ \sum_{j} \overline{p}_{ \mathsf{g}, j } }
\thinspace
\boldsymbol{1}^{ \! \mathsf{T} }
\left[ \boldsymbol{C}_{ \mathsf{g} } \right]_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : \, }
\boldsymbol{p}_{ \mathsf{g} }
\, = \,
\frac{ \overline{p}_{ \mathsf{g}, i } }{ \sum_{j} \overline{p}_{ \mathsf{g}, j } }
\thinspace
\boldsymbol{1}^{ \! \mathsf{T} } \boldsymbol{p}_{ \mathsf{s}}
\, +
\boldsymbol{1}^{ \! \mathsf{T} }
\left[ \boldsymbol{C}_{ \mathsf{d} } \right]_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : \, }
\boldsymbol{p}_{ \mathsf{d}}
\, ,
$$ (pg-slack-indiv)

$$
q_{ \mathsf{g}, i }
\, = \,
\frac{ \overline{q}_{ \mathsf{g}, i } }{ \sum_{j} \overline{q}_{ \mathsf{g}, j } }
\thinspace
\boldsymbol{1}^{ \! \mathsf{T} }
\left[ \boldsymbol{C}_{ \mathsf{g} } \right]_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : \, }
\boldsymbol{q}_{ \mathsf{g} }
\, = \,
\frac{ \overline{q}_{ \mathsf{g}, i } }{ \sum_{j} \overline{q}_{ \mathsf{g}, j } }
\thinspace
\boldsymbol{1}^{ \! \mathsf{T} } \boldsymbol{q}_{ \mathsf{s}}
\, +
\boldsymbol{1}^{ \! \mathsf{T} }
\left[ \boldsymbol{C}_{ \mathsf{d} } \right]_{ 1 \, : \, N_{ \mathsf{s} } \, , \, : \, }
\boldsymbol{q}_{ \mathsf{d}}
\, ,
$$ (qg-slack-indiv)

for all $i, j \in \mathcal{G}_{ \mathcal{s} }$ .
