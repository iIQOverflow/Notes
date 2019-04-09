# virtualenv
## 安装：
sudo apt-get install virtualenv

## 使用创建虚拟环境：
virtual ENV

bin/  可执行文件

lib/python3.x/site-packages 继承/usr/lib/python3.x/site-packages下的所有库

## 继承系统python包文件：
virtualenv --system-site-packages ENV

## 使用指定版本的python
virtualenv ENV --python=python2.7

## 激活：
$ source /path/to/ENV/bin/activate
## 退出：
deactivate