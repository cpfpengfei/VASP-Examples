# INCAR
Various INCAR tags and what they mean / if they are useful.

**ISTART: determines if to read WAVECAR file or not**
- 0 means start from scratch 
- 1 means continue job, read orbitals from WAVECAR
- and so on https://www.vasp.at/wiki/index.php/ISTART

**PREC: Precision**
- Usually use Accurate

**ENCUT: The cutoff energy for planewave basis set in eV**
- Default value: Largest ENMAX of the POTCAR file 
- For geometric relaxations (changing cell size), to accommodate cell volume changes, set ENCUT = 1.3 * (Largest ENMAX)
- 240 for Si FCC

**ICHARG**
- 11 for eigenvalues or DOS given charge density from CHGCAR file in the directory

**ISMEAR: Smearing**
- BZ integration method for relaxation
- 0, 1 or -5 (tetrahedron method most accurate)
- For insulators and semiconductors, NEVER use ISMEAR > 0 
    * Always use tetrahedron method (ISMEAR = -5) or if cell too large use Gaussian (ISMEAR = 0)
- For metals, choose a smearing technique (ISMEAR in INCAR)
    * Tetrahedron method = Good for very accurate total energy calculations 
    * Gaussian, Methfessel-Paxton etc = Good for ionic and geometric relaxations 

    * ISMEAR = N (N>0): method of Methfessel-Paxton order N
    * ISMEAR = 0 for Gaussian, -1 for Fermi, -2 for partial occupancies read from WAVECAR or INCAR
    * ISMEAR = -5 for tetrahedron method (for DOS calculations + very accurate total energy calculations (no relaxation in metals))

**LREAL: Use of reciprocal or real space**
- Default = .False. which uses the reciprocal space 
- LREAL = AUTO - Recomended for systems > 20 atoms --> Use Real Space 

**NSW: Number of ionic steps taken in minimization**
- Make it odd / default = 0 ?

**ISIF**
- Determines which degrees of freedom (ions, cell volume, cell shape) are allowed to change 
- To only optimize ionic positions within cell --> ISIF = 2
- For cell volume and cell shape optimizations ISIF = 3
- Full table: 

| ISIF | Calculate |               | Degrees-of-freedom |            |             |
|------|:---------:|:-------------:|:------------------:|:----------:|:-----------:|
|      | forces    | Stress tensor | positions          | cell shape | cell volume |
| 0    | yes       | no            | yes                | no         | no          |
| 1    | yes       | trace only    | yes                | no         | no          |
| 2    | yes       | yes           | yes                | no         | no          |
| 3    | yes       | yes           | yes                | yes        | yes         |
| 4    | yes       | yes           | yes                | yes        | no          |
| 5    | yes       | yes           | no                 | yes        | no          |
| 6    | yes       | yes           | no                 | yes        | yes         |
| 7    | yes       | yes           | no                 | no         | yes         |

[VASP Documentation](https://www.vasp.at/wiki/index.php/ISIF)

**IBRION**
- Determines how ions are updated and moved 
- If away from minimum --> IBRION = 2 (conjugate gradient)
- If close to min --> IBRION = 1 (quasi-newton)
