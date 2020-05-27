# Calculation of DOS for Si FCC

With help from 
- https://www.vasp.at/wiki/index.php/Fcc_Si_DOS#Calculation
- https://www.youtube.com/watch?v=ohDguVFDPlc

## 4 Steps:
1. Structural relaxation (cell and atoms) --> generate CONTCAR needed for steps 2 - 4
2. Single point self-consistency (SCF) calculation --> generate CHGCAR for steps 3 and 4
3. DOS calculation
4. Band structure calculation 

## INCAR

```
! initialization
System = bulk Si
ISTART = 1; ICHARG = 11      ! charge read from CHGCAR

! electronic optimization
ENCUT = 250
ISMEAR = -5      ! tetrahedron method

! DOS
LORBIT = 11      ! output DOSCAR and PROCAR (total DOS and l,m partial DOS)
NEDOS =  1000    ! number of points of DOS
! EMIN =           ! energy range of DOS
! EMAX = 
! NBANDS =         ! number of bands, obtained in OUTCAR or EIGENVAL of scf calculation
```
- 



## KPOINTS

```
k-points
 0
Monkhorst Pack
 21 21 21
 0  0  0
```
- Why 21?


## POSCAR
```
system Si
5.430
0.5 0.5 0.0
0.0 0.5 0.5
0.5 0.0 0.5
2
cart
0.00 0.00 0.00
0.25 0.25 0.25
```
- This is a bulk Si 2 atoms? in cartesian