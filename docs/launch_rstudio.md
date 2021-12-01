## Follow the steps to launch a RStudio server on Cedar cluster

### Activate dependencies
```
module load python/3.6
source $HOME/jupyter_py3/bin/activate
module load gcc rstudio-server
salloc --time=12:0:0 --ntasks=1 --cpus-per-task=4 --mem-per-cpu=4G --account=rrg-whsiao-ab srun $VIRTUAL_ENV/bin/notebook.sh
```

### Connect local to remote
Run the following command in a new terminal
```
ssh -L 8888:<hostname:port> <username>@<cluster>.computecanada.ca
```

### Launch RStudio in browser
Open any browser and navigate to `http://localhost:8888/?token=<token>`