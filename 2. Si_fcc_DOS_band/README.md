# Calculation of DOS for Si FCC

With help from 
- https://www.vasp.at/wiki/index.php/Fcc_Si_DOS#Calculation
- https://www.youtube.com/watch?v=ohDguVFDPlc
- http://image.sciencenet.cn/olddata/kexue.com.cn/bbs/upload/20083418325986.pdf

### Following the guide mainly from the YouTube video with the following 4 steps:

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
- 21 for higher density packing compared to original 11 used in FCC structural determination


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

## Step 1: Structural relaxation
- After getting the output files, use CONTCAR output as new POSCAR for the remaining steps 2 to 4. 
- CONTCAR contains the new relaxed structure of Si FCC

## Step 2: Single point self-consistency calculations 
- Use relaxed CONTCAR structure for POSCAR
- INCAR settings edits: No ionic relaxation and EDIFF for higher precision results in convergence 
- Key outputs: CHGCAR and WAVECAR
- WAVECAR: Wave function --> Copy to Step 3 DOS
- CHGCAR: Charge density --> Copy to Step 3 DOS

## Step 3: DOS calculations 
- Use CHGCAR and WAVECAR output from Step 2
- INCAR edits: 
    * Dont start from scratch so ISTART = 1
    * ICHARG = 11 to read charge density from the CHGCAR file in directory instead 
    * ISMEAR stick to -5 for tetrahedron method which is especially suitable for DOS calculations 
    * Set LORBIT = 11 to output DOSCAR and PROCAR (which is partial DOS); 10 means spdf orbitals, 11 means s,px,py,pz... ortitals (l and m partial DOS)
    * NEDOS = 1000 for the number of points of DOS
    * EMIN and EMAX for energy range of DOS
    * NBANDS for number of bands
    * Both E range and NBANDS can be obtained in OUTCAR or EIGENVAL of scf calculations; but this can be neglected for VASP to take default values; refer below on how to get from EIGENVAL
- KPOINTS: Stick to 21 21 21
- POSCAR: Stick to same relaxed structure


```
# Example snippet from EIGENVAL output from scf 
 fcc Si                                  
      8    286      8
 
  0.0000000E+00  0.0000000E+00  0.0000000E+00  0.1079797E-03
    1       -6.194068   1.000000
    2        5.643200   1.000000
    3        5.643217   1.000000
    4        5.643217   1.000000
    5        8.203201   0.000000
    6        8.203218   0.000000
    7        8.203218   0.000000
    8        8.783224   0.000000

```
- This means there are 8 bands and 286 kpoints 
- And each segment consists of 8 bands for each kpoint ... 

## Step 4: Band structure calculations 
- Use CHGCAR from step 3, can use WAVECAR from step 2 too (or step 3 as well?)
- INCAR edits:
    * ICHARG = 11 to read from ICHARG file 
    * LORBIT = 11 for both partial and full DOS

- KPOINTS edits: Cannot use uniform grid of kpoints but have to specify kpoints path 

```
k-points for bandstructure L-G-X-U K-G
 10
line
reciprocal
  0.50000  0.50000  0.50000    1
  0.00000  0.00000  0.00000    1

  0.00000  0.00000  0.00000    1
  0.00000  0.50000  0.50000    1

  0.00000  0.50000  0.50000    1
  0.25000  0.62500  0.62500    1

  0.37500  0.7500   0.37500    1
  0.00000  0.00000  0.00000    1

```
- 10 = 10 points between each line 
- First line is L to G, then G to X, then X to U, then K to G
- All points with weight 1
- Need to understand KPOINTS more fundamentally...
