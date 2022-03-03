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

*Lorem ipsum*
