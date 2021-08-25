# Scala
## Install Scala on Windows 10 WDSL 2 Ubuntu 20.4

## Check Java
Open the terminal and check if java is installed

    java -version
    
    openjdk version "1.8.0_292"
    OpenJDK Runtime Environment (build 1.8.0_292-8u292-b10-0ubuntu1~20.04-b10)
    OpenJDK 64-Bit Server VM (build 25.292-b10, mixed mode)

Update all the package

    sudo apt-get update

Install Java JDK if not installed

    sudo apt-get install default-JDK

Install Scala

    sudo apt-get install scala

    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    The following additional packages will be installed:
    libhawtjni-runtime-java libjansi-java libjansi-native-java libjline2-java scala-library scala-parser-combinators scala-xml
    Suggested packages:
    scala-doc
    The following NEW packages will be installed:
    libhawtjni-runtime-java libjansi-java libjansi-native-java libjline2-java scala scala-library scala-parser-combinators scala-xml
    0 upgraded, 8 newly installed, 0 to remove and 118 not upgraded.
    Need to get 25.0 MB of archives.
    After this operation, 28.7 MB of additional disk space will be used.
    Do you want to continue? [Y/n] Y

Check installation and run Scala to start Scala REPL

    scala

    Welcome to Scala 2.11.12 (OpenJDK 64-Bit Server VM, Java 1.8.0_292).
    Type in expressions for evaluation. Or try :help.

    scala>
