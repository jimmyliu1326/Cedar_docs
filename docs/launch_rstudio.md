## How to launch a RStudio server on Cedar cluster

### Activate dependencies for R >= 4.1.*
```
module load rstudio-server python
module load r/4.1.2
module load StdEnv/2020  intel/2020.1.217 proj4-fortran
source $HOME/jupyter_py3/bin/activate
salloc --time=12:0:0 --ntasks=1 --cpus-per-task=16 --mem-per-cpu=2G --account=rrg-whsiao-ab srun $VIRTUAL_ENV/bin/jupyterlab.sh
```

### Activate dependencies for R < 4.1.*
```
module load python/3.6
source $HOME/jupyter_py3/bin/activate
module load nixpkgs/16.09 gcc/7.3.0 rstudio-server/1.2.1335
salloc --time=12:0:0 --ntasks=1 --cpus-per-task=16 --mem-per-cpu=2G --account=rrg-whsiao-ab srun $VIRTUAL_ENV/bin/notebook.sh
```

### Connect local to remote
Run the following command in a new terminal
```
ssh -L 8888:<hostname:port> <username>@<cluster>.computecanada.ca
```

### Launch RStudio in browser
Open any browser and navigate to `http://localhost:8888/?token=<token>`

### Attach terminal to a running job
```
srun --pty --jobid $JOBID -w $NODEID /bin/bash
```
