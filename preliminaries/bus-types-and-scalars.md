(prelim:bus-types-and-scalars)=
# Bus types and scalars

In this work, *buses* (or *nodes*) are classified into three types:
slack-reference (SR), PV, and PQ.
SR and PV buses can be considered *generator buses* in the sense that generator units
are connected to them.
PQ buses can either be *load buses* (that is, the load units are connected to them)
or *zero-injection buses* (that is, neither generator nor load units are connected to them).
The sets of SR, of PV, and of PQ buses are disjoint.

Throughout this work, we use
$N_{ \mathsf{s} }$ to denote the number of SR buses,
$N_{ \mathsf{v} }$ the number of PV buses,
$N_{ \mathsf{q} }$ the number of PQ buses,
and
$N$ the aggregate count of them all.
It follows that
$N = N_{\mathsf{s}} + N_{\mathsf{v}} + N_{\mathsf{q}}$.
In code, we reserve
`N_s`, `N_v`, `N_q`, and `N_b`
as the respective variable names for
$N_{ \mathsf{s} }$,
$N_{ \mathsf{v} }$,
$N_{ \mathsf{q} }$,
and
$N$.
In general, the suffixes `_s`, `_v`, and `_q` in variable names
are hereby used to denote quantities pertaining to SR, PV, and PQ buses, respectively.

Furthermore, we use
$E$ to denote the number of branch elements[^about-branch-elems],
$G$ the number of generator units,
$D$ the number of load units, and
$\delta_{ \mathsf{ref} }$ the voltage phase angle at the reference bus.
Their respective variable names are
`N_br`, `N_g`, `N_d`, and `va_ref`.

[^about-branch-elems]: We use the term 'branch elements' as a common model for
transmission lines, transformers, and phase shifters, as in Section 3.2 of
[*MATPOWER User's Manual version 7.1*](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).
