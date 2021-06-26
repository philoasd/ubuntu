# ubuntu
使用Ubuntu20.04系统配置opencv4.5.3以及opencv_contrib4.5.3环境
# CSDN链接
https://blog.csdn.net/Philloasd/article/details/118251160

# 安装依赖
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

# 下载并解压源码
wget -O opencv.zip https://github.com/opencv/opencv/archive/master.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/master.zip
解压 opencv.zip
解压 opencv_contrib.zip

# 创建构建目录并切换到该目录
mkdir -p build && cd build

# 配置
cmake -DOPENCV_EXTRA_MODULES_PATH=../opencv_contrib-master/modules ../opencv-master -DOPENCV_GENERATE_PKGCONFIG=YES .. -DCUDA_ARCH_BIN='7.5' -DOPENCV_ENABLE_NONFREE=ON

# 建造
make -j8(8为CPU核心数，通过nproc查看)

# 构建完毕后
sudo make install

# 再次等待一段时候后，执行:
sudo sh -c 'echo "/usr/local/lib" >> /etc/ld.so.conf.d/opencv.conf'
sudo ldconfig

# 执行
sudo gedit ~/.bashrc
#在文件最后添加
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig
export PKG_CONFIG_PATH

# 退出后，执行
source ~/.bashrc
# 判断路径时候添加成功，返回:/usr/local/lib/pkgconfig即可
echo $PKG_CONFIG_PATH

# opencv项目CmakeList.txt
cmake_minimum_required(VERSION 3.19)
project(opencvTest)
set(CMAKE_CXX_STANDARD 14)

#寻找OpenCV库
find_package(OpenCV)
#添加头文件
include_directories(${OpenCV_INCLUDE_DIRS})

add_executable(opencvTest opencvTest.cpp)
#链接OpenCV库
target_link_libraries(opencvTest ${OpenCV_LIBS})
