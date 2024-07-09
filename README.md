# cif2qewan
cif2qewan.py is a simple python script to create quantum-ESPRESSO and wannier90 inputs from cif files.

## Usage ######################################
  1. Prepare cif2cell. (See for details, https://sourceforge.net/projects/cif2cell/)

  2. Prepare pseudopotentials in PSLibrary.

  3. Download or clone the github repository, e.g.

     ```sh
     % git clone https://github.com/wannier-utils-dev/cif2qewan
     ```

  4. Edit cif2cell_path and pseudo_dir in cif2qewan.py.

  5. Run.

     ```sh
     % python cif2qewan.py **.cif
  
     % pw.x < scf.in > scf.out
      
     % pw.x < nscf.in > nscf.out
      
     % wannier90.x -pp pwscf
      
     % pw2wannier90.x < pw2wan.in
     ```

  6. Edit dis_froz_max in pwscf.win. Recommended value is around EF+1eV ~ EF+3eV.

  7. Wannierize.

     ```sh
     % wannier90.x pwscf
     ```

## Compare band structures of DFT and wannier90 #####
cif2qewan.py prepares band calculation input files in directory "band".

```sh
% cd band

% pw.x < ../scf.in > scf.out

% pw.x < nscf.in > nscf.out

% bands.x < band.in > band.out

% cd ..

% python band_comp.py
```

Then, you can get the band structure plot of DFT and wannier90.

## Compare band energy of DFT and wannier90 #####
cif2qewan.py prepares nscf input file for energy difference.
Wannier90 Hamiltonian should reproduce the band energy on the kmesh for wannierization. (For example, 8x8x8 mesh including gamma point (8 8 8 0 0 0 in QE expression).)
Here, the code checks the energy difference of DFT and wannier90 on the shifted kmesh. (c.f., 8 8 8 1 1 1 in QE expression)

```sh
% cd check_wannier

% pw.x < ../scf.in > scf.out

% pw.x < nscf.in > nscf.out

% cd ..

% python wannier_conv.py

% cat check_wannier/CONV
```

wannier_conv.py calculates the energy differences and outputs the result in check_wannier/CONV.
average diff means $\delta$ defined by

$$
\delta^2 = \frac{1}{N} \sum_{n,k} (e_{n,k}^{DFT} - e_{n,k}^{Wannier})^2.
$$

## Reference ######################################
- [Iron-based binary ferromagnets for transverse thermoelectric conversion,  A. Sakai, S. Minami, T. Koretsune et al. Nature 581 53-57 (2020)](https://doi.org/10.1038/s41586-020-2230-z)

  The database of anomalous Hall conductivity and anomalous Nernst conductivity is generated using cif2qewan.py.

- [Systematic first-principles study of the on-site spin-orbit coupling in crystals Phys. Rev. B 102 045109 (2020)](https://doi.org/10.1103/PhysRevB.102.045109)

  The spin-orbit couplings are extracted from the tight-binding models generated by cif2qewan.py.
