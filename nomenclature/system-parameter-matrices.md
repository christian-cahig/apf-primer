(nom:syst-param-mats)=
# System parameter matrices

(nom:syst-param-mats:Y)=
## System bus admittance matrix

From elementary steady state power system modelling,
the net current phasors injected at the buses
are a linear function of the corresponding voltage phasors;
the *system bus admittance matrix*
$\mathbf{Y} \in \mathbb{C}^{N \times N}$
is the coefficient of such (complex) linear system[^about-Y].
One can think of $\mathbf{Y}$ to be jointly encoding
the power system's intrinsic parameters (*i.e.*, impedances and admittances)
and topology (*i.e.*, sparsity structure).
In Cartesian notation,
$\mathbf{Y} = \boldsymbol{G} + j\boldsymbol{B}$,
where $\boldsymbol{G}$ and $\boldsymbol{B}$ are the respective conductance and susceptance components.
We reserve the variable names `Y`, `G`, and `B` for
$\mathbf{Y}$, $\boldsymbol{G}$, and $\boldsymbol{B}$, respectively.

[^about-Y]: For brevity, we leave out the details on how $\mathbf{Y}$ is calculated.
Please refer to
Sections 3.2, 3.5, and 3.6 of
[*MATPOWER User's Manual version 7.1*](https://matpower.org/docs/MATPOWER-manual-7.1.pdf),
or to
Section 4 of
[*An introduction to optimal power flow: Theory, formulation, and examples*](https://doi.org/10.1080/0740817X.2016.1189626).

(nom:syst-param-mats:E-Z)=
## Conductance-susceptance matrices

Let
$\boldsymbol{G}_{ \mathsf{0} }$
and
$\boldsymbol{B}_{ \mathsf{0} }$
be equal to
$\boldsymbol{G}$
and to
$\boldsymbol{B}$,
respectively, but with zero diagonals.
Define
$\boldsymbol{A}_{ \mathsf{d} } = \boldsymbol{C}_{ \mathsf{f} }^{ \mathsf{T} } - \boldsymbol{C}_{ \mathsf{t} }^{ \mathsf{T} }$
and
$\boldsymbol{A}_{ \mathsf{u} } = \boldsymbol{C}_{ \mathsf{f} }^{ \mathsf{T} } + \boldsymbol{C}_{ \mathsf{t} }^{ \mathsf{T} }$.
Let
$\odot$
denote elementwise matrix multiplication,
$\operatorname{diag} \! \left( \boldsymbol{m} \right)$
denote a diagonal $M \times M$ matrix whose elements are given by some $M$-vector $\boldsymbol{m}$,
and
$\operatorname{diags} \! \left( \boldsymbol{M} \right)$
denote an $M$-vector comprised of the main-diagonal elements of some $M \times M$ matrix $\boldsymbol{M}$.

The *full conductance-susceptance matrix* $\boldsymbol{E}$ is obtained by first setting it to be

$$
\boldsymbol{E}
\, \gets
\left[ \begin{matrix}
    \operatorname{diag} \! \left( \operatorname{diags} \! \left( \boldsymbol{G} \right) \right)
    &
    \boldsymbol{A}_{ \mathsf{u} } \odot \boldsymbol{G}_{\mathsf{0}} \boldsymbol{A}_{ \mathsf{u} }
    &
    \boldsymbol{A}_{ \mathsf{d} } \odot \boldsymbol{B}_{\mathsf{0}} \boldsymbol{A}_{ \mathsf{u} }
    \, \\
    -\operatorname{diag} \! \left( \operatorname{diags} \! \left( \boldsymbol{B} \right) \right)
    &
    -\boldsymbol{A}_{ \mathsf{u} } \odot \boldsymbol{B}_{\mathsf{0}} \boldsymbol{A}_{ \mathsf{u} }
    &
    \boldsymbol{A}_{ \mathsf{d} } \odot \boldsymbol{G}_{\mathsf{0}} \boldsymbol{A}_{ \mathsf{u} }
    \, \\
\end{matrix} \right]
\in \mathbb{R}^{ 2N \times \left( N + 2E \right) }
\, ,
$$

and then permuting the right most $2E$ columns by "interweaving"
the $\left( N + 1 \right)$-th through the $\left( N + E \right)$-th columns
and
the $\left( N + E + 1 \right)$-th through the $\left( N + 2E \right)$-th:

$$
\boldsymbol{E}
\, \gets
\left[ \begin{matrix}
    \boldsymbol{E}_{ : ,\, 1:N }
    &
    \boldsymbol{E}_{ : ,\, N + 1 }
    &
    \boldsymbol{E}_{ : ,\, N + E + 1 }
    &
    \boldsymbol{E}_{ : ,\, N + 2 }
    &
    \boldsymbol{E}_{ : ,\, N + E + 2 }
    &
    \cdots
    &
    \boldsymbol{E}_{ : ,\, N + E }
    &
    \boldsymbol{E}_{ : ,\, N + 2E }
    \, \\
\end{matrix} \right]
\, .
$$

The *reduced conductance-susceptance matrix* $\boldsymbol{Z}$ is a submatrix of $\boldsymbol{E}$
formed by dropping the topmost $N_{ \mathsf{s} }$ and the
$\left( N + 1 \right)$-th through $\left( N + N_{ \mathsf{s} } \right)$-th
rows of $\boldsymbol{E}$[^about-Z], *i.e.*,

$$
\boldsymbol{Z}
=
\left[ \begin{matrix}
    \boldsymbol{E}_{ N_{ \mathsf{s} } + 1 \, : \, N ,\, : }   \, \\
    \boldsymbol{E}_{ N + N_{ \mathsf{s} } + 1 \, : ,\, : }   \, \\
\end{matrix} \right]
\in \mathbb{R}^{ \left( 2N - 2N_{ \mathsf{s} } \right) \times \left( N + 2E \right) }
\, .
$$

Formally,
$\boldsymbol{E}$
is the coefficient of the linear dependence of
{ref}`the full net nodal injection vector <nom:net-nodal-injs:e-z>`
$\boldsymbol{e}$
on
{ref}`the canonicalized state vector <nom:state-vars:y-u>`
$\boldsymbol{y}$,
*i.e.*,
$\boldsymbol{e} = \boldsymbol{E} \boldsymbol{y}$.
This linear relation is the first component of
{ref}`the factored formulation of the power flow equations <nom:pow-flow-eqns>`.
Similarly,
$\boldsymbol{Z}$
is the coefficient of the linear dependence of
{ref}`the reduced net nodal injection vector <nom:net-nodal-injs:e-z>`
$\boldsymbol{z}$
on
$\boldsymbol{y}$,
*i.e.*,
$\boldsymbol{z} = \boldsymbol{Z} \boldsymbol{y}$.
We reserve the variable names `E` and `Z` for $\boldsymbol{E}$ and for $\boldsymbol{Z}$, respectively.

[^about-Z]: This corresponds to dropping the slack nodal injections.
See {ref}`nom:net-nodal-injs:ez-from-y`.
