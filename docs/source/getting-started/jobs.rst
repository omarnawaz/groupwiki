Job Submission with SLURM
========================================

SLURM (Simple Linux Utility for Resource Management) is the job scheduler on Falcon. It manages the queue of jobs waiting to run on the supercomputer and allocates computing resources fairly among users. This guide covers the basics of submitting and monitoring jobs.

Interactive vs. Batch Jobs
------------------------------------

Before diving into commands, understand the two ways to use Falcon:

**Interactive Jobs**
You run commands directly and see output immediately. Good for:
- Testing code before running full simulations
- Debugging issues
- Quick exploratory work
- Interactive Python/MATLAB sessions

**Batch Jobs**
You write a script with commands and submit it to the queue. The system runs it when resources are available. Good for:
- Long-running simulations
- Jobs that take hours or days
- Running many jobs in sequence
- Unattended computing

Most serious work on Falcon uses batch jobs because interactive sessions have time limits and you need to stay connected.

Interactive Jobs with srun
------------------------------------

**srun** — Run a Command Interactively

Run a command directly on compute resources. Useful for testing and short tasks.

.. code-block:: bash

   srun hostname                          # Get the name of the compute node
   srun -N 1 -n 1 python script.py        # Run Python script on 1 node, 1 task
   srun -N 1 -n 4 python script.py        # Run on 1 node with 4 tasks

Common flags:

.. code-block:: bash

   -N NUM                                 # Number of nodes (--nodes=NUM)
   -n NUM                                 # Number of tasks (--ntasks=NUM)
   -c NUM                                 # Cores per task (--cpus-per-task=NUM)
   -t TIME                                # Time limit (--time=HH:MM:SS)
   --mem=SIZE                             # Memory per node (e.g., 4G, 16G)
   --partition=NAME                       # Which partition/queue (see sinfo)
   --job-name=NAME                        # Name for the job

Example with multiple flags:

.. code-block:: bash

   srun -N 1 -n 1 -c 4 -t 00:30:00 --mem=8G python analysis.py

This requests 1 node, 1 task, 4 cores per task, 30 minutes, and 8GB of memory.

**Important**: Interactive jobs have time limits (usually 1-4 hours depending on the partition). For longer jobs, use batch submission.

Batch Jobs with sbatch
------------------------------------

**sbatch** — Submit a Job Script

For longer or more complex jobs, write a script and submit it to the queue.

**Basic Script Structure**

Create a file called ``myjob.sh``:

.. code-block:: bash

   #!/bin/bash
   #SBATCH --job-name=myanalysis
   #SBATCH --nodes=1
   #SBATCH --ntasks=1
   #SBATCH --cpus-per-task=4
   #SBATCH --time=02:00:00
   #SBATCH --mem=16G
   #SBATCH --output=job_%j.out
   #SBATCH --error=job_%j.err

   # Load any modules you need
   module load python/3.9

   # Your actual commands
   python analysis.py
   echo "Job finished"

Lines starting with ``#SBATCH`` are directives that tell SLURM how to run your job. They must come before any actual commands.

Submit it:

.. code-block:: bash

   sbatch myjob.sh

SLURM will print out the job ID. You can use this to monitor or cancel the job.

**Common SBATCH Directives**

.. code-block:: bash

   #SBATCH --job-name=NAME              # Name for your job
   #SBATCH --nodes=N                    # Number of compute nodes
   #SBATCH --ntasks=N                   # Total number of tasks
   #SBATCH --cpus-per-task=N            # Cores per task (for OpenMP)
   #SBATCH --time=HH:MM:SS              # Time limit
   #SBATCH --mem=SIZE                   # Memory per node (e.g., 16G)
   #SBATCH --mem-per-cpu=SIZE           # Memory per core instead
   #SBATCH --output=file.out            # Where to save stdout
   #SBATCH --error=file.err             # Where to save stderr
   #SBATCH --partition=NAME             # Queue/partition name
   #SBATCH --account=PROJECT            # Project/account to charge
   #SBATCH --mail-type=END,FAIL         # Email when done or failed
   #SBATCH --mail-user=email@domain     # Where to send email
   #SBATCH --dependency=afterok:123     # Run after job 123 completes

**Understanding Job IDs**

In the output file names, ``%j`` gets replaced with the job ID. For example:

.. code-block:: bash

   #SBATCH --output=job_%j.out

If your job ID is 12345, the output file will be ``job_12345.out``.

**Example: Running Python with NumPy**

.. code-block:: bash

   #!/bin/bash
   #SBATCH --job-name=numpy_analysis
   #SBATCH --nodes=1
   #SBATCH --ntasks=1
   #SBATCH --cpus-per-task=8
   #SBATCH --time=04:00:00
   #SBATCH --mem=32G
   #SBATCH --output=results_%j.out
   #SBATCH --error=errors_%j.err

   # Load Python module
   module load python/3.9

   # Run your script
   python -u analysis.py > output_log.txt

The ``-u`` flag makes Python print output immediately instead of buffering it.

Monitoring Jobs
------------------------------------

**squeue** — Show Job Queue

See what jobs are running and queued.

.. code-block:: bash

   squeue                                 # Show all your jobs
   squeue -u $USER                       # Show jobs for current user
   squeue -A PROJECT                     # Show jobs in a project
   squeue -p NAME                        # Show jobs in a partition
   squeue -j 12345                       # Show specific job details
   squeue -l                             # Long format (more details)
   squeue -S StartTime                   # Sort by start time
   squeue --states=PENDING               # Show only pending jobs
   squeue --states=RUNNING               # Show only running jobs

Output columns:
- **JOBID**: Job identifier (use this to cancel or modify)
- **PARTITION**: Queue name
- **NAME**: Job name
- **USER**: Who submitted it
- **ST**: Status (R=Running, PD=Pending, CA=Cancelled, etc.)
- **TIME**: How long it's been running
- **NODES**: Number of nodes
- **NODELIST**: Which nodes it's running on

Example output:

.. code-block:: text

   JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
   12345 cpu           myanalysis omar    R       2:30:15      1 node[01]
   12346 cpu           nextjob  omar    PD       0:00      1 (None)

The job 12345 is running and has been for 2 hours 30 minutes. Job 12346 is pending (waiting for resources).

**sinfo** — Show Partition Information

See available partitions and resources.

.. code-block:: bash

   sinfo                                  # Show all partitions
   sinfo -p PARTITION_NAME               # Show specific partition
   sinfo -N                              # Show per-node information
   sinfo -l                              # Long format

Output shows:
- **PARTITION**: Queue name and if it's the default (*)
- **AVAIL**: If partition is available (up/down)
- **TIMELIMIT**: Maximum job time allowed
- **NODES**: Number of nodes and their states
- States: idle, allocated, down, etc.

**sacct** — Show Accounting Information

View details about completed jobs.

.. code-block:: bash

   sacct                                  # Show your recent jobs
   sacct -j 12345                        # Details for job 12345
   sacct --starttime=2024-01-15          # Jobs since a date
   sacct --format=JobID,JobName,Elapsed,MaxRSS,State

**sstat** — Show Currently Running Job Stats

Get real-time statistics on a running job (useful for optimization).

.. code-block:: bash

   sstat -j 12345                        # Stats for job 12345
   sstat -j 12345 --format=AveCPU,AveVMSize,MaxRSS

Canceling and Modifying Jobs
------------------------------------

**scancel** — Cancel a Job

.. code-block:: bash

   scancel 12345                         # Cancel job 12345
   scancel -u $USER                      # Cancel all your jobs (be careful!)
   scancel --state=PENDING               # Cancel all pending jobs

**scontrol** — Control Jobs

View and modify job properties.

.. code-block:: bash

   scontrol show job 12345               # Show full job details
   scontrol update JobId=12345 TimeLimit=04:00:00  # Change time limit

Job States
------------------------------------

When you check your job status, you'll see one of these states:

- **RUNNING (R)**: Job is currently executing
- **PENDING (PD)**: Job is waiting for resources to become available
- **COMPLETED (CD)**: Job finished successfully
- **FAILED (F)**: Job exited with error
- **CANCELLED (CA)**: You cancelled it with ``scancel``
- **TIMEOUT**: Job hit its time limit and was killed
- **OUT_OF_MEMORY**: Job used more memory than allocated

The reason column shows why a job is pending:
- ``(None)`` — Waiting for resources to free up
- ``(Resources)`` — Not enough resources available
- ``(Priority)`` — Other jobs have higher priority
- ``(Dependency)`` — Waiting for another job to complete

Writing Efficient Job Scripts
------------------------------------

**Best Practices**

1. **Set realistic time limits**: Jobs with longer time limits have lower priority. Don't request 24 hours if you need 2 hours.

2. **Set realistic memory**: Check how much your job actually uses. Don't request 100GB if you need 8GB.

3. **Use the right number of cores**: More cores doesn't always mean faster. Test with different values.

4. **Load modules early**: Load required software before running commands.

5. **Test interactively first**: Use ``srun`` to test before submitting long batch jobs.

6. **Save outputs carefully**:

   .. code-block:: bash

      mkdir -p results/
      python analysis.py > results/output.txt
      python visualize.py > results/plots.txt

7. **Chain jobs with dependencies** instead of running them sequentially:

   .. code-block:: bash

      # Submit first job
      JOB1=$(sbatch job1.sh | awk '{print $NF}')

      # Submit job2 only after job1 completes
      sbatch --dependency=afterok:$JOB1 job2.sh

8. **Use arrays for many similar jobs**:

   .. code-block:: bash

      #!/bin/bash
      #SBATCH --job-name=parameter_sweep
      #SBATCH --array=1-100              # Run 100 copies with different SLURM_ARRAY_TASK_ID
      #SBATCH --time=01:00:00

      PARAM=$SLURM_ARRAY_TASK_ID
      python analysis.py --parameter $PARAM

Common SLURM Errors
------------------------------------

**"sbatch: error: Batch script must start with #! (or optionally a blank line)"**

Make sure your script starts with ``#!/bin/bash``

**"INVALID GRES SPECIFICATION" or similar**

Check your ``#SBATCH`` directives for typos. Common mistakes:
- ``--cpus-per-task`` not ``--cpus-per-tasks`` (no 's')
- ``--nodes`` not ``--node``

**"squeue shows my job but it won't start"**

Your job is pending. Check the reason:

.. code-block:: bash

   squeue -j YOUR_JOB_ID

If it says ``(Resources)``, the partition is busy. Try a shorter time limit or different partition.

**"Job killed because it ran out of memory (OUT_OF_MEMORY)"**

Increase ``--mem`` in your script:

.. code-block:: bash

   #SBATCH --mem=32G    # Instead of 16G

**"TIMEOUT - Job exceeded time limit"**

Increase ``--time``:

.. code-block:: bash

   #SBATCH --time=04:00:00    # Instead of 02:00:00

Useful Workflow
------------------------------------

Here's a typical workflow for running jobs on Falcon:

1. **Develop and test locally** on your laptop or in an interactive session
2. **Test with srun** on a small subset of data:

   .. code-block:: bash

      srun -N 1 -n 1 -c 4 -t 00:10:00 python analysis.py --small-data

3. **Write a batch script** for the full job
4. **Submit with sbatch**:

   .. code-block:: bash

      sbatch myjob.sh

5. **Monitor with squeue**:

   .. code-block:: bash

      squeue -u $USER

6. **Check results** when done:

   .. code-block:: bash

      cat job_12345.out
      cat job_12345.err

7. **Analyze and iterate** if needed

Example: Complete Job Script
------------------------------------

Here's a realistic example for a climate science job:

.. code-block:: bash

   #!/bin/bash
   #SBATCH --job-name=geos_chem_run
   #SBATCH --nodes=4
   #SBATCH --ntasks=128
   #SBATCH --cpus-per-task=1
   #SBATCH --time=06:00:00
   #SBATCH --mem=128G
   #SBATCH --partition=cpu
   #SBATCH --output=geos_chem_%j.out
   #SBATCH --error=geos_chem_%j.err
   #SBATCH --mail-type=END,FAIL
   #SBATCH --mail-user=user@cardiff.ac.uk

   # Load required modules
   module load intel/2023
   module load netcdf/4.9.2
   module load hdf5/1.12.2

   # Set up environment
   export OMP_NUM_THREADS=1
   export GEOS_CHEM_DATA=/data/geos_chem_inputs

   # Create output directory
   mkdir -p results/${SLURM_JOB_ID}
   cd results/${SLURM_JOB_ID}

   # Copy input files
   cp /home/user/input/* .

   # Run simulation
   srun ./GeosChem run_config.txt

   # Post-process results
   python /home/user/postprocess.py

   echo "Job completed at $(date)"

Need Help?
------------------------------------

- Check SLURM documentation: ``man sbatch``, ``man srun``, ``man squeue``
- Ask Omar or colleagues for script help
- Check the Falcon wiki for partition information
- Look at existing job scripts from groupmates
