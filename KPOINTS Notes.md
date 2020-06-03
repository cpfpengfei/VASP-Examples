# KPOINTS
- Simulation is done at the reciprocal space; KPOINTS useful for periodic boundary conditions for solid state materials 
- KPOINTS file specify the Bloch vectors that will be used to sample the first BZ 

Differences in Si lattice params determination 

```
k-points
 0
Monkhorst Pack
 11 11 11
 0  0  0
```

vs for DOS calculations
```
k-points
 0
Monkhorst Pack
 21 21 21
 0  0  0
```

- Uses a higher value in the 4th row for a higher density packing. 
- 0 0 0 means dont shift it?