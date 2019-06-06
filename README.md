
# 1. Introduction
For classloader test, when there is a class with same canonical name，   
but with different "artifactId" or "version"  in pom,  
it seems two different jar would be load in jvm,  
but only one class byte code can be used in JVM.  
so,how it will work in the situation?
# 2. How to do ?
## 2.1 pom dependency
```
git@github.com:liuyngchng/richard.test.classloader.git
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
git clone git@github.com:liuyngchng/richard.test.classloader.main.git
cd richard.test.classloader.main
mvn clean compile package
cd target
cp ~/.m2/repository/richard/test/richard.test.classloader1/1.0/richard.test.classloader1-1.0.jar ./
cp ~/.m2/repository/richard/test/richard.test.classloader2/1.0/richard.test.classloader2-1.0.jar ./
java -cp ./richard.test.classloader.main-1.0.jar:./richard.test.classloader1-1.0.jar:./richard.test.classloader2-1.0.jar richard.test.classloader.Bootstrap
java -cp ./richard.test.classloader.main-1.0.jar:./richard.test.classloader2-1.0.jar:./richard.test.classloader1-1.0.jar richard.test.classloader.Bootstrap

```

# 3. Result

```
RicharddeMac-mini:target richard$ java -cp ./richard.test.classloader.main-1.0.jar:./richard.test.classloader2-1.0.jar:./richard.test.classloader1-1.0.jar richard.test.classloader.Bootstrap
This is me from richard.test.classloader2!
RicharddeMac-mini:target richard$
RicharddeMac-mini:target richard$ java -cp ./richard.test.classloader.main-1.0.jar:./richard.test.classloader1-1.0.jar:./richard.test.classloader2-1.0.jar richard.test.classloader.Bootstrap
This is me from richard.test.classloader1!
RicharddeMac-mini:target richard$
```
# 4. Conclusion    

通过控制 classpath 中 jar 包的顺序，来实现对不同 jar 包中相同类名的类的加载的控制
