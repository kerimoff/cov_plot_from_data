# cov_plot_from_data
This repo explains how to generate coverage_plot pdfs using the data by https://github.com/kerimoff/leafcutter_plot pipeline


## Run the script with container in HPC 
1. create a interactive Slurm srun session (head node does not have enough resources to build the container)

```bash
srun --partition=amd --time=02:30:00 --cpus-per-task=4 --mem=16G --pty /bin/bash
```

2. load needed HPC modules
```bash
module load any/singularity/3.5.3
module load squashfs/4.4
```

3. Pull and build the singularity image
```bash
mkdir singularity_img
singularity build singularity_img/leafcutter_plot.img docker://quay.io/eqtlcatalogue/recap_plot:v22.06.2
```

All the data needed for plotting is gathered here `/gpfs/space/projects/eQTLCatalogue/coverage_plots/V6_artemis_leafcutter_all` (Ask Kaur for access if you need)

I copied an example for convenience here `./data/`


1. Run the script with tar-file as input
```bash
singularity exec singularity_img/leafcutter_plot.img \
    Rscript bin/plot_from_data.R \
    --tar_file data/plot_data_1_2392126_2395784_clu_5647_-\&chr1_2376477_A_C\&ENSG00000157916.tar.gz \
    --outdir results_from_data/
```

or Rds-file as input

```bash
singularity exec singularity_img/leafcutter_plot.img \
    Rscript bin/plot_from_data.R \
    --rds_file data/plot_data_1_2394728_2395784_clu_30114_+\&chr1_2401220_TCC_T\&ENSG00000157916.Rds\
    --outdir results_from_data/
```

There should generate pdf files under `results_from_data` directory.

Note: while running scripts there can be some warnings outputted, but thery are OK. I can clean them if needed later. 