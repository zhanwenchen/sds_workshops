# Codebase for a Hydra-SLURM-based Project

Training codes are not included.

`python main.py -m hydra/launcher=submitit_slurm`

```python
python main.py -m hydra/launcher=submitit_slurm \
   unida_runs.models=lead,dance \
   unida_runs.datasets=offfice,officehome \
   unida_runs.transfers=all \
   unida_runs.settings=pda,osda \
   unida_runs.corruption=false,true \
   unida_runs.unida_enabled=true \
   tta_runs.tta_enabled=false&
```

```python
python main.py -m hydra/launcher=submitit_slurm +experiment=grid_search &

screen -S hydra_test
python main.py -m hydra/launcher=submitit_slurm +experiment=grid_search
```

Command A + D
screen -ls
screen -r hydra_test
screen -rx hydra_test # for force attaching
screen -Xs hydra_test kill # if that screen is gone/dead, this kills it

```python
python main.py -m hydra/launcher=submitit_slurm \
   unida_runs.models=lead \
   unida_runs.datasets=office \
   unida_runs.transfers=all \
   unida_runs.settings=osda \
   unida_runs.corruption=false \
   unida_runs.unida_enabled=true \
   tta_runs.tta_enabled=false
```
