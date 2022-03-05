(prelim:bus-indexing)=
# Bus indexing

We assume that there is a consistent roster of buses.
For an arbitrary $N$-element sequence of quantities corresponding to the buses,
the entries are partitioned such that
the first $N_{ \mathsf{s} }$ elements correspond to the SR bus,
the neext $N_{ \mathsf{v} }$ to the PV buses,
and
the last $N_{ \mathsf{q} }$ to the PQ buses.
The following examples illustrate this.

(prelim:bus-indexing:vecs-n)=
## Vectors of length $N$

The  $N$-vector of nodal voltage magnitudes
(see {ref}`prelim:state-vars:vm-va`)
is partitioned as

$$
\boldsymbol{v} =
\left[ \begin{matrix}
    \boldsymbol{v}_{ \mathsf{s} } \, \\
    \boldsymbol{v}_{ \mathsf{v} } \, \\
    \boldsymbol{v}_{ \mathsf{q} } \, \\
\end{matrix} \right],
$$

This can be written in MATLAB as

```matlab
vm = [vm_s; vm_v; vm_q];
```

or in NumPy as

```python
vm = np.concatenate((vm_s, vm_v, vm_q), axis=0)
```

where `vm` is the dedicated variable name for $\boldsymbol{v}$.

(prelim:bus-indexing:mats-nr-nc)=
## Matrices with $N$ rows and $N$ columns

The $N \times N$ system bus admittance matrix $\mathbf{Y}$
(see {ref}`prelim:syst-param-mats:Y`)
is partitioned as

$$
\mathbf{Y}
=
\left[ \begin{matrix}
    \mathbf{Y}_{ \mathsf{s}, \mathsf{s} }
    & \mathbf{Y}_{ \mathsf{s}, \mathsf{v} }
    & \mathbf{Y}_{ \mathsf{s}, \mathsf{q} } \, \\
    \mathbf{Y}_{ \mathsf{v}, \mathsf{s} }
    & \mathbf{Y}_{ \mathsf{v}, \mathsf{v} }
    & \mathbf{Y}_{ \mathsf{v}, \mathsf{q} } \, \\
    \mathbf{Y}_{ \mathsf{q}, \mathsf{s} }
    & \mathbf{Y}_{ \mathsf{q}, \mathsf{v} }
    & \mathbf{Y}_{ \mathsf{q}, \mathsf{q} } \, \\
\end{matrix} \right]
$$

where

* $\mathbf{Y}_{ \mathsf{s}, \mathsf{s} }$ is the $N_{ \mathsf{s}} \times N_{ \mathsf{s}}$
  submatrix containing self admittances of and mutual admittances between SR buses,
* $\mathbf{Y}_{ \mathsf{s}, \mathsf{v} }$ is the $N_{ \mathsf{s}} \times N_{ \mathsf{v}}$
  submatrix containing mutual admittances between SR and PV buses,
* $\mathbf{Y}_{ \mathsf{s}, \mathsf{q} }$ is the $N_{ \mathsf{s}} \times N_{ \mathsf{s}}$
  submatrix containing mutual admittances between SR and PQ buses,
* $\mathbf{Y}_{ \mathsf{v}, \mathsf{s} }$ is the $N_{ \mathsf{v}} \times N_{ \mathsf{s}}$
  submatrix containing mutual admittances between PV and SR buses,
* $\mathbf{Y}_{ \mathsf{v}, \mathsf{v} }$ is the $N_{ \mathsf{v}} \times N_{ \mathsf{v}}$
  submatrix containing self admittances of and mutual admittances between PV buses,
* $\mathbf{Y}_{ \mathsf{v}, \mathsf{q} }$ is the $N_{ \mathsf{v}} \times N_{ \mathsf{q}}$
  submatrix containing mutual admittances between PV and PQ buses,
* $\mathbf{Y}_{ \mathsf{q}, \mathsf{s} }$ is the $N_{ \mathsf{q}} \times N_{ \mathsf{s}}$
  submatrix containing mutual admittances between PQ and SR buses,
* $\mathbf{Y}_{ \mathsf{q}, \mathsf{v} }$ is the $N_{ \mathsf{q}} \times N_{ \mathsf{v}}$
  submatrix containing mutual admittances between PQ and PV buses,
  and
* $\mathbf{Y}_{ \mathsf{q}, \mathsf{q} }$ is the $N_{ \mathsf{q}} \times N_{ \mathsf{q}}$
  submatrix containing self admittances of and mutual admittances between PQ buses.

The partitioning can be written in MATLAB as

```matlab
Y = [[Y_s_s;  Y_v_s;  Y_q_s], ...
     [Y_s_v;  Y_v_v;  Y_q_v], ...
     [Y_s_q;  Y_v_q;  Y_q_q]];
```

or in NumPy as

```python
Y = numpy.block([
        [Y_s_s, Y_v_s, Y_q_s],
        [Y_s_v, Y_v_v, Y_q_v],
        [Y_s_q, Y_v_q, Y_q_q],
    ])
```

where `Y` is the dedicated variable name for $\mathbf{Y}$.

(prelim:bus-indexing:mats-nr)=
## Matrices with $N$ rows

The $N \times G$ generator connection matrix $\boldsymbol{C}_{ \mathsf{g} }$
(see {ref}`prelim:connection-mats:Cg`)
is partitioned as

$$
\boldsymbol{C}_{ \mathsf{g} }
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{g}, \mathsf{s} } \, \\
    \boldsymbol{C}_{ \mathsf{g}, \mathsf{v} } \, \\
    \boldsymbol{C}_{ \mathsf{g}, \mathsf{q} } \, \\
\end{matrix} \right]
$$

where

* $\boldsymbol{C}_{ \mathsf{g}, \mathsf{s} }$ is the $N_{ \mathsf{s} } \times G$ connection submatrix of generators to the SR buses,
* $\boldsymbol{C}_{ \mathsf{g}, \mathsf{v} }$ is the $N_{ \mathsf{v} } \times G$ connection submatrix of generators to the PV buses,
  and
* $\boldsymbol{C}_{ \mathsf{g}, \mathsf{q} }$ is the $N_{ \mathsf{q} } \times G$ connection submatrix of generators to the PQ buses.

This "vertical stacking" can be written in MATLAB as

```matlab
Cg = [[Cg_s], [Cg_v], [Cg_q]];
% or, more concisely via the semicolon notation,
Cg = [Cg_s; Cg_v; Cg_q];
```

and in NumPy as

```python
Cg = numpy.block([[Cg_s], [Cg_v], [Cg_q]])
# or, using `vstack()`,
Cg = np.vstack((Cg_s, Cg_v, Cg_q))
```

where `Cg` is the reserved variable name for $\boldsymbol{C}_{ \mathsf{g} }$.

(prelim:bus-indexing:mats-nc)=
## Matrices with $N$ columns

The $E \times N$ from-side branch connection matrix $\boldsymbol{C}_{ \mathsf{f} }$
(see {ref}`prelim:connection-mats:Cf-Ct`)
is partitioned as

$$
\boldsymbol{C}_{ \mathsf{f} }
=
\left[ \begin{matrix}
    \boldsymbol{C}_{ \mathsf{f}, \mathsf{s} }
    & \boldsymbol{C}_{ \mathsf{f}, \mathsf{v} }
    & \boldsymbol{C}_{ \mathsf{f}, \mathsf{q} } \,\, \\
\end{matrix} \right]
$$

where, for a consistent roster of branches defined by sending-end ('from' side)
and receiving-end ('to' side) buses,

* $\boldsymbol{C}_{ \mathsf{f}, \mathsf{s} }$ is the $E \times N_{\mathsf{s} }$ submatrix encoding which among the SR buses are at the 'from' side,
* $\boldsymbol{C}_{ \mathsf{f}, \mathsf{v} }$ is the $E \times N_{\mathsf{v} }$ submatrix encoding which among the PV buses are at the 'from' side,
  and
* $\boldsymbol{C}_{ \mathsf{f}, \mathsf{q} }$ is the $E \times N_{\mathsf{q} }$ submatrix encoding which among the PQ buses are at the 'from' side.

This "horizontal stacking" can be written in MATLAB as

```matlab
Cf = [Cf_s, Cf_v, Cf_q]
```

and in NumPy as

```python
Cf = numpy.block([Cf_s, Cf_v, Cf_q])
# or, using `hstack()`,
Cf = np.hstack((Cf_s, Cf_v, Cf_q))
```

where `Cf` is the reserved variable name for $\boldsymbol{C}_{ \mathsf{f} }$.
