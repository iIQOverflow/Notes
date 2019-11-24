# spring boot 项目开发常用目录结构
在spring boot开发中一些常用的目录划分

## 代码层结构

根目录：net.csdn

1.启动类（CsdnApplication.java）推荐放在根目录net.csdn包下

2.实体类（domain）

    A: net.csdn.domain（jpa项目）

    B: net.csdn.pojo（mybatis项目）

3.数据接口访问层(Dao)

    A: net.csdn.repository（jpa项目）

    B: net.csdn.mapper（mybatis项目）

4.数据服务接口层（Service）推荐：net.csdn.service

5.数据服务实现层（Service Implements）推荐：net.csdn.service.impl

    ——使用idea的同学推荐使用net.csdn.serviceImpl目录

6.前端控制器层（Controller）推荐：net.csdn.controller

7.工具类库（utils）推荐：net.csdn.utils

8.配置类（config）推荐：net.csdn.config

9.数据传输对象（dto）推荐：net.csdn.dto

    ——数据传输对象（Data Transfer Object）用于封装多个实体类（domain）之间的关系，不破坏原有的实体类结构

10.视图包装对象（vo）推荐：net.csdn.vo

    ——视图包装对象（View Object）用于封装客户端请求的数据，防止部分数据泄露（如：管理员ID），保证数据安全，不破坏   原有的实体类结构

## 资源目录结构

根目录：resources

1.项目配置文件：resources/application.yml

2.静态资源目录：resources/static/

    ——用于存放html、css、js、图片等资源

3.视图模板目录：resources/templates/

    ——用于存放jsp、thymeleaf等模板文件

4.mybatis映射文件：resources/mapper/（mybatis项目）

5.mybatis配置文件：resources/mapper/config/（mybatis项目）

可参考开源项目：https://gitee.com/aun/Timo
