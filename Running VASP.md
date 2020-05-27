## Generally

1. Create the INCAR, POSCAR, and KPOINTS files 
2. Get the POTCAR for Si (from PP library, concatenate if molecule and be consistent with POSCAR sequence)
3. Create a bash script in pbs format 

## General command 
mpi run np -2 /PATH
- Run in the directory with the 4 files (INCAR, POSCAR, KPOINTS, and POTCAR)
- np -2 means parallel computing with 2 cores 
