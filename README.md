
# 1. Introduction
For test classloader, when there is a class with same canonical name be load twice in jvm,  
how it will work.
# 2. How to do ?
```
mvn clean compile package
cd target
cp ~/.m2/repository/richard/test/richard.test.classloader1/1.0/richard.test.classloader1-1.0.jar ./
cp ~/.m2/repository/richard/test/richard.test.classloader2/1.0/richard.test.classloader2-1.0.jar ./
java -cp ./richard.test.classloader.main-1.0.jar:./richard.test.classloader1-1.0.jar:./richard.test.classloader2-1.0.jar richard.test.classloader.Bootstrap
java -cp ./richard.test.classloader.main-1.0.jar:./richard.test.classloader2-1.0.jar:./richard.test.classloader1-1.0.jar richard.test.classloader.Bootstrap

```
# 3. Result
```java  
RicharddeMac-mini:target richard$ java -cp ./richard.test.classloader.main-1.0.jar:./richard.test.classloader2-1.0.jar:./richard.test.classloader1-1.0.jar richard.test.classloader.Bootstrap
This is me from richard.test.classloader2!
RicharddeMac-mini:target richard$ 
RicharddeMac-mini:target richard$ java -cp ./richard.test.classloader.main-1.0.jar:./richard.test.classloader1-1.0.jar:./richard.test.classloader2-1.0.jar richard.test.classloader.Bootstrap
This is me from richard.test.classloader1!
RicharddeMac-mini:target richard$
```
# 4. Conclusion    
通过控制 classpath 中 jar 包的出现顺序，来实现对不同 jar 包中相同类名的加载的控制