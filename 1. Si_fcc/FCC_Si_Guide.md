# Lattice constant optimization for FCC Si 

## Steps 
- Set the INCAR, POSCAR, and KPOINTS files 
- Get the POTCAR for Si 
- Create a bash script in pbs format 

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