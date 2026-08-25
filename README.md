# Asus-GX10_Qwen3.6-27B-A3B_Qwen3.8_27B
Configuration for Running Qwen3.6 35b MOE and Qwen3.8 27b Dense

Running on **ASUS GX10** (ARM64) via vLLM with Docker Compose.

This may or may not be optimal. I was just trying to get two decently large models with a large context, running smoothly under any request.

## Models

| Service | Model | Purpose | Context Limit | Port |
|---|---|---|---|---|
| `qwen-reason` | `sakamakismile/Huihui-Qwen3.6-35B-A3B-abliterated-NVFP4` | Reasoning | 262,144 | 8000 |
| `qwen38-aeon` | `sakamakismile/Qwen3.8-27B-AEON-ULTIMATE-UNCENSORED-NVFP4` | Coding | 65,536 | 8003 |

## Quick Start

```bash
# Start both containers
docker compose -f vllm/docker-compose.yml up -d
docker compose -f qwen-3.8-28b/docker-compose.yml up -d

# Test
curl http://localhost:8000/v1/models
curl http://localhost:8003/v1/models
```

## Hardware

- **ASUS GX10** (ARM64, GB10 (SM121), ~128 GB unified memory)
  - Should also work in theory on other Nvidia Spark machines
- Both containers use `nvidia` driver for GPU access

## Status

- `docker-compose.yml` files are live
- More details about tuning, concurrency, and optimization coming soon

Pushes memory pretty far:
```bash
$ free -h
               total        used        free      shared  buff/cache   available
Mem:           121Gi       115Gi       5.0Gi       767Mi       2.9Gi       5.7Gi
Swap:           15Gi       8.0Gi       8.0Gi

$ nvidia-smi
Fri Aug 21 00:30:09 2026       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.173.02             Driver Version: 580.173.02     CUDA Version: 13.0     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GB10                    On  |   0000000F:01:00.0 Off |                  N/A |
| N/A   63C    P0             44W /  N/A  | Not Supported          |     96%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A            3154      G   /usr/lib/xorg/Xorg                       18MiB |
|    0   N/A  N/A            3359      G   /usr/bin/gnome-shell                      6MiB |
|    0   N/A  N/A            6008      C   VLLM::EngineCore                      56501MiB |
|    0   N/A  N/A           44757      C   VLLM::EngineCore                      48829MiB |
+-----------------------------------------------------------------------------------------+


```

> **WIP** — initial setup, more to follow.
