# 影像輸出
 
 燒錄.img檔
 
2g:https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-2gb-devkit#write
4g:https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write

根據教程完成燒錄

完成nano的前置設定

打開終端機
輸入:
```ccs
sudo apt update -y
sudo apt upgrade -y
```
(全Yes)
```ccs
sudo apt install cheese
cheese
#webcam
```

接下來要載python版本(可載可不載)
```ccs
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:deadsnakes/ppa
```

Press [ENTER] to continue or Ctrl-c to cancel adding it.  (按 Enter 鍵)

```ccs
sudo apt update
#這裡我都是默認python3的版本python3.6所以看你有沒有需要裝
sudo apt install python3.10 -y
python3.10 --version
sudo apt install python3-pip
```
載入pip
```ccs
update-alternatives --list python
update-alternatives --install /usr/bin/python python /usr/bin/python3.6 1
update-alternatives --install /usr/bin/python python /usr/bin/python3.10 2
sudo update-alternatives --config python
```

```ccs
sudo apt install vim
```
載入
```ccs
sudo vim ~/.bashrc
```
在最後加入這兩行

```ccs
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```
```ccs
source ~/.bashrc
nvcc --version
dpkg -l | grep cuda
```

我是參考這篇去做的:https://chiachun0818.medium.com/jetson-nano-%E9%87%8D%E6%96%B0%E7%B7%A8%E8%AD%AF-opencv-cuda-a65daf5988ab

那我這裡還是附上指令
```ccs
sudo -H pip install -U jetson-stats
sudo reboot
```
重新開機

```ccs
 jtop
 jetson_release
 ```
 
 ```ccs
 sudo jetson_swap -d ~ -s 4 -a
 ```
 swap建置
 
 ```ccs
 nvcc -V 
 ```
 cuda檢查
 
 ```ccs
 # reveal the CUDA location
 sudo sh -c "echo '/usr/local/cuda/lib64' >> /etc/ld.so.conf.d/nvidia-tegra.conf"
 sudo ldconfig -y
 # third-party libraries
 sudo apt-get install build-essential cmake git unzip pkg-config -y
 sudo apt-get install libjpeg-dev libpng-dev libtiff-dev -y
 sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev -y
 sudo apt-get install libgtk2.0-dev libcanberra-gtk* -y
 sudo apt-get install python3-dev python3-numpy python3-pip -y
 sudo apt-get install libxvidcore-dev libx264-dev libgtk-3-dev -y
 sudo apt-get install libtbb2 libtbb-dev libdc1394–22-dev -y
 sudo apt-get install gstreamer1.0-tools libv4l-dev v4l-utils -y
 sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev -y
 sudo apt-get install libavresample-dev libvorbis-dev libxine2-dev -y
 sudo apt-get install libfaac-dev libmp3lame-dev libtheora-dev -y
 sudo apt-get install libopencore-amrnb-dev libopencore-amrwb-dev -y
 sudo apt-get install libopenblas-dev libatlas-base-dev libblas-dev -y
 sudo apt-get install liblapack-dev libeigen3-dev gfortran -y
 sudo apt-get install libhdf5-dev protobuf-compiler -y
 sudo apt-get install libprotobuf-dev libgoogle-glog-dev libgflags-dev -y
```

安裝相依套件

開始裝opencv

```ccs
 cd ~
 wget -O opencv.zip https://github.com/opencv/opencv/archive/4.5.2.zip
 wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.5.2.zip
 unzip opencv.zip && unzip opencv_contrib.zip
 mv opencv-4.5.2 opencv
 mv opencv_contrib-4.5.2 opencv_contrib
 # clean up the zip files
 rm opencv.zip && rm opencv_contrib.zip
 cd ~/opencv
 mkdir build && cd build
```

```ccs
 cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
-D EIGEN_INCLUDE_PATH=/usr/include/eigen3 \
-D WITH_OPENCL=OFF \
-D WITH_CUDA=ON \
-D CUDA_ARCH_BIN=5.3 \
-D CUDA_ARCH_PTX=”” \
-D WITH_CUDNN=ON \
-D WITH_CUBLAS=ON \
-D ENABLE_FAST_MATH=ON \
-D CUDA_FAST_MATH=ON \
-D OPENCV_DNN_CUDA=ON \
-D ENABLE_NEON=ON \
-D WITH_QT=OFF \
-D WITH_OPENMP=ON \
-D WITH_OPENGL=ON \
-D BUILD_TIFF=ON \
-D WITH_FFMPEG=ON \
-D WITH_GSTREAMER=ON \
-D WITH_TBB=ON \
-D BUILD_TBB=ON \
-D BUILD_TESTS=OFF \
-D WITH_EIGEN=ON \
-D WITH_V4L=ON \
-D WITH_LIBV4L=ON \
-D OPENCV_ENABLE_NONFREE=ON \
-D INSTALL_C_EXAMPLES=OFF \
-D INSTALL_PYTHON_EXAMPLES=OFF \
-D BUILD_opencv_python3=TRUE \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D BUILD_EXAMPLES=OFF ..
```

接下來可能需要一個小時的時間(看人品的時候到了)

```ccs
make -j4
```

```ccs
 sudo rm -r /usr/include/opencv4/opencv2
 sudo make install
 sudo ldconfig
 # cleaning (frees 300 MB)
 make clean
 sudo apt-get update
 sudo rm -rf ~/opencv
 sudo rm -rf ~/opencv_contrib
```

檢查opencv

```ccs
 jetson_release
```
出來cuda後應該要是:YES


