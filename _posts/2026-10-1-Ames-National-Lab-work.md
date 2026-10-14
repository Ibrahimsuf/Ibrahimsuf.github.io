---
layout: post
title: "My Research at Ames National Lab"
---


In the summer of 2025, I a wrote python package for reducing error in quantum chemistry calculations and presented at the SULI symposium poster session.

A typical problem in computational chemistry is to compute the energy of a system made of many atoms. Unfortunately most algorithms for computing this energy scales highly nonlinearly with the number of atoms. A solution to this is to use the many-body expansion which breaks down these calculations into calculation the energy of a bunch of fragments. This allows roughly linear scaling which speeds up large simulations, however it introduces Basis Set Superposition Error (BSSE). The repo provides tools to calculate this error, determine which contributions to it are the largest and to reduce the error in future calculations.

Check out the repo [here](https://github.com/rmrresearch/bsse_db).
