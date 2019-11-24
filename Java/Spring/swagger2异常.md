# swagger2异常
## 环境
```
swagger2 2.9.2
springfox-swagger-ui 2.9.2
```
## 异常信息
```
java.lang.NumberFormatException: For input string: ""
        at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65) ~[na:1.8.0_221]
        at java.lang.Long.parseLong(Long.java:601) ~[na:1.8.0_221]
        at java.lang.Long.valueOf(Long.java:803) ~[na:1.8.0_221]
        at io.swagger.models.parameters.AbstractSerializableParameter.getExample(AbstractSerializableParameter.java:412) ~[swagger-models-1.5.20.jar:1.5.20]
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_221]
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_221]
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_221]
        at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_221]
        at com.fasterxml.jackson.databind.ser.BeanPropertyWriter.serializeAsField(BeanPropertyWriter.java:688) [jackson-databind-2.9.9.3.jar:2.9.9.3]
        at com.fasterxml.jackson.databind.ser.std.BeanSerializerBase.serializeFields(BeanSerializerBase.java:719) [jackson-databind-2.9.9.3.jar:2.9.9.3]
```
## 解决方法
`pom.xml`中修改依赖
```xml
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
	<version>2.9.2</version>
	<exclusions>
		<exclusion>
			<groupId>io.swagger</groupId>
			<artifactId>swagger-annotations</artifactId>
		</exclusion>
		<exclusion>
			<groupId>io.swagger</groupId>
			<artifactId>swagger-models</artifactId>
		</exclusion>
	</exclusions>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.9.2</version>
</dependency>
<dependency>
	<groupId>io.swagger</groupId>
	<artifactId>swagger-annotations</artifactId>
	<version>1.5.21</version>
</dependency>
<dependency>
	<groupId>io.swagger</groupId>
	<artifactId>swagger-models</artifactId>
	<version>1.5.21</version>
</dependency>
```
## 参考
[GitHub](https://github.com/springfox/springfox/issues/2265)