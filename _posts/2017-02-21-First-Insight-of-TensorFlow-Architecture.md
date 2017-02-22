---
layout: post
title:  "First Insight of TensorFlow Architecture"
date:   2017-02-21 00:00:00 +0900
categories: jekyll update
type: Tensorflow
excerpt_separator: <!--more-->
---
<!--more-->

Before Going to the Architecture
---
They assume you already knew about the basic knowledge of [TensorFlow programming concepts][R4], and it will be helpful if you also knew about [distributed TensorFlow][R2].


Distributed TensorFlow
---

### Glossary

- Task: Corresponding to a specific `process`, which could be a TensorFlow Server.
- TensorFlow Server: A process, which runs [tf.train.Server][R5] instance, and output `Master Service` and `Worker Service`.
- Master Service: A RPC service.
- Worker Service: A RPC service.

> Note: All TensorFlow Servers implement Master Service. All TensorFlow Servers implement Worker Service.

- Job: Contains one or more tasks. If named with `ps`, meaning `parameter server`; if named with `worker`, meaning performs compute-intensive tasks. You may define the scope of the job.
- Cluster: Contains one or more jobs. Meaning high-level objectives, e.g., to train a neural network, or use many machines in parallel.
- Client: A program we write. It constructs a `tf.Session`, and then interacts with the Cluster.

### An example using distributed TensorFlow

// Start a TensorFlow server as a single-process cluster.

```
import tensorflow as tf
c = tf.constant('Hi to distributed TensorFlow!')
server = tf.train.Server.create_local_server()
```

// Create a session on the server

```
sess = tf.Session(server.target)
sess.run(c)
```

// Prints out

```
'Hello, distributed TensorFlow!'
```

Create a Cluster
---

As we said, we can define the scope of the cluster, e.g., this cluster contains a set of tasks that are preparing to train a model, etc.

There are two steps to create a cluster. First, you need a Spec, to define what does it look like over all of the tasks in the cluster. Second,

### 1. Create a `tf.train.ClusterSpec` to describe the cluster

// Define a dictionary that maps `job names` to `lists of network address`.

```
dict = {'local': ['localhost:2222', 'localhost:2223']}
```

// Pass the dictionary to the `tf.train.ClusterSpec` constructor.

```
tf.train.ClusterSpec(dict)
```

// Then available tasks would be like:

```
/job:local/task:0
/job:local/task:1
```

// If you want to define more jobs, create the dictionary like:

```
dict = {
    'ps': [
        'ps0.example.com:2222',
        'ps1.example.com:2222'
    ],
    'worker': [
        'worker0.example.com:2222',
        'worker1.example.com:2222',
        'worker2.example.com:2222',
    ]}
```

// Pass the dictionary to the `tf.train.ClusterSpec` constructor.

```
tf.train.ClusterSpec(dict)
```

// Then available tasks would be like:

```
/job:ps/task:0
/job:ps/task:1
/job:worker/task:0
/job:worker/task:1
/job:worker/task:2
```

### 2. Create a `tf.train.Server` instance in each task

Quoting from official doc:

> A `tf.train.Server` contains a set of local devices, and a set of connections to other tasks in its `tf.train.ClusterSpec`, and `tf.Session` that can use those to perform a distributed computation.

> Each server is a member of a specific `named job`, and has a `task index` within that job.

For example, declaring like this:

```
cluster = tf.train.ClusterSpec({"local": ["localhost:2222", "localhost:2223"]})

# In task 0:
server = tf.train.Server(cluster, job_name='local', task_index=0)
# In task 1:
server = tf.train.Server(cluster, job_name='local', task_index=1)
```

Currently, we can only **manually** specifying these cluster specifications can be tedious, especially for large clusters. TensorFlow team is working on tools for launching tasks programmatically, e.g. using a cluster manager like [Kubernetes][R6]. If there are particular cluster managers for which you'd like to see support, you can raise a [GitHub issue][R7].


Specifying Distributed Devices in Your Model
---

To place operations on a particular process, you can use the same `tf.device` function that is used to specify whether ops run on the CPU or GPU. For example:

```
with tf.device('/job:ps/task:0'):
    w1 = tf.Variable(...)
    b1 = tf.Variable(...)

with tf.device('/job:ps/task:1'):
    w2 = tf.Variable(...)
    b2 = tf.Variable(...)

with tf.device("/job:worker/task:7"):
  input, labels = ...
  layer_1 = tf.nn.relu(tf.matmul(input, w1) + b1)
  logits = tf.nn.relu(tf.matmul(layer_1, w2) + b2)
  # ...
  train_op = ...

with tf.Session("grpc://worker7.example.com:2222") as sess:
  for _ in range(10000):
    sess.run(train_op)
```

Quoting from official doc:

> In the above example, the variables are created on two tasks in the `ps` job, and the compute-intensive part of the model is created in the `worker` job. TensorFlow will insert the appropriate data transfers between the jobs (from `ps` to `worker` for the forward pass, and from `worker` to `ps` for applying gradients).


### Some APIs of [tf.train.ClusterSpec][R8]

// Create a cluster.

```
>>> dict = {
...     'ps': [
...         'ps0.example.com:2222',
...         'ps1.example.com:2222'
...     ],
...     'worker': [
...         'worker0.example.com:2222',
...         'worker1.example.com:2222',
...         'worker2.example.com:2222',
...     ]}
>>> dict
{'ps': ['ps0.example.com:2222', 'ps1.example.com:2222'], 'worker': ['worker0.example.com:2222', 'worker1.example.com:2222', 'worker2.example.com:2222']}
>>> cluster = tf.train.ClusterSpec(dict)
```

// Examples...

```
>>> cluster.jobs
['ps', 'worker']
>>> cluster.as_cluster_def()
job {
  name: "ps"
  tasks {
    value: "ps0.example.com:2222"
  }
  tasks {
    key: 1
    value: "ps1.example.com:2222"
  }
}
job {
  name: "worker"
  tasks {
    value: "worker0.example.com:2222"
  }
  tasks {
    key: 1
    value: "worker1.example.com:2222"
  }
  tasks {
    key: 2
    value: "worker2.example.com:2222"
  }
}
>>> cluster.as_dict()
{'ps': ['ps0.example.com:2222', 'ps1.example.com:2222'], 'worker': ['worker0.example.com:2222', 'worker1.example.com:2222', 'worker2.example.com:2222']}
>>> cluster.job_tasks('worker')
['worker0.example.com:2222', 'worker1.example.com:2222', 'worker2.example.com:2222']
>>> cluster.num_tasks('worker')
3
>>> cluster.task_address('worker', 2)
'worker2.example.com:2222'
>>> cluster.task_indices('worker')
[0, 1, 2]
```

Replicated Training (Data Parallelism)
---
which could mean:

- Involve multiple tasks in a job training the same model on different mini-batches of data.
- Update shared parameters hosted in one or more tasks in a job.

All tasks typically run on different machines.

There are many ways to specify this structure in TensorFlow, and TensorFlow team is building libraries that will simplify the work of specifying a replicated model.

Possible approaches:

- In-graph replication
- Between-graph replication
- Asynchronous training
- Synchronous training

### Read more

// A related post over stackoverflow
- [task assignment in tensorflow distributed process][R9]

// A whole example of using distributed TensorFlow
- [Putting it all together: example trainer program][R10]

// How Baidu accelerate
- [Baidu's 'Ring Allreduce' Library Increases Machine Learning Efficiency Across Many GPU Nodes][R11]

Coming up...

- [Understand Op Registration and Kernel Linking in TensorFlow][R12]
- [No OpKernel was registered to support Op 'L2Loss' with these attrs [[Node: L2Loss = L2Loss[T=DT_INT64](L2Loss/t)]]][R13]

Reference
---
- [TensorFlow Architecture][R1]
- [Distributed TensorFlow][R2]
- [tf.train.ClusterSpec][R8]
- [關於RPC][R3]


[R1]: https://www.tensorflow.org/extend/architecture
[R2]: https://www.tensorflow.org/deploy/distributed
[R3]: http://cjy0503.pixnet.net/blog/post/28330879-%E9%97%9C%E6%96%BCrpc
[R4]: https://www.tensorflow.org/get_started/get_started
[R5]: https://www.tensorflow.org/api_docs/python/tf/train/Server
[R6]: http://kubernetes.io/
[R7]: https://github.com/tensorflow/tensorflow/issues
[R8]: https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec
[R9]: http://stackoverflow.com/questions/41067398/task-assignment-in-tensorflow-distributed-process
[R10]: https://www.tensorflow.org/deploy/distributed#putting_it_all_together_example_trainer_program
[R11]: http://www.tomshardware.com/news/baidu-svail-ring-allreduce-library,33691.html
[R12]: http://stackoverflow.com/questions/37548662/understand-op-registration-and-kernel-linking-in-tensorflow
[R13]: http://stackoverflow.com/questions/41385137/no-opkernel-was-registered-to-support-op-l2loss-with-these-attrs-node-l2los