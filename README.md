# Vacancies in RMPEA

## Foreword

The purpose of this project is to calculate the vacancy formation energy in 15 refractory multi-principal element alloys (RMPEAs), 10 random binaries, and 5 pure metals. The 15 RMPEAs are all alloys mentioned in [this paper](http://dx.doi.org/10.1016/j.jallcom.2023.170556) except MoNbTaVW, which has been studied. The 10 random binaries are those made of 5 pure metals: Mo, Nb, Ta, V, and W. All alloys are equal-molar. In total, you will run xxx VASP calculations.

To run a VASP simulation on OSCER, you need five files: `vasp.batch`, `INCAR`, `KPOINTS`, `POTCAR`, and `POSCAR`. The only file you need to modify frequently in this project is `POSCAR`, which contains ion positions.

Note: By default, one node and 64 cores are used. If needed, you can increase the numbers of nodes and/or cores in `vasp.batch`, then you may want to modify `INCAR` as well. It is suggested that `KPAR` is set as the number of nodes while `NCORE` the number of cores per node.

## Quaternaries

### MoNbTaV

#### Removal of the first ion

In the directory `relax` in this GitHub repository, there are five files: `vasp.batch`, `INCAR`, `KPOINTS`, `POTCAR`, and `POSCAR`. The only file you need to modify in this project is `POSCAR`, which contains ion positions.

According to lines 6 and 7 in `POSCAR`, the system contains 55 ions:

- The first 11 ions are Mo ions, whose positions are given in lines 9 to 19.
- The next 11 ions are Nb ions, whose positions are given in lines 20 to 30.
- The next 10 (NOT 11) ions are Ta ions, whose positions are given in lines 31 to 40.
- The next 11 ions are V ions, whose positions are given in lines 41 to 51.
- The last 11 ions are W ions, whose positions are given in lines 52 to 62.

On OSCER, create a directory named `relax` and a subdirectory named `1`, then copy the five files into it. Edit `POSCAR` by removing the first ion, i.e., keeping lines 1 to 6 unchanged, and from line 7, it should read

	10 11 10 11 11
	Direct
	0.3333333333	0.3333333333	0.6666666667
	0.6666666667	0.3333333333	0.3333333333

Note that two changes were made: (i) The first number 11 in line 7 was changed to 10; (ii) the original line 9, which corresponds to the original first ion's position, was removed.

Submit the job by `sbatch vasp.batch`. Once the job is fnished, check the file `log.mpi`. The job was successfully finished if you see this line

	reached required accuracy - stopping structural energy minimisation
	
Immediately after that line you may see some warnings, such as

	Warning: ieee_invalid is signaling
	Warning: ieee_divide_by_zero is signaling
	Warning: ieee_underflow is signaling
	Warning: ieee_inexact is signaling
	FORTRAN STOP
	
These warnings are fine. Do not worry about them.

However, if you don't see the line `reached required accuracy` and you are sure the job was finished, that would indicate that the job failed. Email me your `POSCAR` and `log.mpi` files.

#### Removal of the second ion

Create another subdirectory named `2` within the directory `relax`, then copy the five files into it. Note that the `POSCAR` file should be the one from this GitHub repository, NOT the one already modified in the previous calculation.

Then edit `POSCAR` by removing the second ion, i.e., keeping lines 1 to 6 unchanged, and from line 7, it should read

	10 11 10 11 11
	Direct
	0.0000000000	0.3333333333	0.6666666667
	0.6666666667	0.3333333333	0.3333333333

Note that two changes were made: (i) The first number 11 in line 7 was changed to 10; (ii) the original line 10, which corresponds to the original second ion's position, was removed.

Submit the job by `sbatch vasp.batch`.

#### Removal of another ion

Follow the steps above. Create a series of subdirectories, `3`, `4`, ..., `54`. The last one is `54` because there are 54 ions in total. Each time, edit `POSCAR` accordingly. Note that

- When it comes to the removal of the 12th to 22nd ion, line 7 should be modified to `11 10 10 11 11`. That is because those ions are Nb ions.
- Similarly, when it comes to the removal of the 23rd to 32nd ion, line 7 should be `11 11 9 11 11`, because those are Ta ions.
- Similarly, when it comes to the removal of the 33rd to 43rd ion, line 7 should be `11 11 10 10 11`, because those are V ions.
- Similarly, when it comes to the removal of the 44th to 54th ion, line 7 should be `11 11 10 11 10`, because those are W ions.

#### No removal of any ion

Create a subdirectory `0`. Do not delete any ion from POSCAR. Instead, change the number `53` after `MAGMOM` in INCAR to `54`. Submit the job. This is to calculate the energy of the intact structure.

In total, you will run 55 VASP calculations.

Once all 55 jobs were successfully finished. Rename them by adding the corresponding ion name to each subdirectory name. For example, rename `1` as `Mo-1`, rename `2` as `Mo-2`, and so on. Then starting from `12`, rename `12` as `Nb-1`, rename `13` as `Nb-2`, and so on. Leave the last subdirectory as `0`.

## References

If you use any files from this GitHub repository, please cite

- Sheng Yin, Yunxing Zuo, Anas Abu-Odeh, Hui Zheng, Xiang-Guo Li, Jun Ding, Shyue Ping Ong, Mark Asta, Robert O Ritchie, [Atomistic simulations of dislocation mobility in refractory high-entropy alloys and the effect of chemical short-range order](https://doi.org/10.1038/s41467-021-25134-0), npj Comput. Mater. 12 (2021) 4873
