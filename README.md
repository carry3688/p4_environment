# p4_environment
## 仓库
关于P4学习的进阶仓库，只需要命令行
```bash
git clone https://github.com/nsg-ethz/p4-learning.git
```
官方仓库要求安装以下依赖
- [PI](https://github.com/p4lang/PI)
- [*Behavioral Model* (BMv2)](https://github.com/p4lang/behavioral-model)
- [P4C](https://github.com/p4lang/p4c)
- [*Mininet*](http://mininet.org/)
- [*FRRouting*](https://frrouting.org/)
- [*P4-Utils*](https://github.com/nsg-ethz/p4-utils)

下面根据我自己的安装过程，写一下正确在Ubuntu20.04配置基础环境的过程。
## 步骤

### 安装基础依赖项
```bash
sudo apt-get install mininet g++ git automake libtool libgc-dev bison flex libfl-dev libgmp-dev libboost-dev libboost-iostreams-dev pkg-config python python3-scapy curl wget tcpdump cmake
sudo apt install python3-pip
sudo pip install ipaddr
```
### 安装Protobuf
```bash
wget https://github.com/protocolbuffers/protobuf/archive/v3.2.0.zip
unzip v3.2.0.zip
cd protobuf-3.2.0
./autogen.sh
./configure
make
make check
sudo make install
sudo ldconfig
protoc --version
```
注意事项：
1. make的时候要等待一段时间，可以利用这段时间先执行下面的内容。
1. make check的结果应当是7个测试全部pass，如果没有全过，请继续查找问题，绝对不能下一步。
2. 当最后打印版本输出`libprotoc 3.2.0`的时候，代表安装成功。

### 安装P4c
```bash
git clone --recursive https://github.com/p4lang/p4c.git
（如果太慢，可以使用国内镜像，即url改成：github.com.cnpmjs.org，后续步骤同）
cd p4c
./bootstrap.sh
cd build
cmake ..
make -j4
make check -j4
sudo make install
p4c --version
```
注意事项：
这里make的时间巨长，可以利用这一段时间先执行下面的内容。完成了make之后的make check的时间也是非常长，这里貌似要pip安装ply，具体的报错可以在`p4c/build/Testing/Temporary/LastTest.log`日志里面查看。

### 安装PI
```bash
git clone https://github.com/p4lang/PI.git
cd PI
git submodule update --init --recursive
./autogen.sh
./configure
make
sudo make install
sudo ldconfig
```
PI的安装相比来说还是很快的。

### 安装bmv2
bmv2是可以兼容p4的软件交换机环境。
```bash
git clone https://github.com/p4lang/behavioral-model.git
cd behavioral-model
./install_deps.sh
./autogen.sh
./configure
make
sudo make install -j4
make check -j4

```

### 完成
至此，你已经完成了p4环境的配置。