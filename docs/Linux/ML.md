# ML Related

## Jupyter Notebook

```shell
nohup jupyter notebook --no-browser --port=8888 --ip=localhost &
```

## TensorBoard

```shell
tensorboard --logdir='./log' --port=10066
```

tensorboardX:

```python
from tensorboardX import SummaryWriter
self.writer = SummaryWriter('log')
self.writer.add_scalar('loss', loss, update_time)
```

## CUDA version mismatches GPU

```shell
export TORCH_CUDA_ARCH_LIST="7.5"
```

## Show Processes

Show dead processes that cannot be shown by `nvidia-smi`.

```shell
fuser -v /dev/nvidia*
```

## conda pack

```shell
conda activate base
conda pack -n my_env --ignore-editable-packages
# at last move the uncompressed dir my_env into conda env dir
```

## Nvidia

### driver

```shell
dpkg -l | grep nvidia

# remove driver
sudo apt remove --purge 'nvidia-.*'
sudo apt-get autoremove
```

```shell
# to fix: ERROR: Unable to find the kernel source tree for the currently running kernel. Please make sure you have installed the kernel source files for your kernel and that they are properly configured; on Red Hat Linux systems, for example, be sure you have the 'kernel-source' or 'kernel-devel' RPM installed. If you know the correct kernel source files are installed, you may specify the kernel source path with the '--kernel-source-path' command line option.

apt install linux-source
apt install linux-headers-$(uname -r)
```

