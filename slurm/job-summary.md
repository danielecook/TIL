# Dump metrics for all slurm jobs

The command below can be used to dump metrics for jobs run on a Slurm cluster:

```bash
sacct --starttime 2019-02-01 --long -p --delimiter "|" > out.tsv
```

Output is delimited with a `pipe`:

```
JobID|JobIDRaw|JobName|Partition|MaxVMSize|MaxVMSizeNode|MaxVMSizeTask|AveVMSize|MaxRSS|MaxRSSNode|MaxRSSTask|AveRSS|MaxPages|MaxPagesNode|MaxPagesTask|AvePages|MinCPU|MinCPUNode|MinCPUTask|AveCPU|NTasks|AllocCPUS|Elapsed|State|ExitCode|AveCPUFreq|ReqCPUFreqMin|ReqCPUFreqMax|ReqCPUFreqGov|ReqMem|ConsumedEnergy|MaxDiskRead|MaxDiskReadNode|MaxDiskReadTask|AveDiskRead|MaxDiskWrite|MaxDiskWriteNode|MaxDiskWriteTask|AveDiskWrite|AllocGRES|ReqGRES|ReqTRES|AllocTRES|
13491519|13491519|simple-test|compute||||||||||||||||||2|00:00:00|COMPLETED|0:0||Unknown|Unknown|Unknown|128Mc||||||||||||cpu=1,mem=128,node=1|cpu=2,mem=256,node=1|
13491519.batch|13491519.batch|batch||145792K|ca009|0|145792K|0|ca009|0|0|0|ca009|0|0|00:00:00|ca009|0|00:00:00|1|2|00:00:00|COMPLETED|0:0|1.20G|0|0|0|128Mc|0|0.00M|ca009|0|0.00M|0.00M|ca009|0|0.00M||||cpu=2,mem=256,node=1|```

This can be very useful for benchmarking purposes.