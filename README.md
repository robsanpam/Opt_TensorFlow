# TensorFlow-MPI for CPU from sources for Ubuntu 18.04

These instructions are to install CPU-only TensorFlow with MPI support using Python 3.6.5.

Requirements:

    sudo pip3 install git six numpy wheel 
    sudo apt-get install python3-numpy python3-dev python3-pip python3-wheel
    sudo apt-get install pkg-config zip g++ zlib1g-dev unzip

## Downloading TensorFlow

First, clone the TensorFlow repository by issuing:
  
    git clone https://github.com/tensorflow/tensorflow
 
Go to the new subdirectory created _tensorflow_ and select a specific branch to build, for example:
    
    cd tensorflow
    git checkout r1.8
    
## Installing Bazel

Bazel is an open-source build and test tool. We download Bazel by doing:

    wget https://github.com/bazelbuild/bazel/releases/download/0.14.1/bazel-0.14.1-installer-linux-x86_64.sh

Run the Bazel installer as follows:

    chmod +x bazel-0.14.1-installer-linux-x86_64.sh 
    ./bazel-0.14.1-installer-linux-x86_64.sh  --user

Add the following command to your _~/.bashrc_ file:

    export PATH="$PATH:$HOME/bin"
    source /home/robsanpam/.bazel/bin/bazel-complete.bash

## Installing OpenMPI and mpi4py.

Please make sure to follow the instructions on https://github.com/arundasan91/MPI---Message-Passing-Interface/blob/master/MPI4py_Installation.md


## Configuring TensorFlow Installation

Now we can start configuring the installation. We go to the tensorflow directory and run the _configure.py_ with:

    cd tensorflow
    sudo python3 configure.py
    
This script will ask for some paths which will depend on our python installation.

When the script asks about the MPI support, type y

    Do you wish to build TensorFlow with MPI support? [y/N]
    
One of the questions that configure will ask is as follows:

    Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]

TensorFlow recommends accepting the default (-march=native), which will optimize the generated code for your local machine's CPU type.

## Building the Package

Now to build the pip package we need to invoke:

    bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
    
We now run the script that was generated with the previous command by typing:

    bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg

Finally, we use pip3 to install the wheel file created by:

    sudo pip3 install /tmp/tensorflow_pkg/tensorflow-1.8.0-py2-none-any.whl

## Validate Installation

To validate the installation, we can cd to any other folder other than the tensorflow subdirectory from which we invoked the configure command and type:

    python3
    import tensorflow as tf
    hello = tf.constant("Hello, TensorFlow!")
    sess = tf.Session()
    print(sess.run(hello))
    
If the system outputs the following, then we are ready to use Tensorflow:

    Hello, TensorFlow!

