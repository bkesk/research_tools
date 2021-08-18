# Workflow

## Pattern
After each computation that takes more than roughly a second, write to disk.
For example,
- Run simulation, write raw data to disk.
- Gather statistics, write averages, covariances to disk.
- Plot trends, write plots to disk.

A workflow is the directed graph codifying this process.

# Workflow tools
The point of these tools is to codify the workflow for a given project or calculation, automate it, and make it self-documenting and reproducible.

## Snakemake

[Snakemake](https://snakemake.readthedocs.io/en/stable/index.html) start as a simple workflow tool and scale up and a sophisticated one. See the Docs for more examples. [Here's a real-life example.](snakemake_example.smk). In that example, using
```
snakemake results/nv_ROKS_{lda,pbe,pbe0}_c-1_bccpv{d,t}z_s{63,127}_xewald_spin{0,2}/mcscf_casscf_si{0,1}_stspec/mcscfcalc.chk -j100
```
would run an SCF caluclation with a set of options and then run several MCSCF calculations on each one. 

Usinging the `-n` option is useful for checking before you launch 1000 jobs.

There's also support for cloud computing, and its customizable. You can specify the range of valid options, or name a set of options in the `Snakefile`.