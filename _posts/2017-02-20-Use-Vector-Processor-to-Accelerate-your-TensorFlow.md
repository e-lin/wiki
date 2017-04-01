---
layout: post
title:  "Use Vector Processor to Accelerate your TensorFlow"
date:   2017-02-20 00:00:00 +0900
categories: jekyll update
type: Tensorflow
excerpt_separator: <!--more-->
---
<!--more-->

Started from an Error Message
---
When trying to use the API of `tf.count_nonzero`, I encountered this error message:

```
AttributeErrorTraceback (most recent call last)
<ipython-input-7-958f24d508e6> in <module>()
     97
     98 if __name__ == '__main__':
---> 99   main()

<ipython-input-7-958f24d508e6> in main()
     79     predicted = tf.round(tf.nn.sigmoid(pred))
     80     actual = y
---> 81     tp = tf.count_nonzero(predicted * actual)
     82     tn = tf.count_nonzero((predicted - 1) * (actual - 1))
     83     fp = tf.count_nonzero(predicted * (actual - 1))

AttributeError: 'module' object has no attribute 'count_nonzero'
```

It seems that I need to update the version of TensorFlow. After upgrading, although I can use that API, some warning messages showed up saying that:

```
>>> import tensorflow as tf
>>> a = tf.count_nonzero([0,0,3])
>>> with tf.Session() as sess:
...     print(a.eval())
...
W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE4.1 instructions, but these are available on your machine and could speed up CPU computations.
W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use SSE4.2 instructions, but these are available on your machine and could speed up CPU computations.
W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use AVX instructions, but these are available on your machine and could speed up CPU computations.
W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use AVX2 instructions, but these are available on your machine and could speed up CPU computations.
W tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use FMA instructions, but these are available on your machine and could speed up CPU computations.
1

```

> The TensorFlow library wasn't compiled to use SSE4.1 instructions, but these are available on your machine and could speed up CPU computations.

What was that?


What are SSE4.1, SSE4.2, AVX, AVX2, FMA?
---
These are [vector processor][R2], meaning that the CPU implements an instruction set that can operate on 1-D arrays of data (vectors), compared to `scalar processor`, whose instruction set can only operate on signle data items.

This is also a concept of `SIMD (Single Instuction, Multiple Data)`. Common examples are Intel X86's MMX, SSE, and AVX instructions.

Vector processing techniques also operate in video-game console hardware and in graphics accelerators. In 2000, IBM, Toshiba and Sony collaborated to create the Cell processor, consisting of one scalar processor and eight SIMD processors, which found use in the Sony PlayStation 3 among other applications.

Using vector instructions is faster for many tasks; machine learning is such a task.


Enable the Option in your Bazel Build Command
---

Quoting doc from [Tensorflow Download and Setup][R3]

> To be compatible with as wide a range of machines as possible, TensorFlow defaults to only using SSE4 SIMD instructions. Most modern computers support more advanced instructions. So if you're building a binary that you'll only be running on your own machine, you can enable these by using `-march=native` for optimization options when running `configure`. Then you can build your optimized binaries with the following command:

```
$ bazel build --config opt //tensorflow/tools/pip_package:build_pip_package
```

> If you are distributing a binary but know the capabilities of the machines you'll be running on, you can manually choose the right instructions with something like `-march=avx`. You may also want to enable multiple features using several arguments, for example `-mavx2,-mfma`.

> If you run a binary built using SIMD instructions on a machine that doesn't support them, you'll see an illegal instruction error when that code is executed.


### Enable over Google Datalab

As what you have done is to pull a docker image built by the Datalab team, you may ask them to enable AVX2/FMA capable build option when building the image. But I think they might not be willing to do this as this is a bit unnecessary to them.


How Much SIMD Improves TensorFlow?
---

Quoting from [Yaroslav Bulatov][R4]

> On Xeon E5 v3 that gives me 3x improvement in 8k matmul CPU speed compared to the official release (0.35 -> 1.05 T ops/sec) with below build command:

```
bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-mfpmath=both --config=cuda -k //tensorflow/tools/pip_package:build_pip_package
```

Quoting from

> SSE and AVX allow you to load up 4, 8, or some multiple of registers with data and then run the same operation on them at the exact same time. They also provide fast approximations of expensive operations like inverse sqrts, etc. Theoretical speed increase is 4-8x, someone said they got 3x, which is pretty good.

Basically it seems that at least 3X you might get to improve over the TensorFlow.


Read More
---
- [Feature request: Please provide AVX2/FMA capable builds][R6]


Reference
---
- [How to compile Tensorflow with SSE4.2 and AVX instructions?][R1]
- [Vector processor][R2]
- [Tensorflow Download and Setup][R3]
- [Does activating SSE and AVX “really” improve the computational speed of tensorFlow?][R5]


[R1]: http://stackoverflow.com/questions/41293077/how-to-compile-tensorflow-with-sse4-2-and-avx-instructions
[R2]: https://en.wikipedia.org/wiki/Vector_processor
[R3]: https://www.tensorflow.org/versions/master/get_started/os_setup
[R4]: http://stackoverflow.com/users/419116/yaroslav-bulatov
[R5]: http://stackoverflow.com/questions/42311250/does-activating-sse-and-avx-really-improve-the-computational-speed-of-tensorfl
[R6]: https://github.com/tensorflow/tensorflow/issues/7257#issuecomment-277456370
