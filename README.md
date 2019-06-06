
# 1. Introduction
For test classloader, when there is a class with same canonical name be load twice in jvm,  
how it will work.
# 2. How to do ?
## 2.1 pom dependency
```java  
git@111.204.100.90:liuyngchng/richard.test.classloader.git
cd richard.test.classloader
rename <artifactId>richard.test.classloader</artifactId>  as <artifactId>richard.test.classloader1</artifactId>
replace text in src/main/java/richard/test/classloader/TestClass.java "This is me from richard.test.classloader!" with "This is me from richard.test.classloader1!"
mvn clean compile install
cd richard.test.classloader
rename <artifactId>richard.test.classloader</artifactId>  as <artifactId>richard.test.classloader2</artifactId>
replace text in src/main/java/richard/test/classloader/TestClass.java "This is me from richard.test.classloader!" with "This is me from richard.test.classloader2!"
mvn clean compile install
```
## 2.2 package and run
```
git clone git@111.204.100.90:liuyngchng/richard.test.classloader.main.git
cd richard.test.classloader.main
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
