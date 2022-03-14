(prelim:state-vars)=
# State variables and variants

(prelim:state-vars:vm-va)=
## System state variables

We use *system state variables* to collectively refer to two $N$-vectors:
the vector of bus voltage magnitudes,
$\boldsymbol{v}$,
and
the vector of their corresponding phase angles,
$\boldsymbol{\delta}$.
The *full state vector*
$\boldsymbol{s}$
is the concatenation of $\boldsymbol{v}$ and $\boldsymbol{\delta}$,
that is,

$$
\boldsymbol{s}
=
\left[ \begin{matrix}
    \boldsymbol{v}      \, \\
    \boldsymbol{\delta} \, \\
\end{matrix} \right]
\in \mathbb{R}^{2N} \, .
$$ (s)

We reserve the variable names `vm` and `va`
for $\boldsymbol{v}$
and for $\boldsymbol{\delta}$,
respectively.
In code, we use `s` to indicate either
$\boldsymbol{s}$
or the tuple
$\left( \boldsymbol{v}, \boldsymbol{\delta} \right)$.

(prelim:state-vars:aux)=
## Auxiliary state variables

Let
$\boldsymbol{a}$
be the $N$-vector of squared voltage magnitudes,
*i.e.*,

$$
\boldsymbol{a}
=
\operatorname{diag} \! \left( \boldsymbol{v} \right) \boldsymbol{v}
\, .
$$ (a)

Let $\mathsf{f}$ and $\mathsf{t}$ be used as subscripts
to denote quantities at the from- and at the to-side buses, respectively, of the branches.
Then we have the $E$-vectors
$\boldsymbol{v}_{ \mathsf{f} } = \boldsymbol{C}_{ \mathsf{f} } \, \boldsymbol{v}$
and
$\boldsymbol{\delta}_{ \mathsf{f} } = \boldsymbol{C}_{ \mathsf{f} } \, \boldsymbol{\delta}$
for the sending,
and
$\boldsymbol{v}_{ \mathsf{t} } = \boldsymbol{C}_{ \mathsf{t} } \, \boldsymbol{v}$
and
$\boldsymbol{\delta}_{ \mathsf{t} } = \boldsymbol{C}_{ \mathsf{t} } \, \boldsymbol{\delta}$
for the receiving ends.
We can then define the $E$-vectors
$\boldsymbol{k}$ and $\boldsymbol{r}$
as

$$
\boldsymbol{k}
=
\operatorname{diag} \! \left( \boldsymbol{v}_{ \mathsf{f} } \right)
\operatorname{diag} \! \left( \boldsymbol{v}_{ \mathsf{t} } \right)
\cos \! \left( \boldsymbol{\delta}_{ \mathsf{f} } - \boldsymbol{\delta}_{ \mathsf{t} } \right)
\, ,
$$ (k)

$$
\boldsymbol{r}
=
\operatorname{diag} \! \left( \boldsymbol{v}_{ \mathsf{f} } \right)
\operatorname{diag} \! \left( \boldsymbol{v}_{ \mathsf{t} } \right)
\sin \! \left( \boldsymbol{\delta}_{ \mathsf{f} } - \boldsymbol{\delta}_{ \mathsf{t} } \right)
\, .
$$ (r)

We use `a`, `k`, and `r`
as the respective variable names for
$\boldsymbol{a}$, $\boldsymbol{k}$, and $\boldsymbol{r}$.

Let
$\boldsymbol{\alpha}$
be the $N$-vector of the natural logarithms of the squared voltage magnitudes, *i.e.*,

$$
\boldsymbol{\alpha}
=
\ln \! \left( \operatorname{diag} \! \left( \boldsymbol{v} \right) \, \boldsymbol{v} \right)
=
\ln \! \left( \boldsymbol{a} \right)
=
2 \ln \! \left( \boldsymbol{v} \right)
\, .
$$ (alp)

where $\ln \! \left( \cdot \right)$ applies componentwise.
As with $\boldsymbol{v}_{ \mathsf{f} }$ and $\boldsymbol{v}_{ \mathsf{t} }$,
we have the $E$-vectors
$\boldsymbol{\alpha}_{ \mathsf{f} } = \boldsymbol{C}_{ \mathsf{f} } \, \boldsymbol{\alpha}$
and
$\boldsymbol{\alpha}_{ \mathsf{t} } = \boldsymbol{C}_{ \mathsf{t} } \, \boldsymbol{\alpha}$.
We can then define the $E$-vectors
$\boldsymbol{\kappa}$ and $\boldsymbol{\rho}$
as

$$
\boldsymbol{\kappa}
=
\boldsymbol{\alpha}_{ \mathsf{f} } + \boldsymbol{\alpha}_{ \mathsf{t} }
\, , \\
$$ (kap)

$$
\boldsymbol{\rho}
=
\boldsymbol{\delta}_{ \mathsf{f} } - \boldsymbol{\delta}_{ \mathsf{t} }
\, .
$$ (rho)

We use `alp`, `kap`, and `rho`
as the respective variable names for
$\boldsymbol{\alpha}$, $\boldsymbol{\kappa}$, and $\boldsymbol{\rho}$.

(prelim:state-vars:interm)=
## Intermediate state variables

We define the *full intermediate state vector* as the concatenation of $\boldsymbol{\alpha}$ and $\boldsymbol{\delta}$ , *i.e.*,

$$
\boldsymbol{x}
=
\left[ \begin{matrix}
    \boldsymbol{\alpha} \\
    \boldsymbol{\delta}
\end{matrix} \right]
\in \mathbb{R}^{ 2N }
\, .
$$ (x)

The *reduced intermediate state vector* is similarly constructed, except that only the phase angles at the non-reference buses are used, *i.e.*,

$$
\boldsymbol{w}
=
\left[ \begin{matrix}
    \boldsymbol{\alpha} \, \\
    \boldsymbol{\delta}_{ \mathsf{v} } \, \\
    \boldsymbol{\delta}_{ \mathsf{q} } \, \\
\end{matrix} \right]
\in \mathbb{R}^{ 2N - N_{ \mathsf{s} } }
\, ,
$$ (w)

following {ref}`the partitioning scheme <prelim:bus-indexing:vecs-n>` for $N$-vectors.
In code, we use `x` and `w` exclusively to refer to $\boldsymbol{x}$ and to $\boldsymbol{w}$, respectively.

(prelim:state-vars:y-u)=
## Canonicalized state vector and its sigma transform

```{margin}
The term "canonicalized" is based on "canonical forms" in
[*Factored solution of nonlinear equation systems*](https://doi.org/10.1098/rspa.2014.0236).
```

The *canonicalized state vector*
$\boldsymbol{y}$
is the concatenation of $\boldsymbol{a}$
and the "interweaving" of $\boldsymbol{k}$ and $\boldsymbol{r}$,
*i.e.*,

$$
\boldsymbol{y}
=
\left[ \begin{matrix}
    \boldsymbol{a}  \, \\
    k_{1}           \, \\
    r_{1}           \, \\
    k_{2}           \, \\
    r_{2}           \, \\
    \vdots          \, \\
    k_{E - 1}       \, \\
    r_{E - 1}       \, \\
    k_{E}           \, \\
    r_{E}           \, \\
\end{matrix} \right]
\in \mathbb{R}^{ N + 2E }
\, ,
$$ (y)

with `y` as the exclusive variable name.

We define
$\boldsymbol{u}$
as the concatenation of $\boldsymbol{\alpha}$
and the "interweaving" of $\boldsymbol{\kappa}$ and $\boldsymbol{\rho}$,
*i.e.*,

$$
\boldsymbol{u}
=
\left[ \begin{matrix}
    \boldsymbol{\alpha} \, \\
    \kappa_{1}          \, \\
    \rho_{1}            \, \\
    \kappa_{2}          \, \\
    \rho_{2}            \, \\
    \vdots              \, \\
    \kappa_{E - 1}      \, \\
    \rho_{E - 1}        \, \\
    \kappa_{E}          \, \\
    \rho_{E}            \, \\
\end{matrix} \right]
\in \mathbb{R}^{ N + 2E }
\, ,
$$ (u)

with `u` as the reserved variable name.

We refer to $\boldsymbol{u}$ as
*the sigma transform of $\boldsymbol{y}$*;
in symbols,
$\boldsymbol{u} = \operatorname{sigma} \! \left( \boldsymbol{y} \right) $.
This transform can be described as an operator that maps
$\boldsymbol{a}$, $\boldsymbol{k}$, and $\boldsymbol{r}$
to
$\boldsymbol{\alpha}$, $\boldsymbol{\kappa}$, and $\boldsymbol{\rho}$.
It is eaasy to show that the following maps are invoked by
$\operatorname{sigma} \! \left( \boldsymbol{y} \right)$:

$$
\begin{matrix}
    \boldsymbol{a} \mapsto \boldsymbol{\alpha}
    & : &
    \ln \! \left( \boldsymbol{a} \right)
    \, , \\
    \boldsymbol{k} \mapsto \boldsymbol{\kappa}
    & : &
    \ln \! \left(
        \operatorname{diag} \! \left( \boldsymbol{k} \right) \, \boldsymbol{k}
        +
        \operatorname{diag} \! \left( \boldsymbol{r} \right) \, \boldsymbol{r}
    \right)
    \, , \\
    \boldsymbol{r} \mapsto \boldsymbol{\rho}
    & : &
    \operatorname{arctan} \! \left(
        \operatorname{diag}^{-1} \! \left( \boldsymbol{k} \right) \, \boldsymbol{r}
    \right)
    \, , \\
\end{matrix}
$$ (sigma-map)

where
$\operatorname{arctan} \! \left( \cdot \right)$
applies componentwise,
and
$\operatorname{diag}^{-1} \! \left( \cdot \right)$
denotes the matrix inverse of
$\operatorname{diag} \! \left( \cdot \right)$.
Since the sigma transform can be thought of as a function
that takes $\boldsymbol{y}$ as input,
it is implemented in code as a function with the call signature `sigma(y)`.

Moreover, we can say that $\boldsymbol{y}$ is computable from $\boldsymbol{u}$ by taking the
*inverse sigma transform* of $\boldsymbol{u}$;
in symbols,
$\boldsymbol{y} = \operatorname{arcsigma} \! \left( \boldsymbol{u} \right)$.
This transform can be described as an operator mapping
$\boldsymbol{\alpha}$, $\boldsymbol{\kappa}$, and $\boldsymbol{\rho}$
to
$\boldsymbol{a}$, $\boldsymbol{k}$, and $\boldsymbol{r}$.
The following maps are invoked by
$\operatorname{arcsigma} \! \left( \boldsymbol{u} \right)$:

$$
\begin{matrix}
    \boldsymbol{\alpha} \mapsto \boldsymbol{a}
    & : &
    \exp \! \left( \boldsymbol{\alpha} \right)
    \, , \\
    \boldsymbol{\kappa} \mapsto \boldsymbol{k}
    & : &
    \operatorname{sqrt} \! \left( \exp \! \left( \boldsymbol{\kappa} \right) \right)
    \cos \! \left( \boldsymbol{\rho} \right)
    \, , \\
    \boldsymbol{\rho} \mapsto \boldsymbol{r}
    & : &
    \operatorname{sqrt} \! \left( \exp \! \left( \boldsymbol{\kappa} \right) \right)
    \sin \! \left( \boldsymbol{\rho} \right)
    \, , \\
\end{matrix}
$$ (arcsigma-map)

where
$\exp \! \left( \cdot \right)$
and
$\operatorname{sqrt} \! \left( \cdot \right)$
indicate exponentiating and taking the square-root componentwise, respectively.
Since the inverse sigma transform can be thought of as a function
that takes $\boldsymbol{u}$ as input,
it is implemented in code as a function with the call signature `arcsigma(y)`.
