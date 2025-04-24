# Finetuning LLMs with PyTorch/Llama-Factory

## What is and Why Use Llama-Factory?

1. Support most finetuning methods - SFT/DPO/PPO/KTO, etc
2. Easy API for training and deployment

## 0. Make sure you have GPU access

```console
nvidia-smi
(base) zhanwen@zhanwen-mini:~/sds_workshops$ nvidia-smi
Thu Apr 24 17:20:17 2025
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 570.133.20             Driver Version: 570.133.20     CUDA Version: 12.8     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 3090 Ti     On  |   00000000:01:00.0  On |                  Off |
|  0%   44C    P8             28W /  480W |     379MiB /  24564MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
|   1  NVIDIA GeForce RTX 3090 Ti     On  |   00000000:02:00.0 Off |                  Off |
|  0%   46C    P8             29W /  480W |      15MiB /  24564MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
```

## 1. Prepare Environment

```bash
module load miniforge cuda
conda create -n lf python=3.12
conda activate lf
```

## 2. Clone LlamaFactory repo for Structure


```bash
git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
pip install deepspeed py-cpuinfo wandb
```

## 3. Configuring Llama-Factory

The main configuration file is [./LLaMA-Factory/examples/train_full/llama3_full_sft.yaml](../LLaMA-Factory/examples/train_full/llama3_full_sft.yaml)

The additional `deepspeed` (run by `accelerate`) configuration is at [../LLaMA-Factory/examples/deepspeed/ds_z3_offload_config.json](../LLaMA-Factory/examples/deepspeed/ds_z3_offload_config.json)


## 4. Prepare Data

Both the datasets and the configuration See the `data` folder.

The data we are using is at [../LLaMA-Factory/data/alpaca_en_demo.json](../LLaMA-Factory/data/alpaca_en_demo.json)

Dataset format definitions are in [./LLaMA-Factory/data/dataset_info.json](../LLaMA-Factory/data/dataset_info.json)

## 5.1 Train with GUI

```bash
GRADIO_SHARE=1 llamafactory-cli webui
```

## 5.2 Train on the command line

```bash
llamafactory-cli train examples/train_lora/llama3_lora_sft.yaml
```

## 5.3 (LoRA) Merge LoRA Adapter Weights

```bash
llamafactory-cli export examples/merge_lora/llama3_lora_sft.yaml
```


## 6. Deploy trained model (GUI)

```bash
GRADIO_SHARE=1 GRADIO_SERVER_PORT=8080 llamafactory-cli webchat /home/zhanwen/finetuners/saves/llama3.2-1b/lora/sft-lora-10000-20250128181805/config_webchat.yaml |& tee logs_eval/log_eval_$(date +"%Y-%m-%d-%H%M%S").log
```

## 6.2 Deploy Trained Model (OpenAP-Like API Endpoint with VLLM)

```bash
API_PORT=8000 llamafactory-cli api examples/inference/llama3_vllm.yaml
```
