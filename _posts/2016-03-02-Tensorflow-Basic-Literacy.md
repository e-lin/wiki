---
layout: post
title:  "Tensorflow Basic Literacy"
date:   2016-03-02 00:00:00 +0900
categories: jekyll update
type: Tensorflow
excerpt_separator: <!--more-->
---
<!--more-->
Feeling like totally a graphical aspect within Tensorflow after reading its basic usage:

- it uses graph to represent computation
- nodes equal to operations (ops)
- edges mean tensors, we see a tensor as a multi-dimension array (datas, a numpy ndarray)
- need to launch the graph in a session
- variables, constant, fetches, and feeds
- a bit unfamiliar with interactive usage, use `tf.InteractiveSession()` to avoid keeping one variable holding the session.

[Basic Usage][R1]
---

### [Print the value of a Tensor object:][R6]
The easiest way to evaluate the actual value of a Tensor object is to pass it to the `Session.run()` method, or call `Tensor.eval()` when you have a default session (i.e. in a `with tf.Session()`: block, or see below).

``` python
sess = tf.InteractiveSession()

W = tf.Variable(numpy.random.randn(), name="weight")
W.initializer.run()

# print tensor
W.eval() # or sess.run(W)
```

When running tensorflow with the interactive python, we can execute `Tensor.eval()` or `Operation.run(op)` to avoid keeping a variable holding a session

``` python
# Enter an interactive TensorFlow Session.
import tensorflow as tf
sess = tf.InteractiveSession()

x = tf.Variable([1.0, 2.0])
a = tf.constant([3.0, 3.0])

# Initialize 'x' using the run() method of its initializer op.
x.initializer.run()

# Add an op to subtract 'a' from 'x'.  Run it and print the result
sub = tf.sub(x, a)
print(sub.eval())   ##### or print(sess.run(sub))
# ==> [-2. -1.]

# Close the Session when we're done.
sess.close()
```

[Difference between Session.run() and Tensor.run()][N1]

### Variables:
Variables maintain state across executions of the graph, and must be initialized by the following 2 ways:
#1. running an `init` Op:

``` python
state = tf.Variable(0, name=“counter”)
init_op = tf.initialize_all_variables()
sess.run(init_op)
```
#2. using the run() method of its initializer op:

``` python
x = tf.Variable([1.0,2.0])
x.initializer.run()    # x.initializer.run(session=sess)
```

### Fetch
To fetch multiple tensors:

``` python
intermed = tf.add(b, c)
mul = tf.mul(a, intermed)
result = sess.run( [mul, intermed] )  #### fetch 2 tensors
```

### Feed:
Feed is for patching a tensor into any operation in the graph

``` python
a = tf.placeholder(tf.int32)
one = tf.constant(1)
add = tf.add(a, one)

sess.run( [add], feed_dict={ a:[5] } )
```
A placeholder() operation will generate an error if you do not supply a feed for it.

[Variables: Creation, Initialization, Saving, and Loading][R2]
---

### Variables Saving and Restoring:

``` python
a = tf.Variable(..., name="v1")
b = tf.Variable(..., name="v2")
saver = tf.train.Saver()
save_path = saver.save(sess, “/Users/elsalin/model.ckpt")
```
This generates checkpoint files: checkpoint, model.ckpt, and model.ckpt.meta

In the last part of `model.ckpt.meta`, we can see the saved variable names as “v1” and “v2”. Each one of them is saved under the name that was passed when the variable was created.

Restoring variables from a file don't have to initialize them beforehand.

``` python
c = tf.Variable(..., name="v1")
d = tf.Variable(..., name="v2”)
saver = tf.train.Saver()
saver.restore(sess, "/Users/elsalin/model.ckpt”)
```
save\restroe a subset of variables:

``` python
v1 = tf.Variable(..., name="v1")
v2 = tf.Variable(..., name="v2”)
saver = tf.train.Saver({"my_v2": v2})
```

[Tensorflow Mechanics 101][R3]
---

`tf.app.run()` —> a wrapper handles flag parsing and then dispatches to main(). see at [https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/platform/default/_app.py][N2]

`tf.app.flags` —> a wrapper around python-gflag. Rather than an application having to define all flags in or near main(), each python module defines flags that are useful to it.

### Here is the flow of how it works:

#1. prepare datas

#2. build graph

- inference(): data model
- loss(): cross entropy (the way that how you define a good model)
- training()

#3. train model

- run in session
- train loop(): feed the graph(use feed_dict to input batches of datas), check status(print “loss” value), visualize status(TensorBoard), save checkpoint

#4. evaluate model

- eval graph
- eval output

[TensorBoard: Visualizing Learning][R4]
---

### Serialize data:
#1. create graph
use `tf.name_scope` to clean up the graph representation

``` python
with tf.name_scope(‘Wx_b’):
    y = tf.nn.softmax(tf.matmul(x, W) + b)
```
#2. annotate nodes with “summary operations”

``` python
tf.histogram_summary('weights', W)
# or
tf.scalar_summary('accuracy', accuracy)
tf.scalar_summary('cross entropy', cross_entropy)
```
#3. merge all summaries

``` python
merged = tf.merge_all_summaries()
# this returns a ‘string’ containing the serialized ‘summary’ protocol buffer.
```
#4. write to disk ( default out to /tmp/mnist_logs)

``` python
writer = tf.train.SummaryWriter(FLAGS.summaries_dir, sess.graph)
```
#5. every N steps, run

``` python
summary_str = sess.run(merged, feed_dict=feed)
writer.add_summary(summary_str, i)
```

### Launch Tensorboard:
#1. run the source _mnist_with_summaries.py_ under /examples/tutorials/mnist

#2. run `tensorboard --logdir=path/to/log-directory`

#3. see at `localhost:6006`

Reference:
---
[Tensorflow Basic Usage][R1]<br />
[Variables: Creation, Initialization, Saving, and Loading][R2]<br />
[Tensorflow Mechanics 101][R3]<br />
[TensorBoard: Visualizing Learning][R4]<br />
[TensorFlow Tutorial given by Dr. Chung-Cheng Chiu at Google Brain on Dec. 29, 2015 ][R5]<br />
[How to print the value of a Tensor object in TensorFlow?][R6]

[R1]: https://www.tensorflow.org/versions/r0.7/get_started/basic_usage.html
[R2]: https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/how_tos/variables/index.md
[R3]: https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/tutorials/mnist/tf/index.md
[R4]: https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/how_tos/summaries_and_tensorboard/index.md
[R5]: http://www.slideshare.net/tw_dsconf/tensorflow-tutorial
[R6]: http://stackoverflow.com/questions/33633370/how-to-print-the-value-of-a-tensor-object-in-tensorflow
[N1]: http://stackoverflow.com/questions/33610685/in-tensorflow-what-is-the-difference-between-session-run-and-tensor-eval
[N2]: https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/platform/default/_app.py
