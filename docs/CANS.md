# How to run CANS on Cedar

## Download singularity image
The following commands will create an image called `cans_latest.sif` in your current working directory.
```bash
# launch an interactive session with sufficient memory
salloc --time=2:0:0 --ntasks=1 --cpus-per-task=4 --mem-per-cpu=8G --account=your-account
# pull image from Docker Hub
singularity pull docker://jimmyliu1326/cans
```

## Run CANS pipeline using Singularity
You can either submit the following command using `sbatch` or run them in interactive sessions using `salloc`
```bash
singularity exec -H $PWD -B $PWD /path/to/cans_latest.sif \
  CANS.sh -i samples.csv -o /path/to/output --mode dynamic -e 2050
```
Above arguments explained:

* `-H`: specifies the default working directory when the container is launched. This is only important if you want to use relative paths in your CANS.sh call. By default, the working directory is set to your home directory: `$HOME`
* `-B`: mounts a specific directory to the container. By default, singularity containers only mounts the home directory. This is an issue when you run analyses on Compute Canada because you typically run analyses and store data under `/project` or `/scratch`. Thus, you will need to mount any directories outside of your home directory, otherwise the container will not have access to those directories.
