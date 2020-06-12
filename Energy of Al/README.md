__Note: The [full code and guide are referenced here](https://icme.hpc.msstate.edu/mediawiki/index.php/ICME-QM) and this folder is created for personal learning and documentation only.__

# Investigate how energy of Al changes with volume
- Plot E-V curves for Al in different structures (using DFT and VASP)
- Investigate how changing XC functional, kpoints, smearing technique, and energy cutoff changes the results 

## Structures 
- BCC
- FCC
- HCP
- Diamond

## Steps 
1. POSCAR generation
    * To plot E-V curve positions of atoms at different volumes of the crystal structures, we need to generate the POSCAR files at different volumes and then run DFT for each volume itself. 
    * Ranges lattice parameter from 3.5 to 4.5 with 0.1 as interval 
```
for a in `seq -w 3.5 0.1 4.5`
```

2. Execute VASP with INCAR, KPOINTS and POTCAR files also in the directory 
    * Aluminium's POTCAR used is from VASP, using PAW PBE XC functional


3. From output files OUTCAR and OSZICAR, we can obtain the energy, volume and cpu time. Prepare the data files from these values.

4. Curve fitting of E-V data using Murnaghan's equation of state and extracts the equilibrium volume/lattice parameter, bulk modulus, and equilibrium energy values to a SUMMARY file. 


## Curve fitting 
- [evfit](https://icme.hpc.msstate.edu/mediawiki/index.php/Evfit)


