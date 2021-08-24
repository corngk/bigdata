# Install Hadoop 3.3.1 on Windows 10 with WSL 2 Ubuntu 20.04
### Prerequisites
Follow the page below to enable WSL and then install one of the Linux systems from Microsoft Store.

[Windows Subsystem for Linux Installation Guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

### Install Java JDK
Run the following command to update package index:

    sudo apt update

Check whether Java is installed already:

    java -version

Command 'java' not found, but can be installed with:

    sudo apt install default-jre
    sudo apt install openjdk-11-jre-headless
    sudo apt install openjdk-8-jre-headless

Install OpenJDK via the following command:

    sudo apt-get install openjdk-8-jdk

Check the version installed:

    java -version
    openjdk version "1.8.0_191"
    OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-2ubuntu0.18.04.1-b12)
    OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)


### Download Hadoop binary
Go to release page of Hadoop website to find a download URL for Hadoop 3.3.1:

https://downloads.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz

Extract the tar file to hadoop folder. I install it on different drive `/mnt/e`.
Run the following command to create a hadoop folder under home folder:

    mkdir /mnt/e/hadoop

And then run the following command to unzip the binary package:

    tar -xvzf hadoop-3.3.1.tar.gz -C /mnt/e/hadoop

Once it is unzipped, change the current directory to the hadoop folder:

    cd /mnt/e/hadoop/hadoop-3.3.1/

### Configure passphraseless ssh
This step is critical and please make sure you follow the steps.

Make sure you can SSH to localhost in Ubuntu:

    ssh localhost

If you cannot ssh to localhost without a passphrase, run the following command 

To initialize your private and public keys:

    ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    chmod 0600 ~/.ssh/authorized_keys

If you encounter errors like ‘ssh: connect to host localhost port 22: Connection refused’, run the following commands:

    sudo apt-get install ssh
    And then restart the service:
    sudo service ssh restart

### Configure the pseudo-distributed mode (Single-node mode)
Now, we can follow the official guide to configure a single node:

Pseudo-Distributed Operation

The steps are very similar to the ones in my previous post.

Edit `etc/hadoop/hadoop-env.sh` file:

    vi etc/hadoop/hadoop-env.sh

Set a JAVA_HOME environment variable:

    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

Edit `etc/hadoop/core-site.xml:`

    vi etc/hadoop/core-site.xml

Add the following configuration:

    <configuration>
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://localhost:9000</value>
        </property>
    </configuration>

Edit etc/hadoop/hdfs-site.xml:

    vi etc/hadoop/hdfs-site.xml

Add the following configuration:

    <configuration>
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>
    </configuration>

Edit file etc/hadoop/mapred-site.xml:

    vi etc/hadoop/mapred-site.xml

Add the following configuration:

    <configuration>
        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
        <property>
            <name>mapreduce.application.classpath</name>
            <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
        </property>
    </configuration>

Edit file etc/hadoop/yarn-site.xml:

    vi etc/hadoop/yarn-site.xml

Add the following configuration:

    <configuration>
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>
        <property>
            <name>yarn.nodemanager.env-whitelist</name>
            <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
        </property>
    </configuration>

### Format namenode
Run the following command to format the name node:

    bin/hdfs namenode -format

### Run DFS daemons
Run the following commands to start NameNode and DataNode daemons:

    sbin/start-dfs.sh

    Starting namenodes on [localhost]
    Starting datanodes
    Starting secondary namenodes [DESKTOP-2P1IDOF]
    DESKTOP-2P1IDOF: Warning: Permanently added 'desktop-2p1idof' (ECDSA) to the list of known hosts.

You can view the name node through the following URL:

    http://localhost:9870/dfshealth.html#tab-overview


### Run YARN daemon
Run the following command to start YARN daemon:

sbin/start-yarn.sh

Once the services are started, you can view the YARN resource manager web UI through the following URL:

    http://localhost:8088/cluster

### Environment variables
To make it easier to run Hadoop commands, add the following environment variables into .bashrc file in your home folder:

    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
    export HADOOP_HOME=/home/user/hadoop/hadoop-3.3.1
    export PATH=$PATH:$HADOOP_HOME/bin

Remember to change the *user* to your own user name in the Linux system.

### Summary
Congratulations! You have successfully installed a single node Hadoop 3.3.1 cluster in your Ubuntu subsystem of Windows 10. It’s relatively easier as we don’t need to download or compile/build native Hadoop libraries.

BTW, WSL is not a virtual machine however it provides you almost the same experience as you would have in a native Linux system.

Happy coding!