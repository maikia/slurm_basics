# access cluster

ssh USER@CLUSTERIP
pass: PASS

http://CLUSTERIP/ganglia
http://CLUSTERIP/docs/index.html

# basic job management in SLURM

for info on the cluster:

`sinfo`

for info on the nodes:
`squeue`

to force quit the job:

```
scancel -u mtelenczuk
scancel numer_job
```

to run single command in python:

```
salloc -p amd # otwiera nowy shell z dostepem do klastra (partycja amd)
srun python -c 'script here' # uruchamia komende w pythonie na klustrze
ctrl+D # zamyka shell
```

# install conda

```
wget  https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
bash Miniconda2-latest-Linux-x86_64.sh
conda install numpy matplotlib scipy ipython
# add conda to path
echo "export PATH=/home/mtelenczuk/miniconda2/bin:$PATH" >> ~/.bashrc
```


# install neuron

```
wget http://www.neuron.yale.edu/ftp/neuron/versions/v7.4/nrn-7.4.tar.gz
mkdir neuron
tar xzvf nrn-7.4.tar.gz 
cd nrn-7.4/
./configure --prefix=$HOME/neuron --with-nrnpython --without-iv --with-mpi
make -j4
make install
cd src/nrnpython/
python setup.py install
```

# Sample batch task

To schedule a job asychronously (for running later) you need to write a batch script and execute it with `sbatch`. The script
can be written with bash:

script.sh

```
#!/bin/bash

# run 100 wait tasks each one on a separate CPU
for i in `seq 1 100`; do
#note the -n1 -N1 --exclusive options and the ampersand (&) at the end
# 10 is the number of seconds to wait (argument of wait.py script)
srun -n1 -N1 --exclusive python wait.py 10 &
done

# don't forget to wait otherwise the job will be closed permaturely
wait
```

The task itself is a Python script:

wait.py

```
"""prints and waits for N seconds (passed in the command line)"""

import time

import sys

print "wait " + sys.argv[1]
time.sleep(int(sys.argv[1]))
print "finished"
```

To run the wait.py script on 10 processors:

`sbatch -n10 script.sh`

If number of CPUs on one node is not sufficient (n_cpus < 10) `sbatch` will run the processes on other nodes as well.

The job consists of 100 job steps, but only 10 can be executed simulatenously. To check the running job steps use:

`squeue -s`

To exclude a node from the computation use:

`sbatch -n10 --exclude=node01 script.sh`

