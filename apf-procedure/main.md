(apf-proc:main)=
# The APF Procedure

```{caution}
This page is under construction
```

The APF procedure consists of three stages.
In the first stage, we anticipate generator injections using information from $\mathcal{S}$ and $\mathcal{D}$.
Then, we invoke the factored power flow equations at the non-slack buses,
and from them solve for the corresponding bus voltages.
In the last stage, we adjust the slack-bus generator injections by taking into account the computed bus voltages.

```{tableofcontents}
```
