This is a Hodgkin Huxley (HH) simplified compartamental model of a neuron assignment

Author : Sequential code by Dmitri Yudanov (update: Dan Brandt), MPI implemntation by Sharan Vidash Vidya Shanmugham  

New Step Before Compiling:
 
 There is a new step that is required before compiling, but the system tells you what 
 that step is if  you don't do it beforehand. 
 The command to run before running make is as follows:
 
 spack load gcc@7.4.0%gcc@7.4.0
 
Adding Access to gnuplot:
 
 One of the programs you'll be indirectly using is 'gnuplot' which is an open source graphing software.
 The system doesn't know where to find this by default, even though it's already installed. To fix this, 
 simply run this command:
 
 echo 'PATH+=":/tools/gnuplot/5.0.5/bin"' >> ~/.bashrc
 
 After running that command, your user environment will know where to find gnuplot on the next login, or by 
 running the following command:
 
 source ~/.bashrc
 

COMPILING

  To compile the sequential code, run:
    $ make seq_hh
    
  The Makefile has a rule in place to compile the MPI code. You will have to load the mpi api before compiling.
   Loading the mpi api

    $ load module openmpi

   To compile the mpi code

    $ make mpi_hh

  Do not run the simulation on the head node; you must submit the job using SLURM. No one 
  likes having to work on a node pegged at 100% cpu; it can potentially cause 
  problems. A crashed compute node is more preferable to a crashed or unresponsive
  head node. Recalcitrant offenders may be penalized. Submitting jobs
  on the head node is fine. 
  
  The head node is the server you get when you SSH into cluster.ce.rit.edu.
  
RUNNING BATCH JOBS ON CLUSTER ,also MPI running steps 

  Jobs should be scheduled to be run on the cluster using SLURM. 
  A sample script have been included to help you get started. When using the MPI
  script, make sure that you modify the -n option that is passed to sbatch, in
  addition to the one passed to -np for mpirun.
  
  To schedule a job to be run mpi code:

    $ sbatch runner_mpi.sh

  To schedule a job to be run sequential code:

    $ sbatch runner_seq.sh
    
  The number given is the job number. You can use this to identify the job, or
  to delete it (see below - To delete a specific job). Results of the submitted 
  batch jobs can be found in the corrosponding mpi_hh.out or seq_hh.out file.
  This can be changed; see the 
  
  To view running jobs:
    $ squeue
    
  To view status of all nodes:
    $ sinfo

  To delete a specific job:
    $ scancel <job id>

  To kill all jobs submitted by you:
    $ scancel -u <username>
    
  To kill processes run without scancel:
    $ orte-clean
    
  Jobs that are kinda floating about or aren't doing anything useful should be 
  removed from the queue.
  
SAVED DATA
    
  Simulation data is saved in a file with a name following the format:

    data/pWWdXXcYY_MMDDYY_HHMMSS.dat

  where 'WW' is the number of processes used, 'XX' is the number of dendrites,
  'YY' the number of compartments, and 'MMDDYY_...' the time at which the
  simulation was run.

  Simulation data is also graphed into a PNG file saved under a similar name,
  but inside the graphs/ directory.

  If the data/ or graphs/ directories do not exist, they will be created.

TOGGLING PLOTTING OF SIMULATION DATA TO SCREEN/PNG

  The graphing of simulation data can be toggled with two preprocessor flags. To
  disable plotting entirely, remove the 'PLOT_PNG' and 'PLOT_SCREEN' definitions
  from the Makefile.

  If you want to plot to the screen, make sure that 'PLOT_SCREEN' is defined. To
  plot to a PNG file, make sure that PLOT_PNG is defined.

