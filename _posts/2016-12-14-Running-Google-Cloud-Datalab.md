---
layout: post
title:  "Running Google Cloud Datalab"
date:   2016-12-14 00:00:00 +0900
categories: jekyll update
type: Tensorflow
excerpt_separator: <!--more-->
---
<!--more-->


Quickstarts
---

You have 3 approaches to run the Cloud Datalab.

1. Run Cloud Datalab locally
2. Run Cloud Datalab kernels on Google Cloud Platform
3. Run Cloud Datalab notebooks on Google Cloud Platform


### 1. Run Cloud Datalab locally

#1. [Install Docker][R2]

Make sure you have Docker version for 1.12.0 or later.

Basic Docker Command:

```
$ docker-machine version
docker-machine version 0.8.2, build e18a919
```

```
$ docker-machine ls
NAME             ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
default          -        virtualbox   Saved                                         Unknown
dockerdefault2   -        virtualbox   Running   tcp://192.168.99.100:2376           v1.12.3
```

```
$ docker version
Client:
 Version:      1.12.3
 API version:  1.24
 Go version:   go1.6.3
 Git commit:   6b644ec
 Built:        Wed Oct 26 23:26:11 2016
 OS/Arch:      darwin/amd64

Server:
 Version:      1.12.3
 API version:  1.24
 Go version:   go1.6.3
 Git commit:   6b644ec
 Built:        Wed Oct 26 23:26:11 2016
 OS/Arch:      linux/amd64
```

// run a container

```
$ docker run -d -p 80:80 --name webserver nginx
```
If the image is not found locally, Docker will pull it from Docker Hub.
In a web browser, go to http://localhost/ to bring up the home page. (Since you specified the default HTTP port, it isn’t necessary to append :80 at the end of the URL.)

![Imgur](http://i.imgur.com/eAPsohN.png)

// show containers

```
$ docker ps
CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS              PORTS                         NAMES
df6465d23a74        gcr.io/cloud-datalab/datalab:local   "/datalab/run.sh"        11 minutes ago      Up 11 minutes       0.0.0.0:8081->8080/tcp        gigantic_borg
7e3080b10b88        nginx                                "nginx -g 'daemon off"   41 minutes ago      Up 32 minutes       0.0.0.0:80->80/tcp, 443/tcp   webserver
```

// show images

```
$ docker images
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
nginx                          latest              abf312888d13        2 weeks ago         181.5 MB
gcr.io/cloud-datalab/datalab   local               a496e8be8e5e        4 weeks ago         1.653 GB
```

#2. Choose a Google Cloud Platform Project

```
$ gcloud projects list
PROJECT_ID                      NAME                            PROJECT_NUMBER
planet-multilingual             planet-multilingual             809185679101
planet-productsinfo-data        Planet-ProductsInfo-Data        781280302721
planet-productsinfo-data-stage  Planet-ProductsInfo-Data-Stage  139990011683
```

To install the `gcloud` command, install the [Google Cloud SDK][R3].

#3. Install the "datalab" Docker container on your computer

```
$ docker run -d -it -p "127.0.0.1:8081:8080" -v "${HOME}:/content" \
  -e "PROJECT_ID=<PROJECT_ID>" \
  gcr.io/cloud-datalab/datalab:local
```

#4. Open Cloud Datalab in your browser

You will need to accept the Terms of Service at the first time.
![Imgur](http://i.imgur.com/qHmLoGu.png)

![Imgur](http://i.imgur.com/BFPlc8o.png)

When you see below message, you may upgrade the datalab version by below method:

![Imgur](http://i.imgur.com/tppuxC2.png)

What's new is the datalab release info [here][R5].

#5. Upgrade the Datalab Version

// list the current images

```
$ docker images
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
nginx                          latest              abf312888d13        7 weeks ago         181.5 MB
gcr.io/cloud-datalab/datalab   local               a496e8be8e5e        8 weeks ago         1.653 GB
```

// pull the docker image

```
$ docker pull gcr.io/cloud-datalab/datalab:local
local: Pulling from cloud-datalab/datalab
75a822cd7888: Pull complete
a3ed95caeb02: Pull complete
8bc7563039e0: Pull complete
0159943e6d06: Pull complete
Digest: sha256:01056cf8d09bea862f16134d4bc4b1899d13f820e92580e682a2d3a98fb9157f
Status: Downloaded newer image for gcr.io/cloud-datalab/datalab:local
```

// list the current images, you see the old version is kept as `<none>` TAG, and the new version is tagged with `local`.


```
$ docker images
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
gcr.io/cloud-datalab/datalab   local               ca83fac2cd0b        6 days ago          1.769 GB
nginx                          latest              abf312888d13        7 weeks ago         181.5 MB
gcr.io/cloud-datalab/datalab   <none>              a496e8be8e5e        8 weeks ago         1.653 GB
```

You may try to delete the old image, before doing that, remeber to stop the container that are using this old image, and delete that container.

// run the container with the new image

```
$ docker run -d -it -p "127.0.0.1:4041:8080" -v "${HOME}:/content" -e "PROJECT_ID=jpower-ml" --name "datalab" gcr.io/cloud-datalab/datalab:local
```

// see the current containers

```
$ docker ps -a
CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS                   PORTS                      NAMES
7349a74c4da7        gcr.io/cloud-datalab/datalab:local   "/datalab/run.sh"        3 seconds ago       Up 2 seconds             127.0.0.1:4041->8080/tcp   datalab
```

You can go to `http://localhost:4041/` now.


### 2. Run Cloud Datalab on Google Cloud Platform

This approach is installing a local datalab container connected to Google Cloud Platform;
Pros:

- Store and access your notebooks on your local machine, while you:
    - Use the computing and disk resources of a Cloud Platform Virtual Machine to run notebook commands and get results quickly.
    - Use Cloud Platform to access Google Cloud Storage data directly and avoid time-consuming data downloads to your local machine.


#1. [Install Docker][R2]

Make sure you have Docker version for 1.12.0 or later.

#2. Choose a Google Cloud Platform Project

```
$ gcloud projects list
PROJECT_ID                      NAME                            PROJECT_NUMBER
planet-multilingual             planet-multilingual             809185679101
planet-productsinfo-data        Planet-ProductsInfo-Data        781280302721
planet-productsinfo-data-stage  Planet-ProductsInfo-Data-Stage  139990011683
```

To install the `gcloud` command, install the [Google Cloud SDK][R3].

#3. Create a VM on Google Cloud Platform

// Create a `datalab-gateway-network` (insert your `project-id` as shown).

```
$ gcloud compute networks create "datalab-gateway-network" \
  --project "project-id" \
  --description "Network for Datalab kernel gateway"
```

// Allow SSH access to the network (insert your `project-id` as shown).

```
$ gcloud compute firewall-rules create datalab-gateway-network-allow-ssh \
  --project "project-id" \
  --allow tcp:22 \
  --network "datalab-gateway-network" \
  --description "Allow SSH access"
```

// Download the `datalab-gateway.yaml` file that defines how to setup the VM.

```
$ gsutil cp gs://cloud-datalab/gateway.yaml ./data-gateway.yaml
```

// Create the VM.

Insert `instance-name`, your `project-id`, and the `zone` where the VM will be located (see [Choosing a region and zone][R4]). Make sure to select a name for the instance that is unique.

```
$ gcloud compute instances create "instance-name" \
  --project "project-id" \
  --zone "zone" \
  --network "datalab-gateway-network" \
  --image-family "container-vm" \
  --image-project "google-containers" \
  --metadata "google-container-manifest=$(cat datalab-gateway.yaml)" \
  --machine-type "n1-highmem-2" \
  --scopes "cloud-platform"
```

#4. Install the "datalab" Docker container on your computer

```
$ docker run -it -p "127.0.0.1:8081:8080" -v "${HOME}:/content" \
  -e "GATEWAY_VM=project-id/zone/instance-name" \
  gcr.io/cloud-datalab/datalab:local
```

#5. Open Cloud Datalab in your browser

#6. Clean up

To avoid incurring charges to your Google Cloud Platform account for the resources used in this quickstart: **Delete the Google Compute Engine VM when you are finished.**


### 3. Run Cloud Datalab notebooks on Google Cloud Platform

This approach allows you:

- use the computing and disk resources of a Cloud Platform Virtual Machine to run notebook commands and get results quickly.
- use Cloud Platform to access Google Cloud Storage data directly and avoid time-consuming data downloads to your local machine.
- run Cloud Datalab without installing Docker on your local machine (Cloud Datalab notebooks are accessed on a Google Compute Engine VM).


#1. Choose a Google Cloud Platform Project

```
$ gcloud projects list
PROJECT_ID                      NAME                            PROJECT_NUMBER
planet-multilingual             planet-multilingual             809185679101
planet-productsinfo-data        Planet-ProductsInfo-Data        781280302721
planet-productsinfo-data-stage  Planet-ProductsInfo-Data-Stage  139990011683
```

To install the `gcloud` command, install the [Google Cloud SDK][R3].

#2. Create a Compute Engine VM on Google Cloud Platform

// Create a `datalab-gateway-network` (insert your `project-id` as shown).

```
$ gcloud compute networks create "datalab-gateway-network" \
  --project "project-id" \
  --description "Network for Datalab kernel gateway"
```

// Allow SSH access to the network (insert your `project-id` as shown).

```
$ gcloud compute firewall-rules create datalab-gateway-network-allow-ssh \
  --project "project-id" \
  --allow tcp:22 \
  --network "datalab-gateway-network" \
  --description "Allow SSH access"
```

// Download the `datalab-gateway.yaml` file that defines how to setup the VM.

```
$ gsutil cp gs://cloud-datalab/server.yaml ./datalab-server.yaml
```

// Create the VM.

Insert `instance-name`, your `project-id`, and the `zone` where the VM will be located (see [Choosing a region and zone][R4]). Make sure to select a name for the instance that is unique.

```
$ gcloud compute instances create "instance-name" \
  --project "project-id" \
  --zone "zone" \
  --network "datalab-gateway-network" \
  --image-family "container-vm" \
  --image-project "google-containers" \
  --metadata "google-container-manifest=$(cat datalab-gateway.yaml)" \
  --machine-type "n1-highmem-2" \
  --scopes "cloud-platform"
```

#3. Use an SSH tunnel to connect to Cloud Datalab

Run the following command (insert your project-id, zone, and instance-name, as shown).

```
gcloud compute ssh --quiet \
  --project "project-id" \
  --zone "zone" \
  --ssh-flag="-N" \
  --ssh-flag="-L" \
  --ssh-flag="localhost:8081:localhost:8080" \
  "${USER}@instance-name"
```

#4. Open Cloud Datalab in your browser (localhost:8081)

#5. Clean up

To avoid incurring charges to your Google Cloud Platform account for the resources used in this quickstart: **Delete the Google Compute Engine VM when you are finished.**

#6. Next time, we just need to repeat #3. `Use an SSH tunnel to connect to Cloud Datalab` to run datalab over GCP.

```
gcloud compute ssh --quiet \
  --project "project-id" \
  --zone "zone" \
  --ssh-flag="-N" \
  --ssh-flag="-L" \
  --ssh-flag="localhost:8081:localhost:8080" \
  "${USER}@instance-name"
```


Reference
---
- [CLOUD DATALAB][R1]

[R1]: https://cloud.google.com/datalab/
[R2]: https://www.docker.com/products/docker
[R3]: https://cloud.google.com/sdk/downloads
[R4]: https://cloud.google.com/compute/docs/regions-zones/regions-zones#choosing_a_region_and_zone
[R5]: https://github.com/googledatalab/datalab/wiki/Release-Info

