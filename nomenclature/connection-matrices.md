(nom:connection-mats)=
# Connection matrices

(nom:connection-mats:Cg)=
## Generator connection matrix

````{margin}
```{admonition} About $\boldsymbol{C}_{ \mathsf{g} }$
This is the same formulation used in Section 3.3 of the
[MATPOWER User's Manual version 7.1](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).
```
````

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
otherwise.
The variable name `Cg` is reserved for $\boldsymbol{C}_{ \mathsf{g}} $.

(nom:connection-mats:Cd)=
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

(nom:connection-mats:Cf-Ct)=
## Branch connection matrices

````{margin}
```{admonition} About $\boldsymbol{C}_{ \mathsf{f} }$ and $\boldsymbol{C}_{ \mathsf{t} }$
This is the same formulation used in Section 3.2 of the
[MATPOWER User's Manual version 7.1](https://matpower.org/docs/MATPOWER-manual-7.1.pdf).
```
````

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
and its 'to' (receiving) end at the $k$-th bus.
The variable names `Cf` and `Ct` are reserved for
$\boldsymbol{C}_{ \mathsf{f} }$
and
$\boldsymbol{C}_{ \mathsf{t} }$,
respectively.

(nom:connection-mats:C)=
## Generalized branch connection matrix

````{margin}
```{admonition} About $\boldsymbol{C}$
The first step in the construction is based on Equation (45) of
[*Bilinear Power System State Estimation*](https://doi.org/10.1109/TPWRS.2011.2162256).
```
````

The *generalized branch connection matrix* is the coefficient when
{ref}`the sigma transform of the canonicalized state vector <nom:state-vars:y-u>`
$\boldsymbol{u}$
is expressed as a linear function of
{ref}`the intermediate state vector <nom:state-vars:x>`
$\boldsymbol{x}$
*i.e.*,

$$
\boldsymbol{u} = \boldsymbol{C} \boldsymbol{x}
$$

with
$\boldsymbol{C} \in \mathbb{R}^{ \left( N + 2E \right) \times \left( 2N - N_{ \mathsf{s} } \right) }$.
One can construct $\boldsymbol{C}$ in two steps.
First, initialize it to be the coefficient matrix of the linear relation

$$
\left[ \begin{matrix}
    \boldsymbol{\alpha} \\
    \boldsymbol{\kappa} \\
    \boldsymbol{\rho} \\
\end{matrix} \right]
=
\left[ \begin{matrix}
    \boldsymbol{I}_{N} & \boldsymbol{0}_{N, N - N_{\mathsf{s}}} \\
    \boldsymbol{C}_{\mathsf{f}} + \boldsymbol{C}_{\mathsf{t}}
    & \boldsymbol{0}_{E, N - N_{\mathsf{s}}} \\
    \boldsymbol{0}_{E, N}
    &
    \left[ \boldsymbol{C}_{\mathsf{f}} \right]_{:, N_{\mathsf{s}} + 1 :}
    -
    \left[ \boldsymbol{C}_{\mathsf{t}} \right]_{:, N_{\mathsf{s}} + 1 :}
    \\
\end{matrix} \right]
\boldsymbol{x}
$$

where
$\boldsymbol{I}_{N}$ denotes an $N \times N$ identity matrix
and
$\boldsymbol{0}_{a, b}$ denotes an $a \times b$ matrix of zeros.
From {ref}`nom:state-vars:aux`,
$\boldsymbol{u}$ is the concatenation of $\boldsymbol{\alpha}$
and the 'interweaving' of $\boldsymbol{\kappa}$ and $\boldsymbol{\rho}$.
Hence, $\boldsymbol{C}$ is obtained by appropriately permuting the last $2E$ rows
of the above coefficient matrix.
We use `C` as the dedicated variable name for $\boldsymbol{C}$.
