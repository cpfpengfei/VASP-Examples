## VASP Introduction
VASP uses DFT to solve the quantum problem in materials with periodic boundary conditions and the pseudopotential method with a plane waves basis set. 
- Can model systems with max num atoms from 100 - 200 

## Exchange-Correlation functionals
- Functionals modelled on uniform-electron-gas (UEG): Correlation energy has been calculated by Monte Carlo methods for wide range of densities and are parametrized to yield a density functional 
- LDA functional: Pretend an inhomogeneous electronic density locally behaves like a homogeneous electron gas 
- Other functionals: GGA, meta-GGA, van der Waals

More theory here: https://www.vasp.at/wiki/images/5/5d/VASP_lecture_Basics.pdf


## VASP Input Files 

### INCAR
- The control parameters that define the calculation 
- Global break condition, energy cutoff, smearing, ionic and geometric relaxation params 
- Specify what to do: scf calculations, DOS, dielectric properties
- Params to fulfill requirements concerning required precision, requested convergence, calculation time, etc 

### POSCAR
- Periodic simulation cell / Contains information on unit cell and basis atoms
- Information on the system's geometry or structure 

Example:
```
fcc:  Ni
3.53
0.5 0.5 0.0
0.0 0.5 0.5
0.5 0.0 0.5
Ni
1
Selective Dyn
Cartesian
0 0 0  T T T
```
1. Header (comment).
2. Overall scaling constant.
3. Bravais matrix 
4. Name(s) of the atom(s).
5. Number of the atoms (of each atom type).
6. (optional: selective dynamics).
7. Specifies which coordinate system is used ("cartesian" or "direct").
8. Onwards: Positions of the atoms.

### KPOINTS
- k-point mesh 
- Determines the sampling of the first Brillouin Zone 

Example:
```
Automatic mesh
0
G (M)
4 4 4
0.  0.  0.

```
1. Header (comment).
2. Specifies the k mesh generation type. (?)
3. Gamma -centered (Monkhorst-Pack) grid. (?)
4. Number of subdivisions in each direction.
5. Optional shift of the mesh.


### POTCAR
- Pseudopotential (PP) file: Information on PP and XC functional to run the calculation 
- Replaces nucleus and core electrons with fixed effective potential
- From Pseudopotential library in VASP (cant find online since it is commercial)
- Data required to generate the PP, number of valence electrons, atomic mass, energy cut-off 
- If the cell contains different atomic species, the corresponding POTCAR files will then have to be concatenated - in the **same order as the atomic species given in the POSCAR**

```
# Example to concatenate Al with Mg

cat POTCAR_Al POTCAR_Mg >> POTCAR

```

## Output Files 
stdout
- Command line standard outputs will show each iteration, E total energy, and dE change of total energy;
- The final E0 total ground state energy displayed 
- All these data are also written in the OSZICAR 

OSZICAR
- Data of electronic steps and ionic steps 

CONTCAR 
- Position of the system after calculation (POSCAR after calculation)

OUTCAR
- Complete run output including the input data

CHGCAR
- Charge density of the system after run 


## Systems 

***

## Well-converged calculations 
1. Choose XC functional:
- Accuracy: LDA < GGA < Hybrid 

2. KPOINTS
- Do Kpoint convergence; accuracy increases with number of kpoints 
- For metals, choose a smearing technique (ISMEAR in INCAR)
    * Tetrahedron method = Good for very accurate total energy calculations 
    * Gaussian, Methfessel-Paxton etc = Good for ionic and geometric relaxations 

    * ISMEAR = N (N>0): method of Methfessel-Paxton order N
    * ISMEAR = 0 for Gaussian, -1 for Fermi, -2 for partial occupancies read from WAVECAR or INCAR
    * ISMEAR = -5 for tetrahedron method (for DOS calculations + very accurate total energy calculations (no relaxation in metals))

3. Energy Cutoff (ENCUT)
- Controls the completeness of the planewave basis set 

4. Pseudopotentials (POTCAR default)
- Replaces nucleus and core electrons with fixed effective potential 

