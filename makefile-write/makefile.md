# makefile编写模板 (2017.09.11)
## 编译模式
1. `-g表示为可以调试处理`
```
CC       = gcc
CXX      = g++
CFAGS    = -g  
CXXCFAGS = $(CFAGS)
```
## 编译路径、原文件、目标文件设置
1. `本目录下的lib`
```
LIBPATH = ./lib 
SRC = ./main.cpp
TARGET = ./hikclient
```
2.  `编译这个文件,all依赖于hikclinetxxx`
```
all : hikclinetxxx 
```
## 检查环境
1.  `判断$(TRAGET_DIR)是否为空，若空则推出`
```
check_env:
    @if ["$(TRAGET_DIR)" = ""]; then echo "xxxxx";exit 1;fi
    @if ["$(TAG_VERSION)" = "" ]; then echo "XXXXX"; exit 1; fi
    @if ["$(REGISTRY)"= "" ]; then echo "XXXXX"; exit 1; fi
```
## 执行程序的方式
1. `-L 表示动态库所处的路径`
2. `-l 表示使用的动态链接库`
```
#hikclient:check_env
`执行这个程序依赖于check_env`
hikvisionclient:
	$(CXX) $(OPTI) $(CXXFLAGS) $(SRC) -o $(TARGET) -L$(LIBPATH)  -Wl,-rpath=./lib:./lib/HCNetSDKCom  -lhcnetsdk -lpthread -lhpr
```
##安装的方式:
1. `install`依赖于`check_env`
2. `rm -rf 把所有文件及子目录删掉`
3. `mkdir -p 创建含有子目录的文件`
4. `cp -r 复制含有子目录的文件到某个文件中`
```
install:check_env
	rm -rf $(TARGET_DIR)/xxx/overspeed
	mkdir -p $(TARGET_DIR)/xxx/overspeed
	cp -r overspeed hikvisionclient lib $(TARGET_DIR)/xxx/overspeed/
```
## 清除目标文件
1. `.PHONY的作用就是用clean文件，即使存在clean这个名称的文件，也会把其删掉`
```
.PHONY: clean
```
2. `删除目标文件`
```
clean:
	rm -f $(TARGET)
```