## Job script 

**Looping through POSCAR with different lattice constants**
- For example to loop lattice constant in POSCAR from 3.6 to 4.0; this is done to shroten the time of calculation and simplify the file inside the folder 
- Can plot the energy values against lattice constants in the OUTCAR using gnuplot (or excel)

**Example, where lattice parameter $i is varied from 3.5 to 4.3 in the POSCAR**
```
for i in  3.5 3.6 3.7 3.8 3.9 4.0 4.1 4.2 4.3 ; do
	cat >POSCAR <<!
fcc:
   $i
 0.5 0.5 0.0
 0.0 0.5 0.5
 0.5 0.0 0.5
   1
cartesian
0 0 0
!
echo "a= $i"

mpirun  $VASP

done

```