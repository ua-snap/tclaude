# Working on Chinook under tclaude (mandatory)

- You run inside a bubblewrap sandbox. Only the repo, ~/tmp, ~/.claude and the
  directories in TCLAUDE_WRITE_DIRS are writable. A "Read-only file system" error
  means the sandbox is working: stop and tell the user. Never look for a workaround.
- Source datasets are READ-ONLY even inside writable directories: /import/SNAP
  (SNAP's canonical data) and /path/to/datasets.
  Copy data before any operation that could modify it, and work on the copy.
  NCO: always write a new output file; never run ncatted, ncrename, ncks -A or -O
  on source files. Python/R: open source files read-only.
- Don't run heavy computation on the login node; write and submit a Slurm job.
- Slurm works through the tclaude broker: sbatch, squeue, sacct, sinfo, sstat,
  scontrol show, and scancel (for jobs submitted from tclaude only). srun and
  salloc are unavailable, as are Slurm commands inside jobs.
- sbatch takes the job script as a file (not stdin) and only these options:
  --account --array --chdir --comment --constraint --cpus-per-task --dependency
  --error --exclude --gres --job-name --mail-type --mail-user --mem --mem-per-cpu
  --nodelist --nodes --ntasks --ntasks-per-node --output --partition --qos
  --signal --time --time-min --wrap --exclusive --hold --parsable --requeue
  --no-requeue --test-only (and their short forms). Use full option names.
- Jobs get the environment of the shell where tclaude started, not yours:
  activate conda or load modules inside the job script.
- Slurm's own messages (time limit, out of memory, cancellation) are in
  ~/.cache/tclaude/slurm-logs/<jobid>.out, not in the job's log file.
