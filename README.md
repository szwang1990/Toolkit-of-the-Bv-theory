# Toolkit-of-the-Bv-theory
Test cases of the non-breaking wave-induced mixing (the Bv theory) on ocean and climate models. Check the derivation of Bv from [Qiao et al., 2004, GRL](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2004GL019824).
## 1 the MASNUM wave model

## 2 The Bv effect in FESOM
FESOM is the first mature global ocean general circulation model based on unstructured‐mesh methods. The Bv effect on improving upper-ocean simulation has been tested using [FESOM 1.4](https://fesom.de/models/fesom14/). Follow along the steps to reproduce the results of [Wang et al., 2019, JAMES](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2018MS001494).
### 2.1 download FESOM1.4 code and inputdata

### 2.2 porting and configuration
## 3 FIO-ESM v2.1, an Earth system model with MASNUM incorporated
### 3.1 download FIOESM v2.1 code and inputdata
### 3.2 porting
The control structure follows that of CESM1.2. So if you are familiar with CESM porting, configurating FIO-ESM will be much easier.
#### step 1. create `env_mach_specific.your_machine_name` file
There are many `env_mach_specific.machine_name` files in `fioesm2_1/scripts/ccsm_utils/Machines/`. You need to find one that resembles your machine the most, copy it and modify it. Basically you need to load modules related to compiler, mpi and netcdf. Also, some paths may need to be assigned.
#### step 2. modify `config_machines.xml`
```
fioesm2_1/scripts/ccsm_utils/Machines/config_machines.xml
```
This file tells you some basic information about your machine. For example, the operating system of the machine, the mpi lib that will be used, and the job scheduler of the machine. You need to find a `<machine>` tag that resembles your machine the most, copy it and modify the corresponding attributes.
`<DIN_LOC_ROOT>` is the absolute path of the inputdata directory you have downloaded.
`<BATCHSUBMIT>` is the commond for submitting jobs in your machine.

#### step 3. modify `config_compilers.xml` file
```
fioesm2_1/scripts/ccsm_utils/Machines/config_compilers.xml
```  
You need to find a `<compiler>` tag that resembles your machine the most, copy it and modify the corresponding attributes.
  
### 3.3 create new case
Go to the `scripts` directory, and use the create_newcase commond to create your own case.
For example, you can create a pre-industrial case via ```./create_newcase --case ../case/pi --compset B1850C5PM --res f19_g16 --mach your_machine_name```

### 3.4 model configuration
#### step 1. control the run length
`env_run.xml`
To control the model run length, use `xmlquery` to modify the `STOP_N` and `STOP_OPTION` variables in `env_run.xml` file. You may not want to conduct a too long run at first.
For example, the following two lines calls for a two-month long run.
```
./xmlchange STOP_N=2
./xmlchange STOP_OPTION=nmonths
```
#### step 2. contorl each component model
To control the behaviour of each component model, create `user_nl_xxx` files by callting `./cesm_setup`.
Use `user_nl_xxx` to control the behaviour of the corresponding component model.
For example, write the following two lines in `user_nl_pop2` to control the output frequency of POP2 tavg file.
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
### 3.5 build and run the model
Call `case_name.build` to build the model. For example, you have create a case named `pi`, then you need to call the `pi.build` file for building.
Call `case_name.submit` to submit the job to the job scheduling system. For example, you have create a case named `pi`, then you need to call the `pi.submit` file for job submitting.

### 3.6 check model results and the Bv effects
After the run is successfully finished, you may find the model results in `fioesm2_1/results/` directory.
Try to run a pair of experiments: one is the control run with Bv effect deprecated, the other is a sensitivity run with Bv incorporated. Check the difference between the two runs.
Ccontrol the behavior of the MASNUM wave model via the `mwave_nml` namelist in `user_nl_pop2`. To mute the Bv effect, `mwave_run` = .false.
