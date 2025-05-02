
# Roberts Lab Workflows

These files are organized in pairs where a slurm job and corresponding shell script is provided with similar prefix IDs, listed and described here:

-   **02.01-bismark**: Script automatically identifies paired-end FASTQ files by matching *R1* and *R2* patterns, skips previously processed samples by parsing Bismark report logs, and assigns a unique file pair to each SLURM array task using SLURM_ARRAY_TASK_ID. The script includes error checks for missing or empty file references, and executes Bismark with parameters optimized for non-directional libraries, custom Bowtie2 scoring, and parallel processing. Its checkpoint-aware design avoids redundant computation and streamlines large-scale DNA methylation alignment workflows.

-   **04-bismark**: Script selects a paired-end sample file based on the SLURM_ARRAY_TASK_ID, skips any samples already recorded in a checkpoint log, and iteratively runs Bismark with different score_min values. For each alignment run, it creates dedicated output directories and generates summary logs. After all parameter tests for a given sample complete successfully, the sample is logged as processed. The script also parses Bismark report files to extract mapping efficiency metrics and compiles them into a CSV summary, supporting downstream parameter optimization and performance comparison.

-   [**05-bismark**](https://github.com/College-of-the-Environment-Hyak-Users/DNA-Methylation-Mapping/blob/main/srlab-DNA-Methylation-Mapping/05.1.sh): Script dynamically selects a sample based on the SLURM_ARRAY_TASK_ID, checks a checkpoint log to skip already processed samples, and runs Bismark with predefined alignment parameters, including a score_min setting optimized for non-directional libraries. Successful runs are recorded in a checkpoint file to prevent reprocessing, while logs are generated for troubleshooting and performance tracking.
