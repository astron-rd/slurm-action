# SLURM GitHub Actions Runner

This repository contains a custom GitHub Action for running jobs on a SLURM
cluster.

## Usage

### Workflow Example

```yaml
name: SLURM Job Example
on: [push]

jobs:
  run-on-slurm:
    runs-on: [self-hosted, slurm]
    steps:
      - uses: actions/checkout@v3
      - uses: ./.github/actions/slurm-action
        with:
          partition: defq
          gres: gpu:A4000  # Omit for CPU jobs
          time: 00:10:00   # Max runtime (HH:MM:SS)
          commands: |
            echo "Running on $(hostname)"
            nvidia-smi
            <your commands>
```

## Input Parameters

| Parameter | Required | Default  | Description                     |
| --------- | -------- | -------- | ------------------------------- |
| partition | No       | defq     | SLURM partition name            |
| gres      | No       | (none)   | GPU resources (e.g., gpu:A4000) |
| time      | No       | 00:10:00 | Maximum job runtime             |
| commands  | Yes      | -        | Commands to execute on SLURM    |

## Features

- Real-time output
- Automatic cleanup of temporary files
- Supports GPU resource requests via GRES
- Proper error code propagation

# Requirements

- Self-hosted runner on SLURM control node
- `srun` command available
- Shared filesystem access to `$HOME`

## Setup

1. Register runner with `slurm` label:

```bash
./config.sh --labels slurm,self-hosted
```

2. Place this action in .github/actions/slurm/action.yml
1. Add `uses: .github/actions/slurm/action.yml` to your workflow
1. Or use this action directly by specifying: `uses: astron-rd/slurm-action@v1`
