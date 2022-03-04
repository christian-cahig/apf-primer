(nom:prob-data)=
# Problem data

The system data can be grouped into two sets:
the ***previous-snapshot data***, $\mathcal{S}$,
and
the ***present-dispatch data***, $\mathcal{D}$.
For convenience, we drop the modifiers 'previous' and 'present'.
Together, the snapshot and dispatch data form the ***problem data***, $\mathcal{P}$,
*i.e.*, the "given" in an APF task.

In this work, we assume that $\mathcal{S}$ contains

* the bus voltage magnitudes and phase angles,
* the generator injections,
* the load draws,
* the system loss, and
* the connection and parameter matrices.

As for $\mathcal{D}$, we assume that it contains

* the expected load draws,
* the scheduled upper bounds on the generator injections, and
* the connection and parameter matrices.

We suppose that the grid topology does not change between the preceding snapshot and the present dispatch;
hence, the branch connection matrices in $\mathcal{S}$ and in $\mathcal{D}$ will be identical.
We do not constrain the generator connection matrix to be invariant but simply assume it to have the same number of columns;
that is, a generator unit that was previously online but will be offline in the present dispatch
corresponds to zeroing out the appropriate element in the generator connection matrix for $\mathcal{D}$.
On the other hand, we can take the load connection matrix to be invariant;
that is, a load unit served in the preceding snapshot but dropped in the present dispatch
correpsonds to zeroing out the appropriate components in the demand vector for $\mathcal{D}$.
The expected load draws are taken to be the output of some existing forecasting routine.
The parameter matrices are assumed to be produced from some estimation process[^about-YEZ].
Lastly, the scheduled upper bounds on the generator injections can either
be computed from the ramping rates of the generator units, or be simply set to unit capacities.

[^about-YEZ]: For example, see
[*Measurement-Based Parameter Estimation and Analysis of Power System*](http://hdl.handle.net/1853/63600).
