(nom:control-vars-and-load-profile)=
# Control variables and load profile

(nom:control-vars-and-load-profile:unit-level)=
## Unit-level injections and draws

*Control variables* collectively refers to two $G$-vectors:
the vector of generator active power injections,
$\boldsymbol{p}_{ \mathsf{g} }$,
and
the vector of corresponding reactive power injections,
$\boldsymbol{q}_{ \mathsf{g} }$.
The *control vector* is the concatenation of
$\boldsymbol{p}_{ \mathsf{g} }$ and $\boldsymbol{q}_{ \mathsf{g} }$,
*i.e.*,

$$
\boldsymbol{c}
=
\left[ \begin{matrix}
    \boldsymbol{p}_{ \mathsf{g} }   \, \\
    \boldsymbol{q}_{ \mathsf{g} }   \, \\
\end{matrix} \right]
\in \mathbb{R}^{ 2G } \,.
$$

We reserve the variables names `pg` and `qg`
for $\boldsymbol{p}_{ \mathsf{g} }$
and for $\boldsymbol{q}_{ \mathsf{g} }$,
respectively.
In code, we use `c` to indicate either
$\boldsymbol{c}$
or the tuple
$\left( \boldsymbol{p}_{ \mathsf{g} }, \boldsymbol{q}_{ \mathsf{g} } \right)$.

*Load profile* collectively refers to two $D$-vectors:
one for the active power drawn by the load units,
$\boldsymbol{p}_{ \mathsf{d} }$,
and
another for the corresponding reactive components,
$\boldsymbol{q}_{ \mathsf{d} }$.
The *demand vector* is the concatenation of
$\boldsymbol{p}_{ \mathsf{d }}$ and
$\boldsymbol{q}_{ \mathsf{d} }$, *i.e.*

$$
\boldsymbol{d}
=
\left[ \begin{matrix}
    \boldsymbol{p}_{ \mathsf{d} }   \, \\
    \boldsymbol{q}_{ \mathsf{d} }   \, \\
\end{matrix} \right]
\in \mathbb{R}^{ 2D } \,.
$$

We reserve the variables names `pd` and `qd`
for $\boldsymbol{p}_{ \mathsf{d} }$
and for $\boldsymbol{q}_{ \mathsf{d} }$,
respectively.
In code, we use `d` to indicate either
$\boldsymbol{d}$
or the tuple
$\left( \boldsymbol{p}_{ \mathsf{d} }, \boldsymbol{q}_{ \mathsf{d} } \right)$.

(nom:control-vars-and-load-profile:bus-level)=
## Bus-level contributions

Through the {ref}`generator connection matrix <nom:connection-mats:Cg>`,
the aggregate injections at the buses due to the generator units are computed as
$\boldsymbol{C}_{ \mathsf{g} } \left( \boldsymbol{p}_{ \mathsf{g} } + j \boldsymbol{p}_{ \mathsf{g} } \right) $.
In terms of $\boldsymbol{c}$,

$$
\boldsymbol{c}^{\prime}
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{g} } \, \boldsymbol{p}_{ \mathsf{g} } \, \\
    \boldsymbol{C}_{ \mathsf{g} } \, \boldsymbol{q}_{ \mathsf{g} } \, \\
\end{matrix} \right]
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{g} }
    &
    \boldsymbol{0}_{ N,G }          \, \\
    \boldsymbol{0}_{ N,G }
    &
    \boldsymbol{C}_{ \mathsf{g} }   \, \\
\end{matrix} \right]
\,
\left[ \begin{matrix}
    \boldsymbol{p}_{ \mathsf{g} } \, \\
    \boldsymbol{q}_{ \mathsf{g} } \, \\
\end{matrix} \right]
=
\boldsymbol{C}_{ \mathsf{G} } \, \boldsymbol{c}
\, .
$$

Similarly, the {ref}`generator connection matrix <nom:connection-mats:Cg>`
the contributions of the load units to the power injections at the buses are computed as
$\boldsymbol{C}_{ \mathsf{d} } \left( \boldsymbol{p}_{ \mathsf{d} } + j \boldsymbol{p}_{ \mathsf{d} } \right) $.
In terms of $\boldsymbol{d}$,

$$
\boldsymbol{d}^{\prime}
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{d} } \, \boldsymbol{p}_{ \mathsf{d} } \, \\
    \boldsymbol{C}_{ \mathsf{d} } \, \boldsymbol{q}_{ \mathsf{d} } \, \\
\end{matrix} \right]
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{d} }
    &
    \boldsymbol{0}_{ N,D }          \, \\
    \boldsymbol{0}_{ N,D }
    &
    \boldsymbol{C}_{ \mathsf{d} }   \, \\
\end{matrix} \right]
\,
\left[ \begin{matrix}
    \boldsymbol{p}_{ \mathsf{d} } \, \\
    \boldsymbol{q}_{ \mathsf{d} } \, \\
\end{matrix} \right]
=
\boldsymbol{C}_{ \mathsf{D} } \, \boldsymbol{d}
\, .
$$
