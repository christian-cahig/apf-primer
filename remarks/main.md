(rem:main)=
# Remarks

A few remarks are in order.
First, it is best to think of APF as a heuristic procedure whose outcome is an input to some other procedure in some workflow.
In fact, it was initially conceived as a method for obtaining an initial estimates of the generator injections and the bus voltages
for a *standard AC optimal power flow* task such that the estimates already satisfy the power flow equations.

Second, it is possible to anticipate the generator injections by some other heuristic.
For instance, one may use the total active generation projected by a scheduling routine,
say, $p_{ \mathsf{g} }$ , and distribute it among the generator units in proportion to their scheduled limits,
*i.e.*,

$$
\boldsymbol{p}_{ \mathsf{g} }
\, \approx
\frac{ p_{ \mathsf{g} } }{
    \boldsymbol{1}^{ \! \mathsf{T} } \, \overline{ \boldsymbol{p} }_{ \mathsf{g} }
}
\thinspace
\overline{\boldsymbol{p}}_{ \mathsf{g} }
\, .
$$

As for the reactive counterparts, one may use the system's power factor at the previous snapshot,
say, $\widetilde{ \gamma }$,
to obtain an anticipated total reactive generation of

$$
q_{ \mathsf{g} }
\approx
p_{ \mathsf{g} } \, \tan \! \left( \operatorname{arccos}  \! \left( \widetilde{ \gamma } \right) \right)
\, ,
$$

to be distributed among the units as

$$
\boldsymbol{q}_{ \mathsf{g} }
\, \approx
\frac{ q_{ \mathsf{g} } }{
    \boldsymbol{1}^{ \! \mathsf{T} } \, \overline{ \boldsymbol{q} }_{ \mathsf{g} }
}
\thinspace
\overline{\boldsymbol{q}}_{ \mathsf{g} }
\, .
$$

It is also conceivable to use the generator units' previous-snapshot power factors,
say, $\widetilde{ \boldsymbol{\gamma} }_{ \mathsf{g} } \in \mathbb{R}^{ G }$,
to estimate their present-dispatch reactive limits given their scheduled active limits:

$$
\overline{ \boldsymbol{q} }_{ \mathsf{g} }
\, \approx
\operatorname{diag} \! \left( \overline{ \boldsymbol{p} }_{ \mathsf{g} } \right)
\tan \! \left( \operatorname{arccos}  \! \left(
    \widetilde{ \boldsymbol{\gamma} }_{ \mathsf{g} }
\right) \right)
\, .
$$
