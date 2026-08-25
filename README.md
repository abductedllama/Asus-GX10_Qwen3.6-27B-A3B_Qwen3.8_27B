# Asus-GX10_Qwen3.6-35B-A3B_Qwen3.8_27B
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
docker compose -f qwen-reason/docker-compose.yml up -d
docker compose -f qwen-3.8-27b/docker-compose.yml up -d

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

Memory after tweaks (20260824):
```bash
$ free -h
               total        used        free      shared  buff/cache   available
Mem:           121Gi       103Gi       1.1Gi       812Mi        19Gi        18Gi
Swap:           15Gi       2.0Gi        14Gi

$ nvidia-smi
Mon Aug 24 22:54:48 2026       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.173.02             Driver Version: 580.173.02     CUDA Version: 13.0     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GB10                    On  |   0000000F:01:00.0 Off |                  N/A |
| N/A   54C    P0             11W /  N/A  | Not Supported          |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A            3014      G   /usr/lib/xorg/Xorg                       18MiB |
|    0   N/A  N/A            3521      G   /usr/bin/gnome-shell                      6MiB |
|    0   N/A  N/A           16618      C   VLLM::EngineCore                      43079MiB |
|    0   N/A  N/A           16838      C   VLLM::EngineCore                      44699MiB |
+-----------------------------------------------------------------------------------------+
```

> **WIP** — initial setup, more to follow.
