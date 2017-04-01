---
layout: post
title:  "Upgrade the Installed TensorFlow"
date:   2017-02-19 00:00:00 +0900
categories: jekyll update
type: Tensorflow
excerpt_separator: <!--more-->
---
<!--more-->

Get the current version of the installed TensorFlow
---

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

Update the TensorFlow
---

If you are upgrading from a previous installation of TensorFlow < 0.7.1, you should uninstall the previous TensorFlow and protobuf using `pip uninstall` first to make sure you get a clean installation of the updated protobuf dependency.

Uninstall the TensorFlow on your system, and check out [Download and Setup][R2] to reinstall again.

If you are using pip install, go check the available version over `https://storage.googleapis.com/tensorflow`, search keywords with `mac/cpu/tensorflow` to see the availabilities.

Then, set the path for download and execute in sudo.

```
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.0.0-py2-none-any.whl

$ sudo pip install --upgrade $TF_BINARY_URL
```

Update the TensorFlow over Google Datalab?
---

### Update the docker image built by Datalab

List all the docker:

```
$ docker ps -a
CONTAINER ID        IMAGE                                         COMMAND                  CREATED             STATUS                   PORTS                      NAMES
4e8c6ecdbe98        gcr.io/cloud-datalab/datalab:local            "/datalab/run.sh"        2 weeks ago         Up 21 minutes            127.0.0.1:4041->8080/tcp   datalab
```

As we have pulled the docker image built by Datalab team, there could be an update for the docker image.

Paster this path `gcr.io/cloud-datalab/datalab` into your browser, the browser will lead you to `https://console.cloud.google.com/kubernetes/images/tags/datalab?location=GLOBAL&project=cloud-datalab`.

Then you see a lot of versions of docker image over their Container Registry.

![Imgur](http://i.imgur.com/13FYH1f.png)

Try to pull the version of `local-20170218`, and create a new docker to launch the image.

```
$ docker ps -a
CONTAINER ID        IMAGE                                         COMMAND                  CREATED             STATUS                   PORTS                      NAMES
d072d60618c8        gcr.io/cloud-datalab/datalab:local-20170218   "/datalab/run.sh"        21 hours ago        Up 21 hours              127.0.0.1:2021->8080/tcp   dalatab-20170218
4e8c6ecdbe98        gcr.io/cloud-datalab/datalab:local            "/datalab/run.sh"        2 weeks ago         Up 21 minutes            127.0.0.1:4041->8080/tcp   datalab
```

Open the notebook over the browser, then try to print out the version of TensorFlow. You will found the version get to upgrade!


### Use %%bash to update directly

The post over stackoverflow said:

> If you want to temporarily install a newer version into your existing environment for testing purposes **(although this isn't recommended)** , then you could try installing tensorflow with the no dependencies option `(--no-deps)` in order to reduce the chance of breaking the working datalab environment.

```
%%bash
wget https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl && pip install --ignore-installed --no-deps tensorflow-0.8.0-cp27-none-linux_x86_64.whl
```

Go the check over `https://storage.googleapis.com/tensorflow/`, you see a lot of versions by filtering with the keyword `linux/cpu/tensorflow-0.12.0-`. However, I am not aware of the difference between `cp27-none-linux_x86_64.whl`, `cp34-cp34m-linux_x86_64.whl`, `0-cp35-cp35m-linux_x86_64.whl`, and so on.

Just try to run with:

```
%%bash
wget https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.12.0-cp27-none-linux_x86_64.whl && pip install --ignore-installed --no-deps tensorflow-0.12.0-cp27-none-linux_x86_64.whl
```

After running the above command, you should see tensorflow is at version 0.12.0.

![Imgur](http://i.imgur.com/Bm7HU68.png)


Reference
---
- [How to find which version of TensorFlow is installed in my system?][R1]
- [TensorFlow Download and Setup][R2]
- [tensorflow upgrade failed on google datalab][R3]

[R1]: http://stackoverflow.com/questions/38549253/how-to-find-which-version-of-tensorflow-is-installed-in-my-system
[R2]: https://www.tensorflow.org/versions/r0.10/get_started/os_setup#download-and-setup
[R3]: http://stackoverflow.com/questions/37464668/tensorflow-upgrade-failed-on-google-datalab
