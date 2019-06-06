
# 1. Introduction
For test classloader, when there is a class with same canonical name be load twice in jvm,  
how it will work.
# 2. How to do ?
```
mvn clean compile package
java -cp target/richard.test.classloader.main-1.0.jar:~/.m2/repository/richard/test/richard.test.classloader1/1.0/richard.test.classloader1-1.0.jar:~/.m2/repository/richard/test/richard.test.classloader2/1.0/richard.test.classloader2-1.0.jar richard.test.classloader.Bootstrap
```