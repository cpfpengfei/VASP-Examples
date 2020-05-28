# INCAR
Various INCAR tags and what they mean / if they are useful.

ISTART: determines if to read WAVECAR file or not 
- 0 means start from scratch 
- 1 means continue job, read orbitals from WAVECAR
- and so on https://www.vasp.at/wiki/index.php/ISTART

PREC: Precision
- Usually use Accurate

ENCUT: The cutoff energy for planewave basis set in eV
- 240 for Si FCC

ICHARG
- 11 for eigenvalues or DOS given charge density from CHGCAR
