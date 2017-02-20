---
layout: post
title:  "Upgrade the Installed TensorFlow"
date:   2017-02-19 00:00:00 +0900
categories: jekyll update
type: Tensorflow
excerpt_separator: <!--more-->
---
<!--more-->

Steps
---

### Get the current version of the installed TensorFlow

Import the TensorFlow module, and then print out `tf.__version__`

```
import tensorflow as tf
print(tf.__version__)
0.7.1
```
or you can just have two ways like below to type in the terminal.

```
$ python -c 'import tensorflow as tf; print(tf.__version__)'
0.7.1

// or

$ pip list | grep tensorflow
tensorflow (0.7.1)
```

### Update the TensorFlow

If you are upgrading from a previous installation of TensorFlow < 0.7.1, you should uninstall the previous TensorFlow and protobuf using `pip uninstall` first to make sure you get a clean installation of the updated protobuf dependency.

Uninstall the TensorFlow on your system, and check out [Download and Setup][R2] to reinstall again.

If you are using pip install, go check the available version over `https://storage.googleapis.com/tensorflow`, search keywords with `mac/cpu/tensorflow` to see the availabilities.

Then, set the path for download and execute in sudo.

```
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.0.0-py2-none-any.whl

$ sudo pip install --upgrade $TF_BINARY_URL
```

### Update the TensorFlow over Google Datalab?

The post over stackoverflow said:







### Issues I Met




```
AttributeErrorTraceback (most recent call last)
<ipython-input-15-b86dbbf94704> in <module>()
    101
    102 if __name__ == '__main__':
--> 103   main()

<ipython-input-15-b86dbbf94704> in main()
     79     actual = sess.run(y, feed_dict = {y: chuatu.test.labels})
     80
---> 81     tp = tf.cast(tf.count_nonzero(predicted * actual), tf.float)
     82     print(type(tp))
     83     tn = tf.count_nonzero((predicted - 1) * (actual - 1))

AttributeError: 'module' object has no attribute 'float'

```



Reference
---
- [How to find which version of TensorFlow is installed in my system?][R1]
- [TensorFlow Download and Setup][R2]
- [tensorflow upgrade failed on google datalab][R3]

[R1]: http://stackoverflow.com/questions/38549253/how-to-find-which-version-of-tensorflow-is-installed-in-my-system
[R2]: https://www.tensorflow.org/versions/r0.10/get_started/os_setup#download-and-setup
[R3]: http://stackoverflow.com/questions/37464668/tensorflow-upgrade-failed-on-google-datalab
