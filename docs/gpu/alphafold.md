### Alphafold

The Alphfold software is available as a module on the jhpce cluster.

```Shell 
[login31 /users/mmill116/alpha1]$ srun --pty --x11 --mem=100G --cpus-per-task=8 --partition gpu --gpus=1 bash
[compute-126 /users/mmill116/alpha1]$ module load alphafold
(alphafold-gpu) [compute-126 /users/mmill116/alpha1]$ nvidia-smi    (this is to verify that you are assigned a GPU)
(alphafold-gpu) [compute-126 /users/mmill116/alpha1]$ ls
query.fasta
(alphafold-gpu) [compute-126 /users/mmill116/alpha1]$ cat query.fasta
>dummy_sequence
GWSTELEKHREELKEFLKKEGITNVEIRIDNGRLEVRVEGGTERLKRFLEELRQKLEKKGYTVDIKIE
(alphafold-gpu) [compute-126 /users/mmill116/alpha1]$ run_alphafold.sh -d /legacy/alphafold/data -o . -f query.fasta  -t 2020-05-14 -n 8
I0305 14:37:51.029889 140001535510336 templates.py:857] Using precomputed obsolete pdbs /legacy/alphafold/data/pdb_mmcif/obsolete.dat.
I0305 14:37:52.841140 140001535510336 xla_bridge.py:353] Unable to initialize backend 'tpu_driver': NOT_FOUND: Unable to find driver in registry given worker: 
I0305 14:37:52.984758 140001535510336 xla_bridge.py:353] Unable to initialize backend 'rocm': NOT_FOUND: Could not find registered platform with name: "rocm". Available platform names are: Interpreter CUDA Host
 
. . .
 
I0305 15:27:23.305634 140001535510336 amber_minimize.py:178] alterations info: {'nonstandard_residues': [], 'removed_heterogens': set(), 'missing_residues': {}, 'missing_heavy_atoms': {}, 'missing_terminals': {}, 'Se_in_MET': [], 'removed_chains': {0: []}}
I0305 15:27:23.643896 140001535510336 amber_minimize.py:500] Iteration completed: Einit 1628.92 Efinal -2082.65 Time 0.49 s num residue violations 0 num residue exclusions 0 
I0305 15:27:23.721792 140001535510336 run_alphafold.py:277] Final timings for query: {'features': 2523.1148357391357, 'process_features_model_1_pred_0': 2.198369026184082, 'predict_and_compile_model_1_pred_0': 92.24924182891846, 'relax_model_1_pred_0': 5.6730005741119385, 'process_features_model_2_pred_0': 1.1774885654449463, 'predict_and_compile_model_2_pred_0': 95.11810064315796, 'relax_model_2_pred_0': 3.1857430934906006, 'process_features_model_3_pred_0': 0.9210422039031982, 'predict_and_compile_model_3_pred_0': 74.00832223892212, 'relax_model_3_pred_0': 3.2094907760620117, 'process_features_model_4_pred_0': 0.9562208652496338, 'predict_and_compile_model_4_pred_0': 73.54764223098755, 'relax_model_4_pred_0': 3.2828869819641113, 'process_features_model_5_pred_0': 0.9843571186065674, 'predict_and_compile_model_5_pred_0': 72.18165755271912, 'relax_model_5_pred_0': 3.2641286849975586}
 
(alphafold-gpu) [compute-126 /users/mmill116/alpha1]$ ls
query  query.fasta
(alphafold-gpu) [compute-126 /users/mmill116/alpha1]$ ls query
features.pkl  ranked_2.pdb        relaxed_model_1_pred_0.pdb  relaxed_model_5_pred_0.pdb  result_model_3_pred_0.pkl  unrelaxed_model_1_pred_0.pdb  unrelaxed_model_5_pred_0.pdb
msas          ranked_3.pdb        relaxed_model_2_pred_0.pdb  relax_metrics.json          result_model_4_pred_0.pkl  unrelaxed_model_2_pred_0.pdb
ranked_0.pdb  ranked_4.pdb        relaxed_model_3_pred_0.pdb  result_model_1_pred_0.pkl   result_model_5_pred_0.pkl  unrelaxed_model_3_pred_0.pdb
ranked_1.pdb  ranking_debug.json  relaxed_model_4_pred_0.pdb  result_model_2_pred_0.pkl   timings.json               unrelaxed_model_4_pred_0.pdb
 
``` 
To use alphafold without a GPU, you would run:
``` 
[login31 /users/mmill116/alpha1]$ srun --pty --x11 --mem=100G --cpus-per-task=8 bash
[compute-129 /users/mmill116/alpha2]$ module load alphafold
(alphafold-gpu) [compute-129 /users/mmill116/alpha2]$ ls
query.fasta
(alphafold-gpu) [compute-129 /users/mmill116/alpha2]$ cat query.fasta
>dummy_sequence
GWSTELEKHREELKEFLKKEGITNVEIRIDNGRLEVRVEGGTERLKRFLEELRQKLEKKGYTVDIKIE
(alphafold-gpu) [compute-129 /users/mmill116/alpha2]$ run_alphafold.sh -d /legacy/alphafold/data -o . -f query.fasta  -t 2020-05-14 -n 8 -g False
I0305 14:57:32.565061 140585066018624 templates.py:857] Using precomputed obsolete pdbs /legacy/alphafold/data/pdb_mmcif/obsolete.dat.
I0305 14:57:34.727731 140585066018624 xla_bridge.py:353] Unable to initialize backend 'tpu_driver': NOT_FOUND: Unable to find driver in registry given worker: 
2024-03-05 14:57:34.729707: W external/org_tensorflow/tensorflow/tsl/platform/default/dso_loader.cc:66] Could not load dynamic library 'libcuda.so.1'; dlerror: libcuda.so.1: cannot open shared object file: No such file or directory; LD_LIBRARY_PATH: /jhpce/shared/jhpce/core/JHPCE_tools/3.0/lib
2024-03-05 14:57:34.729740: W external/org_tensorflow/tensorflow/compiler/xla/stream_executor/cuda/cuda_driver.cc:265] failed call to cuInit: UNKNOWN ERROR (303)
I0305 14:57:34.730122 140585066018624 xla_bridge.py:353] Unable to initialize backend 'cuda': FAILED_PRECONDITION: No visible GPU devices.
 
. . .
I0305 16:54:48.980324 140585066018624 amber_minimize.py:408] Minimizing protein, attempt 1 of 100.
I0305 16:54:49.271893 140585066018624 amber_minimize.py:69] Restraining 574 / 1170 particles.
I0305 16:54:54.823270 140585066018624 amber_minimize.py:178] alterations info: {'nonstandard_residues': [], 'removed_heterogens': set(), 'missing_residues': {}, 'missing_heavy_atoms': {}, 'missing_terminals': {}, 'Se_in_MET': [], 'removed_chains': {0: []}}
I0305 16:54:55.044994 140585066018624 amber_minimize.py:500] Iteration completed: Einit 1776.57 Efinal -2114.81 Time 4.96 s num residue violations 0 num residue exclusions 0 
I0305 16:54:55.143276 140585066018624 run_alphafold.py:277] Final timings for query: {'features': 3216.2297768592834, 'process_features_model_1_pred_0': 6.108574390411377, 'predict_and_compile_model_1_pred_0': 821.0001108646393, 'relax_model_1_pred_0': 12.479053258895874, 'process_features_model_2_pred_0': 2.8280999660491943, 'predict_and_compile_model_2_pred_0': 780.6746006011963, 'relax_model_2_pred_0': 9.498374223709106, 'process_features_model_3_pred_0': 2.3144617080688477, 'predict_and_compile_model_3_pred_0': 732.0320601463318, 'relax_model_3_pred_0': 8.531827449798584, 'process_features_model_4_pred_0': 2.274519920349121, 'predict_and_compile_model_4_pred_0': 732.8238620758057, 'relax_model_4_pred_0': 9.743109226226807, 'process_features_model_5_pred_0': 2.1754395961761475, 'predict_and_compile_model_5_pred_0': 668.7868385314941, 'relax_model_5_pred_0': 8.610054969787598}
(alphafold-gpu) [compute-096 /users/mmill116/alpha2]$ ls
query  query.fasta
(alphafold-gpu) [compute-096 /users/mmill116/alpha2]$ ls query
features.pkl  ranked_1.pdb  ranked_4.pdb                relaxed_model_2_pred_0.pdb  relaxed_model_5_pred_0.pdb  result_model_2_pred_0.pkl  result_model_5_pred_0.pkl     unrelaxed_model_2_pred_0.pdb  unrelaxed_model_5_pred_0.pdb
msas          ranked_2.pdb  ranking_debug.json          relaxed_model_3_pred_0.pdb  relax_metrics.json          result_model_3_pred_0.pkl  timings.json                  unrelaxed_model_3_pred_0.pdb
ranked_0.pdb  ranked_3.pdb  relaxed_model_1_pred_0.pdb  relaxed_model_4_pred_0.pdb  result_model_1_pred_0.pkl   result_model_4_pred_0.pkl  unrelaxed_model_1_pred_0.pdb  unrelaxed_model_4_pred_0.pdb
 
```
 
In our testing, the GPU version took about 50 minutes to run, and the non-GPU version took nearly 2 hours.
 
You can use “run_alphafold.sh --help” to see all of the options for running alphafold.
 
You do also need a fair amount of RAM.  In the above exmaples, we requested
100GB, and in looking at the “sacct” output for these example jobs, 
only used about 17GB for the non-gpu run and 34GB for the gpu run.
 
```
[login31 /users/mmill116]$ sacct -j 2907929 -o JobID,JobName,partition,AllocTres%40,MaxVMSize,MAXRSS,State%20 --units=G
JobID           JobName  Partition                                AllocTRES  MaxVMSize     MaxRSS                State 
------------ ---------- ---------- ---------------------------------------- ---------- ---------- -------------------- 
2907929            bash     shared     billing=102408,cpu=8,mem=100G,node=1                                  COMPLETED 
2907929.ext+     extern                billing=102408,cpu=8,mem=100G,node=1          0          0            COMPLETED 
2907929.0          bash                               cpu=8,mem=100G,node=1    100.96G     17.47G            COMPLETED 
[login31 /users/mmill116]$ sacct -j 2907783 -o JobID,JobName,partition,AllocTres%40,MaxVMSize,MAXRSS,State%20 --units=G
JobID           JobName  Partition                                AllocTRES  MaxVMSize     MaxRSS                State 
------------ ---------- ---------- ---------------------------------------- ---------- ---------- -------------------- 
2907783            bash        gpu billing=102410,cpu=8,gres/gpu:tesa100=1+                                  COMPLETED 
2907783.ext+     extern            billing=102410,cpu=8,gres/gpu:tesa100=1+          0          0            COMPLETED 
2907783.0          bash            cpu=8,gres/gpu:tesa100=1,gres/gpu=1,mem+    109.65G     34.30G            COMPLETED 
``` 
