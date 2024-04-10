|Short Name|Long Name|Explanation|
|-----|-----|----|
|PD|PENDING|Job is waiting to start|
|R|RUNNING|Job is currently running|
|CG|COMPLETING|Job has ended, clean-up has begun|
|CD|COMPLETED|Job finished normally, with exit code 0|
|F|FAILED|Job finished abnormally, with a non-zero exit code|
|CA|CANCELLED|Job was cancelled by the user or a sysadmin|
|OOM|OUT_OF_MEMORY|Job was killed for exceeding its memory allocation|
|TO|TIMEOUT|Job was killed for exceeding its time limit|

