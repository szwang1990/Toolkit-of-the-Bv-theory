# Toolkit-of-the-Bv-theory
Test cases of the non-breaking wave-induced mixing (the Bv theory) on ocean and climate models. Check the derivation of Bv from [Qiao et al., 2004, GRL](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2004GL019824).
## 1 the MASNUM wave model

## 2 The Bv effect in FESOM
FESOM is the first mature global ocean general circulation model based on unstructured‐mesh methods. The Bv effect on improving upper-ocean simulation has been tested using [FESOM 1.4](https://fesom.de/models/fesom14/). Follow along the steps to reproduce the results of [Wang et al., 2019, JAMES](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2018MS001494).
### 2.1 download FESOM1.4 code and inputdata

### 2.2
## 3 FIO-ESM v2.1, an Earth system model with MASNUM incorporated
### 3.1 download FIOESM v2.1 code and inputdata
### 3.2 porting
### 3.3 create new case
Go to the `scripts` directory, and use the create_newcase commond to create your own case.
For example, you can create a pre-industrial case via ```./create_newcase --case ../case/pi --compset B1850C5PM --res f19_g16 --mach your_machine_name```

### 3.4 porting and configuration
Use `user_nl_xxx` to control the behaviour of the corresponding component model.
For example, write the following two lines in user_nl_pop2 to control the output frequency of POP2 tavg file.
```
tavg_freq = 1 1 1
tavg_freq_opt = 'nmonth' 'nmonth' 'once'
```
control the behavior of the MASNUM wave model via the `mwave_nml` namelist in `pop2_in`

**Note**: For a smooth model test, we suggest you to edit the user_nl_cice like this:
```
ice_ic = /path/to/inputdata/ice/cice/cice.r.1951-01-01.nc
ocnbndy = 'default'
fbot_xfer_type = 'constant'
```
