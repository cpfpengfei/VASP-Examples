# KPOINTS
- Simulation is done at the reciprocal space; KPOINTS useful for periodic boundary conditions for solid state materials 
- KPOINTS file specify the Bloch vectors that will be used to sample the first BZ 

## Monkhorst-Pack mode
Automatic grid generation mode by providing the numbers for the subdivisions 
- With the first 3 lines and the last line constant for most cases, they key difference for this mode is to select the N_1, N_2, and N_3 in the 4th line. 
- Usually N_1 : N_2 : N_3 are selected to ~ b1 : b2 : b3 where b are the reciprocal lattice vectors
- For most lattices (cubic, tetragonal, and simple orthorhombic), this is basically N_1 : N_2 : N_3 ~ 1/a1 : 1/a2 : 1/a3 where a are the Bravais lattice vectors of the unit cell! 


**Example: Differences in Si lattice params determination**

```
k-points comment line 
 0
Monkhorst Pack
 11 11 11
 0  0  0
```

vs for DOS calculations
```
k-points comment line 
 0
Monkhorst Pack
 21 21 21  ! --> subdivisions N_1, N_2, and N_3 along reciprocal lattice vectors 
 0  0  0 ! --> optional shift of the mesh (s_1, s_2, s_3)
```
- Uses a higher value in the 4th row for a higher density packing. 


## Reciprocal mode (Cartesian mode)
```
k-points comment line
0
Cartesian
0.25 0.00 0.00 ! --> g1
0.00 0.25 0.00 ! --> g2
0.00 0.00 0.25 ! --> g3
0.00 0.00 0.00 ! --> shift for g1, g2, g3
```
- Here, lines 4-6 specify the generating vectors where g = g1 + g2 + g3 = x1b1 + x2b2 + x3b3 in which b = reciprocal basis vectors and x are the values we should provide in lines 4-6
- The last line (7th): specify the shift (s1 - s3) of the k-point lattice wrt the origin (in coordinates wrt the generating lattice vectors for each corresponding g)
- Therefore, the resulting mesh of k-points: 
```
k = g1(n1 + s1) + g2(n2 + s2) + g3(n3 + s3)
```

## Equivalents in both modes
```
Automatic generation
0
Reciprocal
 0.25 0.00 0.00
 0.00 0.25 0.00
 0.00 0.00 0.25
 0.50 0.50 0.50
```
^is the same as:

```
Automatic generation
0
Monkhorst-pack
 4 4 4
 0 0 0
```
