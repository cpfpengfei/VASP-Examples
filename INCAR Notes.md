# INCAR
Various INCAR tags and what they mean / if they are useful.

ISTART: determines if to read WAVECAR file or not 
- 0 means start from scratch 
- 1 means continue job, read orbitals from WAVECAR
- and so on https://www.vasp.at/wiki/index.php/ISTART

PREC: Precision
- Usually use Accurate

ENCUT: The cutoff energy for planewave basis set in eV
- Default value: Largest ENMAX of the POTCAR file 
- For geometric relaxations (changing cell size), set ENCUT = 1.3 * (Largest ENMAX)
- 240 for Si FCC

ICHARG
- 11 for eigenvalues or DOS given charge density from CHGCAR file in the directory

ISMEAR 
- 0, 1 or -5 (tetrahedron method most accurate)

LREAL: Use of reciprocal or real space 
- Default = .False. which uses the reciprocal space 
- LREAL = AUTO - Recomended for systems > 20 atoms --> Use Real Space 
