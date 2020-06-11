## Bulk Relaxations

### Setup on INCAR
For Al 
```
PREC = Accurate
ISMEAR = 1 ! --> Al is metallic
ENCUT = 318
EDIFF = 1e-6

ISIF = 3
NSW = 100
IBRION = 2
```
### Procedures 
1. Do relaxation as per above INCAR
2. Copy CONTCAR output to POSCAR and run again
    * Use cleanup.sh to clear output files 
    * ISIF = 2 in this step is not necessary 
3. Run again but with tetrahedron method 
    * ISMEAR = -5 to get very accurate energies 
    * Never use energies from a relaxation run if the volume change is allowed (as per ISIF)


## Volumetric relaxation 
Best to use ionic relaxations (ISIF = 2) at different volumes and plot energy vs volume curve
- Fit to equation of state + obtain eqm volume and energy
- Can use evfit program to fit 

At the end of relaxation (ISIF = 2/3), if there is a convergence to parameters specified, the stdout will read: 
```
reached required accuracy - stopping structural energy minimisation
```

### A) Energy-Volume curve automation script 
```
totT=0

for a in `seq -w 3.5 0.1 4.5`   # range of lattice parameter
do
echo "a= $a" 

cat >POSCAR <<!
Al FCC
1.0
$a  0.00  0.00                  # 
0.00   $a 0.00
0.00   0.00  $a
4
Direct
0.00000000 0.00000000 0.0000000
0.50000000 0.00000000 0.5000000
0.50000000 0.50000000 0.0000000
0.00000000 0.50000000 0.5000000
!

mpirun -np <no. of processors> <path of VASP executable>

V=`grep volume/ion OUTCAR|awk '{print $5}'`
E=`tail -n1 OSZICAR | awk '{ print $5/4}'`
T=`grep 'CPU time' OUTCAR |awk '{print $6}'`
totT=`echo $totT $T|awk '{print $1+$2}'`
echo $a $T >> TvsA
echo $a $E >> EvsA
echo $V $E >> EvsV
./cleanvaspfiles

done
```
- Uses lattice parameters from 3.5 -> 4.5 A (10 points)
- Extracts final volume/ion, energy/atom, time for calculation 
- Makes 2 output files of 2 columns: Energy/ion for each lattice param, energy/ion for each volume/ion

### B) Curve Fitting script 

```
cat <<! | evfit
FCC
4        
EvsA
evfit.4
!

Emin=`grep Emin evfit.4|awk '{print $2}'`
A0=`grep A0 evfit.4|awk '{print $2}'`
K0=`grep A0 evfit.4|awk '{print $4}'`

tail +5 evfit.4|awk '{print $1,"\t", $2,"\t", $3}' > grace

echo "Data for Al in FCC" >> SUMMARY
echo "=================================================" >> SUMMARY
echo "Equilibrium Energy per Atom    = $Emin">>SUMMARY
echo "Equilibrium lattice constant   = $A0">>SUMMARY
echo "Bulk Modulus                   = $K0">>SUMMARY
echo "Total CPU time (sec)           = $totT">>SUMMARY
```
- Use evfit program to fit data to Murnaghan's equation of state
- From the fitting we can derive: 
    * From Emin = onwards, the script extracts data from evfit output (E0 = eqm energy/atom, A0 = eqm lattice constant, K0 = Bulk modulus)

[For A and B, recommended: Read full guide here.](https://icme.hpc.msstate.edu/mediawiki/index.php/ICME-QM)