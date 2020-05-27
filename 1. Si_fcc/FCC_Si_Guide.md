# Lattice constant optimization for FCC Si 

## Steps 
1. Create the INCAR, POSCAR, and KPOINTS files 
2. Get the POTCAR for Si (from PP library, concatenate if molecule and be consistent with POSCAR sequence)
3. Create a bash script in pbs format 

## PBS script 

Template 

```
#! /bin/bash
BIN=/path/to/your/vasp/executable
rm WAVECAR SUMMARY.fcc
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
echo "a= $i" ; mpirun -np 2 $BIN
E=`awk '/F=/ {print $0}' OSZICAR` ; echo $i $E  >>SUMMARY.fcc
done
cat SUMMARY.fcc

```
- For this script, it assumes POSCAR file is not present so there is a for loop to construct the POSCAR.
- And for every loop, it runs VASP with mpirun -np 2 first then from the OSZICAR file extract the values and adds a line to SUMMARY.fcc 
- Lastly, concatenate the SUMMARY.fcc file 

