+++
title = "Parallel Programming in R on High Performance Computing (HPC) systems at CSU"
subtitle = "Rmpi implementation on Summit/Cray"

date = 2019-01-27T00:00:00
lastmod = 2018-01-27T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jialuo Liu"]

tags = ["Computing", "R"]
summary = "Rmpi implementation on Summit/Cray"

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["deep-learning"]` references
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects = ["internal-project"]

# Featured image
# To use, add an image named `featured.jpg/png` to your project's folder.
# [image]
  # Caption (optional)
  # caption = "Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)"

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, # BottomRight
  # focal_point = ""

  # Show image only in page previews?
  # preview_only = false

# Set captions for image gallery.

[[gallery_item]]
album = "gallery"
image = "theme-default.png"
caption = "Default"

[[gallery_item]]
album = "gallery"
image = "theme-ocean.png"
caption = "Ocean"

[[gallery_item]]
album = "gallery"
image = "theme-forest.png"
caption = "Forest"

[[gallery_item]]
album = "gallery"
image = "theme-dark.png"
caption = "Dark"

[[gallery_item]]
album = "gallery"
image = "theme-apogee.png"
caption = "Apogee"

[[gallery_item]]
album = "gallery"
image = "theme-1950s.png"
caption = "1950s"

[[gallery_item]]
album = "gallery"
image = "theme-coffee-playfair.png"
caption = "Coffee theme with Playfair font"

[[gallery_item]]
album = "gallery"
image = "theme-cupcake.png"
caption = "Cupcake"
+++

Parallel computing in Python and Matlab is similar, a useful post can be found [here](https://github.com/ResearchComputing/Parallelization_Workshop).

Good problems for such type of high performance computing systems are the so-called "Embarassingly parallelizable problems". That is, the problem can be separated to a number of mutually independent tasks.
## Road-map for Running Your R Code on Summit

### 1. Code Preparation.
For convenience, I often prepare three files:

- A bash file: (job.txt)

#### Summit

```{bash}
#!/bin/bash

#SBATCH -p shas                   # Send this job to the shas partition
#SBATCH -N 10                      # number of nodes
#SBTACH -J YourJobDescription
##SBATCH --mem=96000
#SBATCH --ntasks-per-node=24
#SBATCH -t 0-24:00                  # wall time (D-HH:MM)
#SBATCH -o spl.%j.out             # STDOUT (%j = JobId)
#SBATCH -e spl.%j.err             # STDERR (%j = JobId)
#SBATCH --mail-type=END,FAIL        # notifications for job done & fail
#SBATCH --mail-user=YourEmailAddress # send-to address

R_PROFILE=/home/<USERNAME>/R/x86_64-pc-linux-gnu-library/3.5/snow/RMPISNOWprofile;
export R_PROFILE
module load R
ml gcc
ml openmpi/2.0.1

date
START=`date +%s`
mpirun Rscript --no-save control.R <par1> <par2> <par3> ${SLURM_JOBID}.R_output
END=`date +%s`
date
ELAPSED=$(( $END - $START ))
echo "Elapsed time (hrs):
$(echo "scale=10; $ELAPSED/3600" | bc)"
```
#### Cray

```{bash}
#!/bin/bash
#PBS -N mod
#PBS -j oe
#PBS -l mppwidth=120 # number of cores
#PBS -l walltime=01:00:00
#PBS -q small
cd $PBS_O_WORKDIR
R_PROFILE=$HOME/lustrefs/R/library/snow/RMPISNOWprofile; export R_PROFILE
date
START=`date +%s`
aprun -B R --no-save --no-restore CMD BATCH control.R $PBS_JOBNAME.R_output
END=`date +%s`
date
ELAPSED=$(( $END - $START ))
echo "Elapsed time (hrs):
$(echo "scale=10; $ELAPSED/3600" | bc)"
```

- A control R file: (control.R).

```{r}
# -------------------------------------------------- #
#        Install and load needed R packages          #
# -------------------------------------------------- #
list.of.packages <- c("Rmpi","doSNOW","foreach","doRNG","iterators","data.table","parallel")
new.packages <- list.of.packages[!(list.of.packages %in% installed.packages()[,"Package"])]
if(length(new.packages)) install.packages(new.packages)
invisible(lapply(list.of.packages, require, character.only = TRUE))

# You can pass arguments here
args <- commandArgs(TRUE)
par1 <- as.double(args[1])
par2 <- as.double(args[2])
par3 <- as.double(args[3])

working.directory = '/path/to/working/directory'
worker.script = 'main.R'

cl = getMPIcluster()
clusterExport(cl, ls())
setwd(working.directory)
source(worker.script)

clusterEvalQ(cl, setwd(working.directory))
clusterEvalQ(cl, source(worker.script))
registerDoSNOW(cl)
rdata_name = paste0("Cov",CovStructure,"Type",Type,"NS",ns,".RData")
nreps = 400
iter0 = 1:nreps
seedM = rbind(rep(401,nreps),c(iter0),c(iter0))
res = foreach(iter = 1:400, .combine=list,.maxcombine=400,.options.RNG=seedM,
              .multicombine=TRUE, .errorhandling = 'remove') %dorng% {
		res =  run.sim()
    # I used to save my results for each iteration, in case my code won't finish and lose everything.
                save(res,file=paste0(working.directory,"/results/par1",par1,"/par2",par2,"/par3",par3,"/res",iter,".Rdata"))
                return(format(object.size(test),units = "Mb"))
              }
.Last = NULL
save.image(rdata_name)
stopCluster(cl)
```

- And an R file containing the main function (main.R).


### 2. Set Up Your R environment
For Summit users, you can follow the instructions [here](https://github.com/ResearchComputing/Parallelization_Workshop/blob/master/Day4-Parallel_R/01-R-setup.pdf).
For Cray users, you can follow the [instructions](https://www.stat.colostate.edu/~jah/Computing_Hints/files/CRAY.tutorial.pdf) by Dr. Jennifer A. Hoeting.


### 3. Submit your Job

On Summit, in your terminal, type

```{bash}
sbatch job.txt
```

On Cray, enter

```{bash}
qsub job.txt
```
