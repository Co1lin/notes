# PyTorch

## Fix random seed:

```python
torch.manual_seed(config['seed'])
torch.cuda.manual_seed_all(config['seed'])
np.random.seed(config['seed'])
random.seed(config['seed'])
```

## Distributed

Env:

```python
import torch.distributed as dist

parser.add_argument("--local_rank", type=int, help='local rank for DistributedDataParallel')

opt = parse_option()
torch.cuda.set_device(opt.local_rank) # dist.get_rank()
torch.distributed.init_process_group(backend='nccl', init_method='env://')
```

Sampler and dataloader:

```python
from torch.utils.data.distributed import DistributedSampler
from torch.utils.data import DataLoader
train_sampler = DistributedSampler(TRAIN_DATASET) # arg shuffle=True by default

train_loader = DataLoader(TRAIN_DATASET,
                           batch_size=args.batch_size,
                           shuffle=False,
                           num_workers=args.num_workers,
                           sampler=train_sampler,
                           drop_last=True)
```

Model:

```python
from torch.nn.parallel import DistributedDataParallel
model = model.cuda()
# arg broadcast_buffers=True by default enables sync_batchnorm
model = DistributedDataParallel(model, device_ids=[dist.get_rank()])
```

Run in shell:

```python
CUDA_VISIBLE_DEVICES=0,1,2,3 \
python -m torch.distributed.launch --nproc_per_node 4 \
    <your python script>
```

## cuDNN

```python
torch.backends.cudnn.enabled = True
torch.backends.cudnn.benchmark = True
# fix the algorithm for convolution
# torch.backends.cudnn.deterministic = True
```

