# Caffe instalation


Caffe is a deep learning framework made with expression, speed, and modularity in mind. It is developed by the Berkeley Vision and Learning Center (BVLC) and by community contributors. Yangqing Jia created the project during his PhD at UC Berkeley. Caffe is released under the [BSD 2-Clause license](https://github.com/BVLC/caffe/blob/master/LICENSE).

## 1. General dependencies
```bash
> sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
> sudo apt-get install --no-install-recommends libboost-all-dev
> sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```
## 2. Download caffe

Create a directory where you would like to install caffe. For all future reference, this will be called the <caffe-home>
From inside the **<caffe-home>** directory, execute the following commands :
```bash
> mkdir Caffe_Folder; cd Caffe_Folder
> git clone https://github.com/BVLC/caffe.git
> cd caffe
> cp Makefile.config.example Makefile.config
```
## 3. Install Open-BLAS


BLAS, Low levels routines for performing common linear algebra operations such as vector addition, scalar multiplication, dot products, linear combinations and matrix multiplications. They are de facto standard low-level routines for linear algebra libraries. These routines have binding for both C and Fortran.
```bash
> sudo apt-get install libopenblas-dev

> cd src
> git clone https://github.com/xianyi/OpenBLAS
> cd OpenBLAS
> make FC=gfortran
> sudo make PREFIX=/opt/openblas install
```
Finally, you have to let your system know about these new libraries. 
Add a file to /etc/ld.so.conf.d/ called openblas.conf, containing the path to your new libraries (/opt/openblas/lib):

```bash
> sudo echo "/usr/local/lib" > /etc/ld.so.conf.d/openblas.conf
> sudo ldconfig.
```


## 4. Install OpenCV


OpenCV has C++, C, Python and Java interfaces and supports Windows, Linux, Mac OS, iOS and Android. OpenCV was designed for computational efficiency and with a strong focus on real-time applications. Written in optimized C/C++, the library can take advantage of multi-core processing

```bash
[compiler] > sudo apt-get install build-essential
[required] > sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
[optional] > sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

[compiler] > sudo apt-get install build-essential
[required] > sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
[optional] > sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

> cd ..
```

Download latest release from: [http://opencv.org/downloads.html]

```bash
> cd opencv
> mkdir release
> cd release
> cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
> make
> sudo make install
> sudo echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf
> sudo ldconfig
```
**If** after installing it, got this error: *libdc1394 error: Failed to initialize libdc1394*, run
```bash
> sudo ln /dev/null /dev/raw1394
```

At this point you should be able to *import cv2* when run `python` in the terminal, with no errors. 

## 5. Install Boost

Boost provides free peer-reviewed portable C++ source libraries.

*It was previoulsy installed*


## 6. Setting caffe

In the ~/caffe folder, edit *Makefile.config*. Here it is importante set options since we do not want to use GPU (*We do not have one :(*). Uncomment the line **`CPU_ONLY := 1`**. It is most probably at line 8. As we are using *OpenBLAS*, it requires to modify line 38, and put **`BLAS := open`**. Be aware of python path and dir, also matlab path if needed.
```bash
> make all
> make test
> make runtest
> make pycaffe 
> make distribute
```
If matlab-caffe is needed, run `make matcaffe` instead of `make pycaffe`


### Note

**If** got this error when run *make all*: 
```bash
> CXX/LD -o .build_release/tools/upgrade_net_proto_binary.bin
> .build_release/lib/libcaffe.so: referencia a `cv::imread(cv::String const&, int)'
```
Add these commands to *Makefile* at line 174 LIBRARIES += glog...
```bash
> opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs
```
Then:
> `make clean`

Try again. 


## 7. Add PYTHON path

```bash
> echo export PYTHONPATH=~/path/to/caffe/python:$PYTHONPATH >> ~/.bashrc
```
At this point you should be able to *import caffe*, when run `python` in the terminal, with no errors. 


### Note
**If** got this error after `import caffe`: *ImportError: No module named skimage.io*

Try next:

* ``` sudo apt-get install python-pip ```
* [optional] Download Anaconda from [https://www.continuum.io/downloads]
* [Optional] Locate Anaconda and run `bash Anaconda-2.3.0-Linux-x86_64.sh` (Or any version released at this moment)
* ``` sudo apt-get install libatlas-base-dev gfortran ```
* ``` sudo pip install scipy (Takes a couple of minutes) ```
* ``` sudo pip install scikit-image ```
* ``` [Optional] sudo pip install pandas ```


**Now you should be able to import caffe**


## 8. Install ipython and notebook

###   8.1. Download the GoogLeNet model and clone the deepdream GitHub repository.

```bash
> cd ~/Caffe_Folder
> wget http://dl.caffe.berkeleyvision.org/bvlc_googlenet.caffemodel
> mv bvlc_googlenet.caffemodel caffe/models/bvlc_googlenet
> git clone https://github.com/google/deepdream.git
```

###   8.2. Install the dependencies

```bash
> sudo pip install protobuf
> sudo apt-get install ipython
> sudo apt-get install ipython-notebook
> sudo pip install jupyter
```


###   8.3. Run the IPython Notebook

Now, inside *deepdream* folder run on terminal:
**`ipython notebook`**

then click on `dream.ipynb` and you will see an example. 
