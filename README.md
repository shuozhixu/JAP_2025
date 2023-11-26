# Vacancy formation energies in refractory metals and alloys

## Foreword

The purpose of this project is to calculate the vacancy formation energies in 5 quaternaries, 10 ternaries, 10 binaries, and 5 pure metals. The first 15 alloys are shown in Table 1 of [this paper](http://dx.doi.org/10.1016/j.jallcom.2023.170556); note that the quinary, MoNbTaVW, has been studied in another project. The 10 binaries are MoNb, MoTa, MoV, MoW, NbTa, NbV, NbW, TaV, TaW, and VW. The 5 pure metals are Mo, Nb, Ta, V, and W. All alloys are equal-molar. In total, there are 30 metals and alloys, and we will run 1480 VASP calculations, i.e., $5\times 3 = 15$ for pure metals, $5\times 73 = 365$ for quaternaries, $10\times 55 = 550$ for ternaries, and $10\times 55 = 550$ for binaries. Based on these simulations, we will obtain $5+5\times 4+10\times 3+10\times 2 = 75$ vacancy formation energies. Then we can try training a machine learning model to predict these energies from local chemical compositions.

To run a VASP simulation on OSCER, we need five files: `vasp.batch`, `INCAR`, `KPOINTS`, `POTCAR`, and `POSCAR`. The first three files are the same for most simulations (except those for a single ion in a vacuum) in this project, and they can be found in the `common/` directory in this GitHub repository. The fourth file, `POTCAR`, can be made from one or more of `POTCAR_Mo`, `POTCAR_Nb`, `POTCAR_Ta`, `POTCAR_V`, and `POTCAR_W`, which can also be found in the `common/` directory in this GitHub repository. The last file, `POSCAR`, differs for different types of materials and/or simulations.

Note: By default, one node and 64 cores are used. If needed, we can increase the numbers of nodes and/or cores in `vasp.batch`, then we may want to modify `INCAR` as well. It is suggested that `KPAR` is set as the number of nodes while `NCORE` the number of cores per node.

If you use 64 cores for each job, please make sure not to run more than 8 jobs in total at any time, such that other students can also run simulations. Also, pay attention to the amount of data in your $HOME. They can build up quickly. Once the data is more than 20 GB, you won't be able to run anything.

## Pure metals

Files needed for pure metals can be found in the `common/`, `pure_metals/` and `pure_metals_vacuum/` directories in this GitHub repository.

### Mo

#### POTCAR

The `POTCAR` file for Mo can be obtained by

	cp POTCAR_Mo POTCAR
	
#### Removal of one ion
	
Create a directory named `Mo` on OSCER, and copy `vasp.batch`, `INCAR`, `KPOINTS`, `POTCAR`, and `POSCAR` there. Do NOT use any files in the `pure_metals_vacuum/` directory in this GitHub repository.

According to line 7 in `POSCAR`, the system contains 54 ions, all of which are of the same type `A`. The specific element is decided by that in `POTCAR`, regardless of what element names are provided in `POSCAR`.

Within the directory `Mo`, create a subdirectory named `1`, then copy the five files into it. Edit `POSCAR` by removing the first ion, i.e., keeping lines 1 to 6 unchanged, and from line 7, it should read

	53
	Direct
	0.66666667  0.33333333  1.00000000
	0.83333333  0.16666667  0.16666667

Note that two changes were made: (i) The number 54 in line 7 was changed to 53; (ii) the original line 9, which corresponds to the original first ion's position, was removed.

Submit the job by

	sbatch vasp.batch
	
Once the job is fnished, check the file `log.mpi`. The job was successfully finished if we see this line

	reached required accuracy - stopping structural energy minimisation

Immediately after that line we may see some warnings, such as

	Warning: ieee_invalid is signaling
	Warning: ieee_divide_by_zero is signaling
	Warning: ieee_underflow is signaling
	Warning: ieee_inexact is signaling
	FORTRAN STOP
	
These warnings are fine. Do not worry about them.

However, if you don't see the line `reached required accuracy` and you are sure the job was finished, that would indicate that the job failed. Consult with Subah or me.

Once the job was successfully finished, open the file `OSZICAR` and go to the last line, where we will find the value of `F`, like this:

	F= -.XXXX

That negative number is the energy of the system containing a vacancy. Let's call it _E_<sub>defective</sub>.

#### No removal of any ion

Create a subdirectory `0`. Do not delete any ion from `POSCAR`. Again, do NOT use any files in the `pure_metals_vacuum/` directory in this GitHub repository. Submit the job. Record that negative number in `OSZICAR`, which is the energy of the intact structure. Let's call it _E_<sub>perfect</sub>.

#### A single ion in a vacuum

Create a subdirectory `vac`. Copy there the four files in the `pure_metals_vacuum/` directory in this GitHub repository, as well as `POTCAR` which is from

	cp POTCAR_Mo POTCAR

Submit the job. This is to calculate the energy of a single Mo ion in a vacuum. Let's call it _E_<sub>metal</sub>.

#### Vacancy formation energy

The vacancy formation energy can be calculated using Equation 1 of [this paper](https://doi.org/10.1016/j.rinma.2022.100320).

### Other four pure metals

The other four pure metals are: Nb, Ta, V, and W.

Follow the same procedures for Mo, we can calculate the vacancy formation energies in other four pure metals. Don't forget to use the correct `POTCAR` and _E_<sub>metal</sub> for each metal.

Once you are done with this part, email the vacancy formation energies to Subah and me. Also let Subah know the "single ion in a vacuum" energies for all five metals. Subah can then use that to calculate the vacancy formation energies for the five elements in the quinary.

## Quaternaries

All quaternaries use the same `POSCAR`, which can be found in the `quaternaries/` directory in this GitHub repository. There are 72 ions in each quaternary.

### MoNbTaV

#### POTCAR

The `POTCAR` file for MoNbTaV can be obtained by

	cat POTCAR_Mo POTCAR_Nb POTCAR_Ta POTCAR_V > POTCAR

#### Removal of the first ion

Create a directory named `MoNbTaV` on OSCER, and copy `vasp.batch`, `INCAR`, `KPOINTS`, `POTCAR`, and `POSCAR` there.

According to lines 6 and 7 in `POSCAR`, the system contains 72 ions:

- The first 18 ions are Mo ions, whose positions are given in lines 9 to 26.
- The next 18 ions are Nb ions, whose positions are given in lines 27 to 44.
- The next 18 ions are Ta ions, whose positions are given in lines 45 to 62.
- The last 18 ions are V ions, whose positions are given in lines 63 to 80.

Within the directory `MoNbTaV`, create a subdirectory named `1`, then copy the five files into it. Edit `POSCAR` by removing the first ion, i.e., keeping lines 1 to 6 unchanged, and from line 7, it should read

	17 18 18 18
	Direct
	0.83333333  0.37500000  0.16666667
	0.33333333  0.75000000  1.00000000

Note that two changes were made: (i) The first number 18 in line 7 was changed to 17; (ii) the original line 9, which corresponds to the original first ion's position, was removed.

Submit the job by `sbatch vasp.batch`.

#### Removal of the second ion

Create another subdirectory named `2` within the directory `MoNbTaV`, then copy the five files into it. Note that the `POSCAR` file should be the one in the directory `MoNbTaV/` from this GitHub repository, NOT the one already modified in the previous calculation.

Then edit `POSCAR` by removing the second ion, i.e., keeping lines 1 to 6 unchanged, and from line 7, it should read

	17 18 18 18
	Direct
	0.66666667  0.75000000  1.00000000
	0.33333333  0.75000000  1.00000000

Note that two changes were made: (i) The first number 18 in line 7 was changed to 17; (ii) the original line 10, which corresponds to the original second ion's position, was removed.

Submit the job by `sbatch vasp.batch`.

#### Removal of another ion

Follow the steps above. Create a series of subdirectories, `3`, `4`, ..., `72`. The last one is `72` because there are 72 ions in total. Each time, edit `POSCAR` accordingly. Note that

- When it comes to the removal of the 19th to 36th ion, line 7 should be modified to `18 17 18 18`. That is because those ions are Nb ions.
- Similarly, when it comes to the removal of the 37th to 54th ion, line 7 should be `18 18 17 18`, because those are Ta ions.
- Similarly, when it comes to the removal of the 55th to 72nd ion, line 7 should be `18 18 18 17`, because those are V ions.

#### No removal of any ion

Create a subdirectory `0`. Do not delete any ion from `POSCAR`. Submit the job. This is to calculate the energy of the intact structure, _E_<sub>perfect</sub>.

In total, we will run 73 VASP calculations for MoNbTaV.

Once all 73 jobs were successfully finished. Rename them by adding the corresponding ion name to each subdirectory name. For example, rename `1` as `Mo-1`, rename `2` as `Mo-2`, and so on. Then starting from `19`, rename `19` as `Nb-1`, rename `20` as `Nb-2`, and so on. Leave the last subdirectory as `0`.

Then in calculating the vacancy formation energy, _E_<sub>defective</sub> for the same type of ion (e.g., Mo) should be the mean value among `Mo-1`, `Mo-2`, ..., `Mo-18`. Eventually, we will have four vacancy formation energies, one for each type of element, in MoNbTaV.

### MoNbTaW

There are five quaternaries in total. MoNbTaW is the second one alphabetically. 

The `POTCAR` file for MoNbTaW can be obtained by

	cat POTCAR_Mo POTCAR_Nb POTCAR_Ta POTCAR_W > POTCAR
	
Follow the same procedures for MoNbTaV, we can obtain the vacancy formation energies for the four elements in MoNbTaW. Again, we need to perform 73 VASP calculations.

Note: the vacancy formation energy for Mo is likely not the same in pure Mo, MoNbTaV, and MoNbTaW.

### Other three quaternaries

Follow the same procedures for MoNbTaV, we can obtain the vacancy formation energies in the other three quaternaries.

Once you are done with this part, email the vacancy formation energies to Subah and me.

## Ternaries

All ternaries use the same `POSCAR`, which can be found in the `ternaries/` directory in this GitHub repository. There are 54 ions in each ternary.

### MoNbTa

The `POTCAR` file for MoNbTa can be obtained by

	cat POTCAR_Mo POTCAR_Nb POTCAR_Ta > POTCAR
	
Follow the same procedures for MoNbTaV, we can obtain the vacancy formation energies for the three elements in MoNbTa. We need to perform 55 VASP calculations here.

### Other nine ternaries

Follow the same procedures for MoNbTa, we can obtain the vacancy formation energies in the other nine ternaries.

Once you are done with this part, email the vacancy formation energies to Subah and me.

## Binaries

All binaries use the same `POSCAR`, which can be found in the `binaries/` directory in this GitHub repository. There are 54 ions in each binary.

### MoNb

The `POTCAR` file for MoNb can be obtained by

	cat POTCAR_Mo POTCAR_Nb > POTCAR
	
Follow the same procedures for MoNbTaV, we can obtain the vacancy formation energies for the two elements in MoNb. We need to perform 55 VASP calculations here.

### Other nine binaries

Follow the same procedures for MoNb, we can obtain the vacancy formation energies in the other nine binaries.

Once you are done with this part, email the vacancy formation energies to Subah and me.

## References

If you use any files from this GitHub repository, please cite

- Sheng Yin, Yunxing Zuo, Anas Abu-Odeh, Hui Zheng, Xiang-Guo Li, Jun Ding, Shyue Ping Ong, Mark Asta, Robert O Ritchie, [Atomistic simulations of dislocation mobility in refractory high-entropy alloys and the effect of chemical short-range order](https://doi.org/10.1038/s41467-021-25134-0), npj Comput. Mater. 12 (2021) 4873
