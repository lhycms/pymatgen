---
layout: default
title: pymatgen.electronic_structure.boltztrap.md
nav_exclude: true
---

# pymatgen.electronic_structure.boltztrap module

This module provides classes to run and analyze boltztrap on pymatgen band
structure objects. Boltztrap is a software interpolating band structures and
computing materials properties from this band structure using Boltzmann
semi-classical transport theory.

Boltztrap has been developed by Georg Madsen.

[http://www.icams.de/content/research/software-development/boltztrap/](http://www.icams.de/content/research/software-development/boltztrap/)

You need version 1.2.3 or higher

References are:

```default
Madsen, G. K. H., and Singh, D. J. (2006).
BoltzTraP. A code for calculating band-structure dependent quantities.
Computer Physics Communications, 175, 67-71
```


### _class_ pymatgen.electronic_structure.boltztrap.BoltztrapAnalyzer(gap=None, mu_steps=None, cond=None, seebeck=None, kappa=None, hall=None, doping=None, mu_doping=None, seebeck_doping=None, cond_doping=None, kappa_doping=None, hall_doping=None, intrans=None, dos=None, dos_partial=None, carrier_conc=None, vol=None, warning=None, bz_bands=None, bz_kpoints=None, fermi_surface_data=None)
Bases: `object`

Class used to store all the data from a boltztrap run.

Constructor taking directly all the data generated by Boltztrap. You
won’t probably use it directly but instead use the from_files and
from_dict methods.


* **Parameters**


    * **gap** – The gap after interpolation in eV


    * **mu_steps** – The steps of electron chemical potential (or Fermi
    level) in eV.


    * **cond** – The electronic conductivity tensor divided by a constant
    relaxation time (sigma/tau) at different temperature and
    fermi levels.
    The format is {temperature: [array of 3x3 tensors at each
    Fermi level in mu_steps]}. The units are 1/(Ohm\*m\*s).


    * **seebeck** – The Seebeck tensor at different temperatures and fermi
    levels. The format is {temperature: [array of 3x3 tensors at
    each Fermi level in mu_steps]}. The units are V/K


    * **kappa** – The electronic thermal conductivity tensor divided by a
    constant relaxation time (kappa/tau) at different temperature
    and fermi levels. The format is {temperature: [array of 3x3
    tensors at each Fermi level in mu_steps]}
    The units are W/(m\*K\*s)


    * **hall** – The hall tensor at different temperature and fermi levels
    The format is {temperature: [array of 27 coefficients list at
    each Fermi level in mu_steps]}
    The units are m^3/C


    * **doping** – The different doping levels that have been given to
    Boltztrap. The format is {‘p’:[],’n’:[]} with an array of
    doping levels. The units are cm^-3


    * **mu_doping** – Gives the electron chemical potential (or Fermi level)
    for a given set of doping.
    Format is {‘p’:{temperature: [fermi levels],’n’:{temperature:
    [fermi levels]}}
    the Fermi level array is ordered according to the doping
    levels in doping units for doping are in cm^-3 and for Fermi
    level in eV


    * **seebeck_doping** – The Seebeck tensor at different temperatures and
    doping levels. The format is {‘p’: {temperature: [Seebeck
    tensors]}, ‘n’:{temperature: [Seebeck tensors]}}
    The [Seebeck tensors] array is ordered according to the
    doping levels in doping units for doping are in cm^-3 and for
    Seebeck in V/K


    * **cond_doping** – The electronic conductivity tensor divided by a
    constant relaxation time (sigma/tau) at different
    temperatures and doping levels
    The format is {‘p’:{temperature: [conductivity tensors]},
    ‘n’:{temperature: [conductivity tensors]}}
    The [conductivity tensors] array is ordered according to the
    doping levels in doping units for doping are in cm^-3 and for
    conductivity in 1/(Ohm\*m\*s)


    * **kappa_doping** – The thermal conductivity tensor divided by a constant
    relaxation time (kappa/tau) at different temperatures and
    doping levels.
    The format is {‘p’:{temperature: [thermal conductivity
    tensors]},’n’:{temperature: [thermal conductivity tensors]}}
    The [thermal conductivity tensors] array is ordered according
    to the doping levels in doping units for doping are in cm^-3
    and for thermal conductivity in W/(m\*K\*s)


    * **hall_doping** – The Hall tensor at different temperatures and doping
    levels.
    The format is {‘p’:{temperature: [Hall tensors]},
    ‘n’:{temperature: [Hall tensors]}}
    The [Hall tensors] array is ordered according to the doping
    levels in doping and each Hall tensor is represented by a 27
    coefficients list.
    The units are m^3/C


    * **intrans** – a dictionary of inputs e.g. {“scissor”: 0.0}


    * **carrier_conc** – The concentration of carriers in electron (or hole)
    per unit cell


    * **dos** – The dos computed by Boltztrap given as a pymatgen Dos object


    * **dos_partial** – Data for the partial DOS projected on sites and
    orbitals


    * **vol** – Volume of the unit cell in angstrom cube (A^3)


    * **warning** – string if BoltzTraP outputted a warning, else None


    * **bz_bands** – Data for interpolated bands on a k-point line
    (run_type=BANDS)


    * **bz_kpoints** – k-point in reciprocal coordinates for interpolated
    bands (run_type=BANDS)


    * **fermi_surface_data** – energy values in a 3D grid imported from the
    output .cube file.



#### as_dict()

* **Returns**

    MSONable dict.



#### _static_ check_acc_bzt_bands(sbs_bz, sbs_ref, warn_thr=(0.03, 0.03))
Compare sbs_bz BandStructureSymmLine calculated with boltztrap with
the sbs_ref BandStructureSymmLine as reference (from MP for
instance), computing correlation and energy difference for eight bands
around the gap (semiconductors) or Fermi level (metals).
warn_thr is a threshold to get a warning in the accuracy of Boltztap
interpolated bands.
Return a dictionary with these keys:
- “N”: the index of the band compared; inside each there are:

>
> * “Corr”: correlation coefficient for the 8 compared bands


> * “Dist”: energy distance for the 8 compared bands


> * “branch_name”: energy distance for that branch


* “avg_corr”: average of correlation coefficient over the 8 bands


* “avg_dist”: average of energy distance over the 8 bands


* “nb_list”: list of indexes of the 8 compared bands


* “acc_thr”: list of two float corresponding to the two warning

    thresholds in input


* “acc_err”: list of two bools:

    True if the avg_corr > warn_thr[0], and
    True if the avg_dist > warn_thr[1]

See also compare_sym_bands function doc.


#### _static_ from_dict(data)

* **Parameters**

    **data** – Dict representation.



* **Returns**

    BoltztrapAnalyzer



#### _static_ from_files(path_dir, dos_spin=1)
get a BoltztrapAnalyzer object from a set of files.


* **Parameters**


    * **path_dir** – directory where the boltztrap files are


    * **dos_spin** – in DOS mode, set to 1 for spin up and -1 for spin down



* **Returns**

    a BoltztrapAnalyzer object



#### get_average_eff_mass(output='eigs', doping_levels=True)
Gives the average effective mass tensor. We call it average because
it takes into account all the bands
and regions in the Brillouin zone. This is different than the standard
textbook effective mass which relates
often to only one (parabolic) band.
The average effective mass tensor is defined as the integrated
average of the second derivative of E(k)
This effective mass tensor takes into account:
-non-parabolicity
-multiple extrema
-multiple bands.

For more information about it. See:

Hautier, G., Miglio, A., Waroquiers, D., Rignanese, G., & Gonze,
X. (2014).
How Does Chemistry Influence Electron Effective Mass in Oxides?
A High-Throughput Computational Analysis. Chemistry of Materials,
26(19), 5447-5458. doi:10.1021/cm404079a

or

Hautier, G., Miglio, A., Ceder, G., Rignanese, G.-M., & Gonze,
X. (2013).
Identification and design principles of low hole effective mass
p-type transparent conducting oxides.
Nature Communications, 4, 2292. doi:10.1038/ncomms3292

Depending on the value of output, we have either the full 3x3
effective mass tensor,
its 3 eigenvalues or an average


* **Parameters**


    * **output** (*str*) – ‘eigs’ for eigenvalues, ‘tensor’ for the full


    * **average** (*tensor and 'average' for an*) –


    * **doping_levels** (*bool*) – True for the results to be given at


    * **levels** (*different doping*) –


    * **results** (*False for*) –


    * **potentials** (*at different electron chemical*) –



* **Returns**

    {temp:[]},’n’:{temp:[]}}
    with an array of effective mass tensor, eigenvalues of average
    value (depending on output) for each temperature and for each
    doping level.
    The ‘p’ links to hole effective mass tensor and ‘n’ to electron
    effective mass tensor.



* **Return type**

    If doping_levels=True,a dictionary {‘p’



#### get_carrier_concentration()
gives the carrier concentration (in cm^-3).


* **Returns**

    []} with an array of carrier concentration
    (in cm^-3) at each temperature
    The array relates to each step of electron chemical potential



* **Return type**

    a dictionary {temp



#### get_complete_dos(structure: [Structure](pymatgen.core.structure.md#pymatgen.core.structure.Structure), analyzer_for_second_spin=None)
Gives a CompleteDos object with the DOS from the interpolated projected band structure.


* **Parameters**


    * **structure** – necessary to identify sites for projection


    * **analyzer_for_second_spin** – must be specified to have a CompleteDos with both Spin components



* **Returns**

    a CompleteDos object


Example of use in case of spin polarized case:

> BoltztrapRunner(bs=bs,nelec=10,run_type=”DOS”,spin=1).run(path_dir=’dos_up/’)
> an_up=BoltztrapAnalyzer.from_files(“dos_up/boltztrap/”,dos_spin=1)

> BoltztrapRunner(bs=bs,nelec=10,run_type=”DOS”,spin=-1).run(path_dir=’dos_dw/’)
> an_dw=BoltztrapAnalyzer.from_files(“dos_dw/boltztrap/”,dos_spin=-1)

> cdos=an_up.get_complete_dos(bs.structure,an_dw)


#### get_complexity_factor(output='average', temp=300, doping_levels=False, Lambda=0.5)
Fermi surface complexity factor respect to calculated as explained in Ref.
Gibbs, Z. M. et al., Effective mass and fermi surface complexity factor
from ab initio band structure calculations.
npj Computational Materials 3, 8 (2017).


* **Parameters**


    * **output** – ‘average’ returns the complexity factor calculated using the average
    of the three diagonal components of the seebeck and conductivity tensors.
    ‘tensor’ returns the complexity factor respect to the three
    diagonal components of seebeck and conductivity tensors.


    * **doping_levels** – False means that the complexity factor is calculated
    for every value of the chemical potential
    True means that the complexity factor is calculated
    for every value of the doping levels for both n and p types


    * **temp** – temperature of calculated seebeck and conductivity.


    * **Lambda** – fitting parameter used to model the scattering (0.5 means constant
    relaxation time).



* **Returns**

    a list of values for the complexity factor w.r.t the chemical potential,
    if doping_levels is set at False;
    a dict with n an p keys that contain a list of values for the complexity factor
    w.r.t the doping levels, if doping_levels is set at True;
    if ‘tensor’ is selected, each element of the lists is a list containing
    the three components of the complexity factor.



#### get_conductivity(output='eigs', doping_levels=True, relaxation_time=1e-14)
Gives the conductivity (1/Ohm\*m) in either a full 3x3 tensor
form, as 3 eigenvalues, or as the average value
(trace/3.0) If doping_levels=True, the results are given at
different p and n doping
levels (given by self.doping), otherwise it is given as a series
of electron chemical potential values.


* **Parameters**


    * **output** (*str*) – the type of output. ‘tensor’ give the full


    * **tensor** (*3x3*) –


    * **and** (*'eigs' its 3 eigenvalues*) –


    * **eigenvalues** (*'average' the average** of **the three*) –


    * **doping_levels** (*bool*) – True for the results to be given at


    * **levels** (*different doping*) –


    * **results** (*False for*) –


    * **potentials** (*at different electron chemical*) –


    * **relaxation_time** (*float*) – constant relaxation time in secs



* **Returns**

    {‘p’:[],’n’:[]}}.
    The ‘p’ links to conductivity
    at p-type doping and ‘n’ to the conductivity at n-type
    doping. Otherwise,
    returns a {temp:[]} dictionary. The result contains either
    the sorted three eigenvalues of the symmetric
    conductivity tensor (format=’eigs’) or a full tensor (3x3
    array) (output=’tensor’) or as an average
    (output=’average’).
    The result includes a given constant relaxation time

    units are 1/Ohm\*m




* **Return type**

    If doping_levels=True, a dictionary {temp



#### get_extreme(target_prop, maximize=True, min_temp=None, max_temp=None, min_doping=None, max_doping=None, isotropy_tolerance=0.05, use_average=True)
This method takes in eigenvalues over a range of carriers,
temperatures, and doping levels, and tells you what is the “best”
value that can be achieved for the given target_property. Note that
this method searches the doping dict only, not the full mu dict.


* **Parameters**


    * **target_prop** – target property, i.e. “seebeck”, “power factor”,
    “conductivity”, “kappa”, or “zt”


    * **maximize** – True to maximize, False to minimize (e.g. kappa)


    * **min_temp** – minimum temperature allowed


    * **max_temp** – maximum temperature allowed


    * **min_doping** – minimum doping allowed (e.g., 1E18)


    * **max_doping** – maximum doping allowed (e.g., 1E20)


    * **isotropy_tolerance** – tolerance for isotropic (0.05 = 5%)


    * **use_average** – True for avg of eigenval, False for max eigenval



* **Returns**

    {“value”, “temperature”, “doping”, “isotropic”}



* **Return type**

    A dictionary with keys {“p”, “n”, “best”} with sub-keys



#### get_hall_carrier_concentration()
gives the Hall carrier concentration (in cm^-3). This is the trace of
the Hall tensor (see Boltztrap source code) Hall carrier concentration
are not always exactly the same than carrier concentration.


* **Returns**

    []} with an array of Hall carrier concentration
    (in cm^-3) at each temperature The array relates to each step of
    electron chemical potential



* **Return type**

    a dictionary {temp



#### get_mu_bounds(temp=300)

* **Parameters**

    **temp** – Temperature.



* **Returns**

    The chemical potential bounds at that temperature.



#### get_power_factor(output='eigs', doping_levels=True, relaxation_time=1e-14)
Gives the power factor (Seebeck^2 \* conductivity) in units
microW/(m\*K^2) in either a full 3x3 tensor form,
as 3 eigenvalues, or as the average value (trace/3.0) If
doping_levels=True, the results are given at
different p and n doping levels (given by self.doping), otherwise it
is given as a series of
electron chemical potential values.


* **Parameters**


    * **output** (*str*) – the type of output. ‘tensor’ give the full 3x3


    * **tensor** –


    * **and** (*'eigs' its 3 eigenvalues*) –


    * **eigenvalues** (*'average' the average** of **the three*) –


    * **doping_levels** (*bool*) – True for the results to be given at


    * **levels** (*different doping*) –


    * **results** (*False for*) –


    * **potentials** (*at different electron chemical*) –


    * **relaxation_time** (*float*) – constant relaxation time in secs



* **Returns**

    {‘p’:[],’n’:[]}}. The
    ‘p’ links to power factor
    at p-type doping and ‘n’ to the conductivity at n-type doping.
    Otherwise,
    returns a {temp:[]} dictionary. The result contains either the
    sorted three eigenvalues of the symmetric
    power factor tensor (format=’eigs’) or a full tensor (3x3 array) (
    output=’tensor’) or as an average
    (output=’average’).
    The result includes a given constant relaxation time

    units are microW/(m K^2)




* **Return type**

    If doping_levels=True, a dictionary {temp



#### get_seebeck(output='eigs', doping_levels=True)
Gives the seebeck coefficient (microV/K) in either a
full 3x3 tensor form, as 3 eigenvalues, or as the average value
(trace/3.0) If doping_levels=True, the results are given at
different p and n doping
levels (given by self.doping), otherwise it is given as a series
of electron chemical potential values.


* **Parameters**


    * **output** (*str*) – the type of output. ‘tensor’ give the full


    * **tensor** (*3x3*) –


    * **and** (*'eigs' its 3 eigenvalues*) –


    * **eigenvalues** (*'average' the average** of **the three*) –


    * **doping_levels** (*bool*) – True for the results to be given at


    * **levels** (*different doping*) –


    * **results** (*False for*) –


    * **potentials** (*at different electron chemical*) –



* **Returns**

    {‘p’:[],’n’:[]}}.
    The ‘p’ links to Seebeck at p-type doping
    and ‘n’ to the Seebeck at n-type doping. Otherwise, returns a
    {temp:[]} dictionary
    The result contains either the sorted three eigenvalues of
    the symmetric
    Seebeck tensor (output=’eigs’) or a full tensor (3x3 array) (
    output=’tensor’) or as an average
    (output=’average’).

    units are microV/K




* **Return type**

    If doping_levels=True, a dictionary {temp



#### get_seebeck_eff_mass(output='average', temp=300, doping_levels=False, Lambda=0.5)
Seebeck effective mass calculated as explained in Ref.
Gibbs, Z. M. et al., Effective mass and fermi surface complexity factor
from ab initio band structure calculations.
npj Computational Materials 3, 8 (2017).


* **Parameters**


    * **output** – ‘average’ returns the seebeck effective mass calculated using
    the average of the three diagonal components of the seebeck tensor.
    ‘tensor’ returns the seebeck effective mass respect to the three
    diagonal components of the seebeck tensor.


    * **doping_levels** – False means that the seebeck effective mass is calculated
    for every value of the chemical potential
    True means that the seebeck effective mass is calculated
    for every value of the doping levels for both n and p types


    * **temp** – temperature of calculated seebeck.


    * **Lambda** – fitting parameter used to model the scattering (0.5 means constant
    relaxation time).



* **Returns**

    a list of values for the seebeck effective mass w.r.t the chemical potential,
    if doping_levels is set at False;
    a dict with n an p keys that contain a list of values for the seebeck effective
    mass w.r.t the doping levels, if doping_levels is set at True;
    if ‘tensor’ is selected, each element of the lists is a list containing
    the three components of the seebeck effective mass.



#### get_symm_bands(structure: [Structure](pymatgen.core.structure.md#pymatgen.core.structure.Structure), efermi, kpt_line=None, labels_dict=None)
Function useful to read bands from Boltztrap output and get a
BandStructureSymmLine object comparable with that one from a DFT
calculation (if the same kpt_line is provided). Default kpt_line
and labels_dict is the standard path of high symmetry k-point for
the specified structure. They could be extracted from the
BandStructureSymmLine object that you want to compare with. efermi
variable must be specified to create the BandStructureSymmLine
object (usually it comes from DFT or Boltztrap calc).


#### get_thermal_conductivity(output='eigs', doping_levels=True, k_el=True, relaxation_time=1e-14)
Gives the electronic part of the thermal conductivity in either a
full 3x3 tensor form,
as 3 eigenvalues, or as the average value (trace/3.0) If
doping_levels=True, the results are given at
different p and n doping levels (given by self.doping), otherwise it
is given as a series of
electron chemical potential values.


* **Parameters**


    * **output** (*str*) – the type of output. ‘tensor’ give the full 3x3


    * **tensor** –


    * **and** (*'eigs' its 3 eigenvalues*) –


    * **eigenvalues** (*'average' the average** of **the three*) –


    * **doping_levels** (*bool*) – True for the results to be given at


    * **levels** (*different doping*) –


    * **results** (*False for*) –


    * **potentials** (*at different electron chemical*) –


    * **k_el** (*bool*) – True for k_0-PF\*T, False for k_0


    * **relaxation_time** (*float*) – constant relaxation time in secs



* **Returns**

    {‘p’:[],’n’:[]}}. The
    ‘p’ links to thermal conductivity
    at p-type doping and ‘n’ to the thermal conductivity at n-type
    doping. Otherwise,
    returns a {temp:[]} dictionary. The result contains either the
    sorted three eigenvalues of the symmetric
    conductivity tensor (format=’eigs’) or a full tensor (3x3 array) (
    output=’tensor’) or as an average
    (output=’average’).
    The result includes a given constant relaxation time

    units are W/mK




* **Return type**

    If doping_levels=True, a dictionary {temp



#### get_zt(output='eigs', doping_levels=True, relaxation_time=1e-14, k_l=1)
Gives the ZT coefficient (S^2\*cond\*T/thermal cond) in either a full
3x3 tensor form,
as 3 eigenvalues, or as the average value (trace/3.0) If
doping_levels=True, the results are given at
different p and n doping levels (given by self.doping), otherwise it
is given as a series of
electron chemical potential values. We assume a constant relaxation
time and a constant
lattice thermal conductivity.


* **Parameters**


    * **output** (*str*) – the type of output. ‘tensor’ give the full 3x3


    * **tensor** –


    * **and** (*'eigs' its 3 eigenvalues*) –


    * **eigenvalues** (*'average' the average** of **the three*) –


    * **doping_levels** (*bool*) – True for the results to be given at


    * **levels** (*different doping*) –


    * **results** (*False for*) –


    * **potentials** (*at different electron chemical*) –


    * **relaxation_time** (*float*) – constant relaxation time in secs


    * **k_l** (*float*) – lattice thermal cond in W/(m\*K)



* **Returns**

    {‘p’:[],’n’:[]}}. The
    ‘p’ links to ZT
    at p-type doping and ‘n’ to the ZT at n-type doping. Otherwise,
    returns a {temp:[]} dictionary. The result contains either the
    sorted three eigenvalues of the symmetric
    ZT tensor (format=’eigs’) or a full tensor (3x3 array) (
    output=’tensor’) or as an average
    (output=’average’).
    The result includes a given constant relaxation time and lattice
    thermal conductivity



* **Return type**

    If doping_levels=True, a dictionary {temp



#### _static_ parse_cond_and_hall(path_dir, doping_levels=None)
Parses the conductivity and Hall tensors
:param path_dir: Path containing .condtens / .halltens files
:param doping_levels: ([float]) - doping lvls, parse outtrans to get this.


* **Returns**

    mu_steps, cond, seebeck, kappa, hall, pn_doping_levels,
    mu_doping, seebeck_doping, cond_doping, kappa_doping,
    hall_doping, carrier_conc



#### _static_ parse_intrans(path_dir)
Parses boltztrap.intrans mainly to extract the value of scissor applied
to the bands or some other inputs.


* **Parameters**

    **path_dir** – (str) dir containing the boltztrap.intrans file



* **Returns**

    a dictionary containing various inputs that had

        been used in the Boltztrap run.




* **Return type**

    intrans (dict)



#### _static_ parse_outputtrans(path_dir)
Parses .outputtrans file.


* **Parameters**

    **path_dir** – dir containing boltztrap.outputtrans



* **Returns**

    tuple - (run_type, warning, efermi, gap, doping_levels)



#### _static_ parse_struct(path_dir)
Parses boltztrap.struct file (only the volume).


* **Parameters**

    **path_dir** – (str) dir containing the boltztrap.struct file



* **Returns**

    (float) volume



#### _static_ parse_transdos(path_dir, efermi, dos_spin=1, trim_dos=False)
Parses .transdos (total DOS) and .transdos_x_y (partial DOS) files
:param path_dir: (str) dir containing DOS files
:param efermi: (float) Fermi energy
:param dos_spin: (int) -1 for spin down, +1 for spin up
:param trim_dos: (bool) whether to post-process / trim DOS.


* **Returns**

    tuple - (DOS, dict of partial DOS)



### _exception_ pymatgen.electronic_structure.boltztrap.BoltztrapError()
Bases: `Exception`

Exception class for boltztrap.
Raised when the boltztrap gives an error.


### _class_ pymatgen.electronic_structure.boltztrap.BoltztrapRunner(bs, nelec, dos_type='HISTO', energy_grid=0.005, lpfac=10, run_type='BOLTZ', band_nb=None, tauref=0, tauexp=0, tauen=0, soc=False, doping=None, energy_span_around_fermi=1.5, scissor=0.0, kpt_line=None, spin=None, cond_band=False, tmax=1300, tgrid=50, symprec=0.001, cb_cut=10, timeout=7200)
Bases: `MSONable`

This class is used to run Boltztrap on a band structure object.


* **Parameters**


    * **bs** – A band structure object


    * **nelec** – the number of electrons


    * **dos_type** – two options for the band structure integration: “HISTO”
    (histogram) or “TETRA” using the tetrahedon method. TETRA
    typically gives better results (especially for DOSes)
    but takes more time


    * **energy_grid** – the energy steps used for the integration (eV)


    * **lpfac** – the number of interpolation points in the real space. By
    default 10 gives 10 time more points in the real space than
    the number of kpoints given in reciprocal space


    * **run_type** – type of boltztrap usage. by default
    - BOLTZ: (default) compute transport coefficients
    - BANDS: interpolate all bands contained in the energy range
    specified in energy_span_around_fermi variable, along specified
    k-points
    - DOS: compute total and partial dos (custom BoltzTraP code
    needed!)
    - FERMI: compute fermi surface or more correctly to
    get certain bands interpolated


    * **band_nb** – indicates a band number. Used for Fermi Surface interpolation
    (run_type=”FERMI”)


    * **spin** – specific spin component (1: up, -1: down) of the band selected
    in FERMI mode (mandatory).


    * **cond_band** – if a conduction band is specified in FERMI mode,
    set this variable as True


    * **tauref** – reference relaxation time. Only set to a value different than
    zero if we want to model beyond the constant relaxation time.


    * **tauexp** – exponent for the energy in the non-constant relaxation time
    approach


    * **tauen** – reference energy for the non-constant relaxation time approach


    * **soc** – results from spin-orbit coupling (soc) computations give
    typically non-polarized (no spin up or down) results but single
    electron occupations. If the band structure comes from a soc
    computation, you should set soc to True (default False)


    * **doping** – the fixed doping levels you want to compute. Boltztrap provides
    both transport values depending on electron chemical potential
    (fermi energy) and for a series of fixed carrier
    concentrations. By default, this is set to 1e16 to 1e22 in
    increments of factors of 10.


    * **energy_span_around_fermi** – usually the interpolation is not needed on the entire energy
    range but on a specific range around the Fermi level.
    This energy gives this range in eV. by default it is 1.5 eV.
    If DOS or BANDS type are selected, this range is automatically
    set to cover the entire energy range.


    * **scissor** – scissor to apply to the band gap (eV). This applies a scissor
    operation moving the band edges without changing the band
    shape. This is useful to correct the often underestimated band
    gap in DFT. Default is 0.0 (no scissor)


    * **kpt_line** – list of fractional coordinates of kpoints as arrays or list of
    Kpoint objects for BANDS mode calculation (standard path of
    high symmetry k-points is automatically set as default)


    * **tmax** – Maximum temperature (K) for calculation (default=1300)


    * **tgrid** – Temperature interval for calculation (default=50)


    * **symprec** – 1e-3 is the default in pymatgen. If the kmesh has been
    generated using a different symprec, it has to be specified
    to avoid a “factorization error” in BoltzTraP calculation.
    If a kmesh that spans the whole Brillouin zone has been used,
    or to disable all the symmetries, set symprec to None.


    * **cb_cut** – by default 10% of the highest conduction bands are
    removed because they are often not accurate.
    Tune cb_cut to change the percentage (0-100) of bands
    that are removed.


    * **timeout** – overall time limit (in seconds): mainly to avoid infinite
    loop when trying to find Fermi levels.



#### as_dict()

* **Returns**

    MSONable dict



#### _property_ bs()
The BandStructure


* **Type**

    return



#### _property_ nelec()
Number of electrons


* **Type**

    return



#### run(path_dir=None, convergence=True, write_input=True, clear_dir=False, max_lpfac=150, min_egrid=5e-05)
Write inputs (optional), run BoltzTraP, and ensure
convergence (optional).


* **Parameters**


    * **path_dir** (*str*) – directory in which to run BoltzTraP


    * **convergence** (*bool*) – whether to check convergence and make
    corrections if needed


    * **write_input** – (bool) whether to write input files before the run
    (required for convergence mode)


    * **clear_dir** – (bool) whether to remove all files in the path_dir
    before starting


    * **max_lpfac** – (float) maximum lpfac value to try before reducing egrid
    in convergence mode


    * **min_egrid** – (float) minimum egrid value to try before giving up in
    convergence mode



#### write_def(output_file)
Writes the def to an output file.


* **Parameters**

    **output_file** – Filename



#### write_energy(output_file)
Writes the energy to an output file.


* **Parameters**

    **output_file** – Filename



#### write_input(output_dir)
Writes the input files.


* **Parameters**

    **output_dir** – Directory to write the input files.



#### write_intrans(output_file)
Writes the intrans to an output file.


* **Parameters**

    **output_file** – Filename



#### write_proj(output_file_proj, output_file_def)
Writes the projections to an output file.


* **Parameters**

    **output_file** – Filename



#### write_struct(output_file)
Writes the structure to an output file.


* **Parameters**

    **output_file** – Filename



### pymatgen.electronic_structure.boltztrap.compare_sym_bands(bands_obj, bands_ref_obj, nb=None)
Compute the mean of correlation between bzt and vasp bandstructure on
sym line, for all bands and locally (for each branches) the difference
squared (%) if nb is specified.


### pymatgen.electronic_structure.boltztrap.eta_from_seebeck(seeb, Lambda)
It takes a value of seebeck and adjusts the analytic seebeck until it’s equal
Returns: eta where the two seebeck coefficients are equal
(reduced chemical potential).


### pymatgen.electronic_structure.boltztrap.read_cube_file(filename)

* **Parameters**

    **filename** – Cube filename



* **Returns**

    Energy data.



### pymatgen.electronic_structure.boltztrap.seebeck_eff_mass_from_carr(eta, n, T, Lambda)
Calculate seebeck effective mass at a certain carrier concentration
eta in kB\*T units, n in cm-3, T in K, returns mass in m0 units.


### pymatgen.electronic_structure.boltztrap.seebeck_eff_mass_from_seebeck_carr(seeb, n, T, Lambda)
Find the chemical potential where analytic and calculated seebeck are identical
and then calculate the seebeck effective mass at that chemical potential and
a certain carrier concentration n.


### pymatgen.electronic_structure.boltztrap.seebeck_spb(eta, Lambda=0.5)
Seebeck analytic formula in the single parabolic model.