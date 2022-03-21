(prelim:connection-mats)=
# Connection matrices

(prelim:connection-mats:Cf-Ct)=
## Branch connection matrices

Connectivity of branches are encoded by
the *from-* and the *to-side branch connection matrices*
$\boldsymbol{C}_{ \mathsf{f} }$
and
$\boldsymbol{C}_{ \mathsf{t} }$.
These are sparse $E \times N$ matrices constructed such that
$\left[ \begin{matrix} \boldsymbol{C}_{ \mathsf{f} } \end{matrix} \right]_{i,j} = 1$
and
$\left[ \begin{matrix} \boldsymbol{C}_{ \mathsf{t} } \end{matrix} \right]_{i,j} = 1$
if the $i$-th branch (in a consistent roster) has
its 'from' (sending) end at the $j$-th bus
and its 'to' (receiving) end at the $k$-th bus[^about-Cf-Ct].
The variable names `Cf` and `Ct` are reserved for
$\boldsymbol{C}_{ \mathsf{f} }$
and
$\boldsymbol{C}_{ \mathsf{t} }$,
respectively.

[^about-Cf-Ct]: Aside from notation, this is the same formulation used in Section 3.2 of
[*MATPOWER User's Manual version 7.1*](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).

(prelim:connection-mats:Cg)=
## Generator connection matrix

The *generator connection matrix*
$\boldsymbol{C}_{ \mathsf{g} }$
encodes which generator units are connected to which buses.
With consistent rosters of $G$ generator units and of $N$ buses
(indexed according to {doc}`./bus-indexing`),
$\boldsymbol{C}_{ \mathsf{g}}$ is a sparse $N \times G$ matrix where
(using bracket indexing)
$\left[ \begin{matrix} \boldsymbol{C}_{ \mathsf{g}} \\ \end{matrix} \right]_{i, j} =  1$
if generator $j$ is at bus $i$,
and
$\left[ \begin{matrix} \boldsymbol{C}_{ \mathsf{g}} \\ \end{matrix} \right]_{i, j} =  0$
otherwise[^about-Cg].
The variable name `Cg` is reserved for $\boldsymbol{C}_{ \mathsf{g}} $.

[^about-Cg]: Aside from notation, this is the same formulation used in Section 3.3 of
[*MATPOWER User's Manual version 7.1*](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).

(prelim:connection-mats:Cd)=
## Load connection matrix

The *load connection matrix*
$\boldsymbol{C}_{\mathsf{d} }$
tells us which load units are connected to which buses.
With consistent rosters of $D$ load units and of $N$ buses,
it is a sparse $N \times D$ matrix where
$\left[ \begin{matrix} \boldsymbol{C}_{\mathsf{d} } \\ \end{matrix} \right]_{i, j} = 1$
if load $j$ is at bus $i$,
and
$\left[ \begin{matrix} \boldsymbol{C}_{\mathsf{d} } \\ \end{matrix} \right]_{i, j} = 0$
otherwise.
The variable name `Cd` is reserved for $\boldsymbol{C}_{\mathsf{d} }$.

(prelim:connection-mats:CW)=
## Branch-to-bus incidence matrices

Let
$ \boldsymbol{C}_{ \mathsf{ft} }^{ + } $
and
$ \boldsymbol{C}_{ \mathsf{ft} }^{ - } $
be the *undirected* and the *directed branch-to-bus incidence matrices*
given by

$$
\boldsymbol{C}_{ \mathsf{ft} }^{ + }
=
\boldsymbol{C}_{ \mathsf{f} } + \boldsymbol{C}_{ \mathsf{t} }
\in \mathbb{R}^{ E \times N }
\, ,
$$ (Cft-plus)

$$
\boldsymbol{C}_{ \mathsf{ft} }^{ - }
=
\boldsymbol{C}_{ \mathsf{f} } - \boldsymbol{C}_{ \mathsf{t} }
\in \mathbb{R}^{ E \times N }
\, ,
$$ (Cft-minus)

respectively.
The *full augmented branch-to-bus incidence matrix* $\boldsymbol{C}$ is obtained by first setting it to be

```{margin}
Equation {eq}`C-step-1` is an extension of Equation (45) of
[*Bilinear Power System State Estimation*](https://doi.org/10.1109/TPWRS.2011.2162256).
```

$$
\boldsymbol{C}
\, \gets
\left[ \begin{matrix}
    \boldsymbol{I}_{ N }                    &   \boldsymbol{0}_{ N ,\, N }
    \\
    \boldsymbol{C}_{ \mathsf{ft} }^{ + }    &   \boldsymbol{0}_{ E ,\, N }
    \\
    \boldsymbol{0}_{ E ,\, N }              &   \boldsymbol{C}_{ \mathsf{ft} }^{ - }
    \\
\end{matrix} \right]
\in \mathbb{R}^{ \left( N + 2E \right) \times \left( 2N \right) }
\, ,
$$ (C-step-1)

with
$\boldsymbol{I}_{N}$ being an $N \times N$ identity matrix
and
$\boldsymbol{0}_{a, b}$ denoting an $a \times b$ matrix of zeros,
and then permuting the last $2E$ rows by "interweaving"
the $\left( N + 1 \right)$-th through the $\left( N + E \right)$-th rows
and
the $\left( N + E + 1 \right)$-th through the $\left( N + 2E \right)$-th:

$$
\boldsymbol{C}
\, \gets
\left[ \begin{matrix}
    \boldsymbol{C}_{1 : N, \, :}        \, \\
    \boldsymbol{C}_{N + 1, \, :}        \, \\
    \boldsymbol{C}_{N + E + 1, \, :}    \, \\
    \boldsymbol{C}_{N + 2, \, :}        \, \\
    \boldsymbol{C}_{N + E + 2, \, :}    \, \\
    \vdots                              \, \\
    \boldsymbol{C}_{N + E - 1, \, :}    \, \\
    \boldsymbol{C}_{N + 2E - 1, \, :}   \, \\
    \boldsymbol{C}_{N + E, \, :}        \, \\
    \boldsymbol{C}_{N + 2E, \, :}       \, \\
\end{matrix} \right]
\, .
$$ (C-step-2)

The *reduced augmented branch-to-bus incidence matrix* $\boldsymbol{W}$ is a submatrix of $\boldsymbol{C}$
formed by removing from the latter the $\left( N + 1 \right)$-th through $\left( N + N_{ \mathsf{s} } \right)$-th columns:

$$
\boldsymbol{W}
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ : ,\, 1:N }
    &
    \boldsymbol{C}_{ : ,\, N + N_{ \mathsf{s} } + 1 : }
    \, \\
\end{matrix} \right]
\in \mathbb{R}^{ \left( N + 2E \right) \times \left( 2N - N_{ \mathsf{s} } \right) }
\, .
$$

In code, we use `C` and `W` for $\boldsymbol{C}$ and for $\boldsymbol{W}$, respectively.

<!-- 
Formally,
{ref}`the sigma transform of the canonicalized state vector <prelim:state-vars:y-u>`
$\boldsymbol{u}$
is related to
{ref}`the full intermediate state vector <prelim:state-vars:interm>`
$\boldsymbol{x}$
as
$\boldsymbol{u} = \boldsymbol{C} \boldsymbol{x}$.
This linear relation is the second component of
{ref}`the factored formulation of the power flow equations <prelim:pow-flow-eqns:balance>`.
-->
