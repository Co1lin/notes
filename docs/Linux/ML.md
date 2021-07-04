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

