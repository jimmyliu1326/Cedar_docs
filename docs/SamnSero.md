# Deploying and Running SamnSero on Compute Canada

#### 1. Activate Nextflow module

```bash
module load nextflow
```

#### 2. Install SamnSero pipeline

```bash
# request an interactive session using salloc
salloc --time=2:0:0 --cpus-per-task=16 --mem=16G --account=group-account
# call nextflow to clone pipeline source code
nextflow pull jimmyliu1326/SamnSero_Nextflow -r [version]
# print pipeline help msg
nextflow run jimmyliu1326/SamnSero_Nextflow -r [version] --help
# download centrifuge database
mkdir -p centrifuge_db && wget -O - https://genome-idx.s3.amazonaws.com/centrifuge/p_compressed%2Bh%2Bv.tar.gz | tar -xzv -C centrifuge_db
```

> You can find the latest pipeline version at https://github.com/jimmyliu1326/SamnSero_Nextflow

#### 3. Create a sample sheet (.csv) that maps sample names to data paths

Given FASTQ files organized under sample specific folders, you can use the `find` command to generate a sample sheet.

Raw Read File Directory Structure:
```
/path/to/data/
├── Sample_1
│   └── Sample_1.fastq.gz
├── Sample_2
│   └── Sample_2.fastq.gz
└── Sample_3
    ├── Sample_3a.fastq.gz
    └── Sample_3b.fastq.gz
```

Create sample sheet given above directory structure:
```
find /path/to/data/ -maxdepth 1 -mindepth 1 -type d -printf '%f,%p\n' > samples.csv
```

#### Running SamnSero Nextflow pipeline

**Method 1:** Running on an interactive node
```bash
# launch a running instance
screen
# request an interactive session
salloc --time=2:0:0 --cpus-per-task=16 --mem=16G --account=group-account
# run SamnSero
nextflow run jimmyliu1326/SamnSero_Nextflow -latest --input samples.csv --out_dir out --qc --annot --centrifuge /path/to/centrifuge_db/ --account group-account -profile slurm
```

**Method 2:** Submit pipeline run to Slurm in a single command
```bash
sbatch --error samnsero.error --output samnsero.out \
    --mem 16G --cpus-per-task 4 --time 2:0:0 \
    --wrap "nextflow run jimmyliu1326/SamnSero_Nextflow \
    -latest --input samples.csv --out_dir out --qc --annot --centrifuge /path/to/centrifuge_db/ --account group-account -profile slurm"
```

**Method 3:** Submit pipeline run to Slurm in a script

Prepare a bash script and save it as `Run_SamnSero.sh`:
```bash
#!/usr/bin/env bash
#SBATCH --time=02:00:00
#SBATCH --job-name=samnsero
#SBATCH --cpus-per-task=8
#SBATCH --mem=8G
#SBATCH --error=%x.error
#SBATCH --output=%x.out

module load nextflow apptainer
nextflow run jimmyliu1326/SamnSero_Nextflow \
    -latest \
    --input /path/to/samples.csv \
    --out_dir /path/to/out \
    --qc \
    --annot \
    --centrifuge /path/to/centrifuge_db/ \
    --account group-account \
    -profile slurm
```

Submit the bash script as a Slurm job:
```bash
sbatch Run_SamnSero.sh
```
