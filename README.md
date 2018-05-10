# HPC Coursework

Base coursework for the Advanced High Performance Computing class.

* Source code is in the `d2q9-bgk.c` file
* Results checking scripts are in the `check/` directory

## Compiling and running

To compile type `make`. Editing the values for `CC` and `CFLAGS` in the Makefile can be used to enable different compiler options or use a different compiler. These can also be passed on the command line:

    $ make CFLAGS="-O3 -fopenmp -DDEBUG"

Input parameter and obstacle files are all specified on the command line of the `d2q9-bgk` executable.

Usage:

    $ ./d2q9-bgk <paramfile> <obstaclefile>
eg:

    $ ./d2q9-bgk input_256x256.params obstacles_256x256.dat

## Checking results

An automated result checking function is provided that requires you to load a particular Python module (`module load Python/2.7.12-foss-2016b`). Running `make check` will check the output file (average velocities and final state) against some reference results. By default, it should look something like this:

    $ make check
    python check/check.py --ref-av-vels-file=check/128x128.av_vels.dat --ref-final-state-file=check/128x128.final_state.dat --av-vels-file=./av_vels.dat --final-state-file=./final_state.dat
    Total difference in av_vels : 5.270812566515E-11
    Biggest difference (at step 1219) : 1.000241556248E-14
      1.595203170657E-02 vs. 1.595203170658E-02 = 6.3e-11%

    Total difference in final_state : 5.962977334129E-11
    Biggest difference (at coord (6,2)) : 1.000588500943E-14
      3.329122639178E-02 vs. 3.329122639179E-02 = 3e-11%

    Both tests passed!

This script takes both the reference results and the results to check (both average velocities and final state). This is also specified in the makefile and can be changed like the other options:

    $ make check REF_AV_VELS_FILE=check/128x256.av_vels.dat REF_FINAL_STATE_FILE=check/128x256.final_state.dat
    python check/check.py --ref-av-vels-file=check/128x256.av_vels.dat --ref-final-state-file=check/128x256.final_state.dat --av-vels-file=./av_vels.dat --final-state-file=./final_state.dat
    ...

All the options for this script can be examined by passing the --help flag to it.

    $ python check/check.py --help
    usage: check.py [-h] [--tolerance TOLERANCE] --ref-av-vels-file
                    REF_AV_VELS_FILE --ref-final-state-file REF_FINAL_STATE_FILE
    ...


## Running on BlueCrystal Phase 4

When you wish to submit a job to the queuing system on BlueCrystal, you should use the job submission script provided.

    $ sbatch job_submit_d2q9-bgk

This will dispatch a job to the queue, which you can monitor using the
`squeue` command:

    $ squeue -u $USER

When finished, the output from your job will be in a file called
`d2q9-bgk.out`:

    $ less d2q9-bgk.out

If you wish to run a different set of input parameters, you should
modify `job_submit_d2q9-bgk` to update the value assigned to `options`.

# Serial output for sample inputs
Running times were taken on a Phase 4 node.
- 128x128
```
./d2q9-bgk  input_128x128.params obstacles_128x128.dat
==done==
Reynolds number:		9.751927375793E+00
Elapsed time:			31.141698 (s)
Elapsed user CPU time:		31.144229 (s)
Elapsed system CPU time:	0.002000 (s)
```

- 128x256
```
./d2q9-bgk  input_128x256.params obstacles_128x256.dat
==done==
Reynolds number:		3.715003967285E+01
Elapsed time:			63.553739 (s)
Elapsed user CPU time:		63.558292 (s)
Elapsed system CPU time:	0.004000 (s)
```

- 256x256
```
./d2q9-bgk  input_256x256.params obstacles_256x256.dat
==done==
Reynolds number:		1.005141162872E+01
Elapsed time:			259.721668 (s)
Elapsed user CPU time:		259.744916 (s)
Elapsed system CPU time:	0.005000 (s)
```

- 1024x1024
```
./d2q9-bgk  input_1024x1024.params obstacles_1024x1024.dat
==done==
Reynolds number:		3.375851392746E+00
Elapsed time:			1072.777873 (s)
Elapsed user CPU time:		1072.855937 (s)
Elapsed system CPU time:	0.018001 (s)
```

# Visualisation

You can view the final state of the simulation by creating a .png image file using a provided Gnuplot script:

    $ gnuplot final_state.plt
