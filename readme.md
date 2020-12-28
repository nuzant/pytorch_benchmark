# Distributed Pytorch Benchmark

## Requirements
pytorch >= 1.6.0
torchvision >= 0.7.0

## Preparations
Download data:
```
$ wget -c https://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz
```
Make data and model directory, extract data. (Do not change directory names) On each cluster nodes, run following commands:
```
$ mkdir data/
$ tar -xvzf cifar-10-python.tar.gz --directory=data/
$ mkdir saved_models/
```

## Run benchmark code
On each cluster node, use torch.distributed.launch module to run benchmark.py. For example, assuming there are 3 nodes, the address of the first node is 192.168.51.225 (use `kubectl get pods -o wide`), then run the following commands should run the benchmark.

On first node:
```
python -m torch.distributed.launch --nproc_per_node=8 --nnodes=3 --node_rank=0 --master_addr="192.168.51.225" --master_port=1234 benchmark.py
```

On second node:
```
python -m torch.distributed.launch --nproc_per_node=8 --nnodes=3 --node_rank=1 --master_addr="192.168.51.225" --master_port=1234 benchmark.py
```

On third node:
```
python -m torch.distributed.launch --nproc_per_node=8 --nnodes=3 --node_rank=2 --master_addr="192.168.51.225" --master_port=1234 benchmark.py
```

Argument `--nproc_per_node` defines number of processes invoked. Each process occupies 1 CPU and 1 GPU. 