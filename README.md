# Tensorflow-on-jetson-tk1
Install Nano text editor since I hate vim
$sudo apt-get update
$sudo apt-get install nano

Install g++ 
$sudo apt-get install g++

Install Java
Install java jdk152 because newer java versions do not support.
download  jdk-8u152-linux-arm32-vfp-hflt.tar.gz
create jvm directory to organize all JDK versions  
$sudo mkdir /usr/lib/jvm
extract 
$tar zxvf jdk-8u152-linux-arm32-vfp-hflt.tar.gz
$sudo mv jdk1.8.0_152 /usr/lib/jvm

Add JDK 
$sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_152/bin/java" 1
 $sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0_152/bin/javac" 1

if you installed another version of java before:
$sudo update-alternatives --config java
You will see a list of JDK versions, choose the number of jdk152
javac too
$sudo update-alternatives --config javac

$sudo nano /etc/environment
Add
JAVA_HOME=/usr/lib/jvm/jdk1.8.0_152
export JAVA_HOME

reload system path
$. /etc/environment


Finally, check for the version
$java -version
you should see java version "1.8.0_152" in the output
$javac -version
javac 1.8.0_152


Install dependencies of protobuf and Bazel
$sudo apt-get install git zip unzip autoconf automake libtool curl zlib1g-dev  

Create swap disk since Jetson TK1 has only 2Gb memory

Install Bazel
$mkdir bazel
$cd bazel
$wget https://github.com/bazelbuild/bazel/releases/download/0.4.4/bazel-0.4.4-dist.zip
$ unzip bazel-0.4.4-dist.zip 

Increase maximum heap space or you will get "OutOfMemoryError" when compiling bazel
sudo nano -c scripts/bootstrap/compile.sh 
In line 110, add -J-Xms256m -J-Xmx512m so it looks like 
" run "${JAVAC}" -J-Xms256m -J-Xmx512m -classpath "${classpath}" -sourcepath "${sourcepath}"

build 
$./compile.sh

   Copy bazel to /usr/local/bin.

$sudo cp output/bazel /usr/local/bin/bazel

make sure bazel is working
$bazel
[bazel release 0.4.4- (@non-git)]...

 *Install "CUDA 6.5"  
 $ wget http://developer.download.nvidia.com/compute/cuda/6_5/rel/installers/cuda-repo-l4t-r21.2-6-5-prod_6.5-34_armhf.deb  
$ sudo dpkg -i cuda-repo-l4t-r21.2-6-5-prod_6.5-34_armhf.deb  
 $ sudo apt-get update
 $ sudo apt-get install cuda-toolkit-6-5  

Download cudnn v2 from nvidia 
  tar zxvf cudnn-6.5-linux-ARMv7-v2.tgz 
 cd cudnn-6.5-linux-ARMv7-v2
sudo cp cudnn.h /usr/local/cuda-6.5/include/
sudo cp libcudnn* /usr/local/cuda-6.5/lib/  
cd


 *Install "CUDA 7.0"
 $ wget http://developer.download.nvidia.com/embedded/L4T/r24_Release_v1.0/CUDA/cuda-repo-l4t-7-0-local_7.0-76_armhf.deb  
 $ sudo dpkg -i cuda-repo-l4t-7-0-local_7.0-76_armhf.deb  
 $ sudo apt-get update
 $ sudo apt-get install cuda-toolkit-7-0  
 $ cd /usr/local  
 $ sudo rm cuda  
 $ sudo ln -s cuda-6.5/ cuda link to other directory
