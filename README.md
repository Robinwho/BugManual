# BugBook
Record the major bugs encountered in the projects. So easy to find & summary in the future.
##BUG 1
Expected behavior

Follow the tips by the link:
https://github.com/Robinwho/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md#optional-setup-gpu-for-mac

After Docker is installed, launch a Docker container with the TensorFlow binary image as follows.
$ docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow
Actual behavior

$ docker run -it -p 8888:8888 gcr.io/tensorflow/tensorflow
Information

GET THE ERRORS:

Unable to find image 'gcr.io/tensorflow/tensorflow:latest' locally
docker: Error response from daemon: Get https://gcr.io/v1/_ping: dial tcp 64.233.189.82:443: i/o timeout.
See 'docker run --help'.
Is this the problem of proxy? Coz when I run:

ping google.com
Then printing the infos:

PING google.com (172.217.24.14): 56 data bytes
Request timeout for icmp_seq 0
Request timeout for icmp_seq 1
Request timeout for icmp_seq 2
Request timeout for icmp_seq 3

Having tried many ways, but doesn't work. 

Finally with the help of these two [](links(http://blog.csdn.net/chenming_hnu/article/details/54600270) & ), the problem is fixed!
The correct command:
>sudo docker run -it -p 8888:8888 tensorflow/tensorflow

###Detail
https://github.com/docker/for-mac/issues/1145
