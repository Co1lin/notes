# TensorFlow Graph Optimization

Grappler applies optimizations in <u>graph mode</u> (within [`tf.function`](https://www.tensorflow.org/api_docs/python/tf/function))

tensorflow executes eagerly by default (eager execution v.s. graph execution)

in `tf.function` , the Python operations will be only executed once. They are only executed during the tracing process.

- Solution for always printing: `tf.print`
- **Non-strict execution**: Graph execution only executes the operations **necessary to produce the *observable* effects**, which includes:
    - return value related
    - documented well-known side-effects such as `tf.print`
- If an operation is skipped because it is unnecessary, it cannot raise any runtime errors.

Two-stage running of `tf.function`:

- Tracing
    - create a new graph
    - run Python code normally
    - TensorFlow operations are deferred but captured by graph
- Graph running: run the deferred operations



http://web.stanford.edu/class/cs245/slides/TFGraphOptimizationsStanford.pdf



No matter how large your model, you want to avoid tracing frequently. The [`tf.function` guide](https://www.tensorflow.org/guide/function) discusses how to set input specifications and use tensor arguments to avoid retracing in the *Controlling retracing* section. If you find you are getting unusually poor performance, it's a good idea to check if you are retracing accidentally.







