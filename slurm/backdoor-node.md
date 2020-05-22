# Run a 'backdoor' interactive node

If you have restrictive login or interactive nodes on your SLURM cluster, you can create a job and SSH into a node to perform work interactively.

This snippet below will submit a job for 3 days that simply sleep for 3 days so you can SSH into the node to do work interactively.

It is a good idea to keep resource requests low as this practice may be frowned upon (-;

```bash
sbatch --part=cpu --tasks-per-cpu=2 --time=3-00:00:00 <(echo -e '#!/bin/bash\nsleep 3d')
```


