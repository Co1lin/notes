# Neural Network

## Backpropagation

利用链式法则推导反向传播：

![image-20210610143658606](NN.assets/image-20210610143658606.png)

![image-20210610143741705](NN.assets/image-20210610143741705.png)

![image-20210610143802249](NN.assets/image-20210610143802249.png)

可以看到，第 $k$ 个隐含层的偏导数表达式中的 $\delta_{(k+1),m}$ 是第 $k+1$ 个隐含层中算过的。
