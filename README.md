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

script.sh

```
#!/bin/bash

# run 100 wait tasks each one on a separate CPU
for i in `seq 1 100`; do
srun -n1 -N1 --exclusive python wait.py 10 &
done

wait
```


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

