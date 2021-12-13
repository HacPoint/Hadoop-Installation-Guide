# Hadoop-Installation-Guide
Complete Documentation for Hadoop Installation in Ubuntu

# Installation

## 1. Install OpenJDK on Ubuntu

### Update your system

`sudo apt update`

### Install OpenJDK 8:

`sudo apt install openjdk-8-jdk -y`

### Verify Current Java Version

`java -version; javac -version`

## 2. Set Up a Non-Root User for Hadoop

### Install OpenSSH on Ubuntu

`sudo apt install openssh-server openssh-client -y`

### Create Hadoop User

`sudo adduser hdoop`

### Switch to Hadoop User

`su - hdoop`

### Enable Passwordless SSH for Hadoop User

`ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa`

### Use the cat command to store the public key as authorized_keys in the ssh directory

`cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`

### Set the permissions for your user with the chmod command

`chmod 0600 ~/.ssh/authorized_keys`

### Verify everything is set up correctly by using the hdoop user to SSH to localhost

`ssh localhost`

## 3. Download and Install Hadoop on Ubuntu

### Use the provided mirror link and download the Hadoop package with the wget command

`wget https://downloads.apache.org/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz`

### Extract the files to initiate the Hadoop installation

`tar xzf hadoop-3.2.1.tar.gz`

## 4. Single Node Hadoop Deployment (Pseudo-Distributed Mode)

### A Hadoop environment is configured by editing a set of configuration files:

- bashrc
- hadoop-env.sh
- core-site.xml
- hdfs-site.xml
- mapred-site-xml
- yarn-site.xml

### (a) Configure Hadoop Environment Variables (bashrc)

`sudo nano .bashrc`

- Define the Hadoop environment variables by adding the following content to the end of the file:

```
export HADOOP_HOME=/home/hdoop/hadoop-3.2.1
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS"-Djava.library.path=$HADOOP_HOME/lib/nativ"
```
- Apply the changes to the current running environment by using the following command:

`source ~/.bashrc`

### (b) Edit hadoop-env.sh File

- Use the previously created $HADOOP_HOME variable to access the hadoop-env.sh file

`sudo nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh`

- Add the following line:

`export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64`

- If you need help to locate the correct Java path, Run the following command in your terminal window:

`which javac`

- Use the provided path to find the OpenJDK directory with the following command

`readlink -f /usr/bin/javac`

### (c) Edit core-site.xml File

- Open the core-site.xml file in a text editor

`sudo nano $HADOOP_HOME/etc/hadoop/core-site.xml`

- Add the following configuration to override the default values for the temporary directory and add your HDFS URL to replace the default local file system setting:

```
<configuration>
<property>
  <name>hadoop.tmp.dir</name>
  <value>/home/hdoop/tmpdata</value>
</property>
<property>
  <name>fs.default.name</name>
  <value>hdfs://127.0.0.1:9000</value>
</property>
</configuration>
```

### (d) Edit hdfs-site.xml File

- Use the following command to open the hdfs-site.xml file for editing:

`sudo nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml`

- Add the following configuration to the file and, if needed, adjust the NameNode and DataNode directories to your custom locations:

```
<configuration>
<property>
  <name>dfs.data.dir</name>
  <value>/home/hdoop/dfsdata/namenode</value>
</property>
<property>
  <name>dfs.data.dir</name>
  <value>/home/hdoop/dfsdata/datanode</value>
</property>
<property>
  <name>dfs.replication</name>
  <value>1</value>
</property>
</configuration>
```
### (e) Edit mapred-site.xml File

- Use the following command to access the mapred-site.xml file and define MapReduce values:

`sudo nano $HADOOP_HOME/etc/hadoop/mapred-site.xml`

- Add the following configuration to change the default MapReduce framework name value to yarn:

```
<configuration> 
<property> 
  <name>mapreduce.framework.name</name> 
  <value>yarn</value> 
</property> 
</configuration>
```

### (f) Edit yarn-site.xml File

- Open the yarn-site.xml file in a text editor:

`sudo nano $HADOOP_HOME/etc/hadoop/yarn-site.xml`

- Append the following configuration to the file:

```
<configuration>
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
  <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>127.0.0.1</value>
</property>
<property>
  <name>yarn.acl.enable</name>
  <value>0</value>
</property>
<property>
  <name>yarn.nodemanager.env-whitelist</name>   
  <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PERPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
</property>
</configuration>
```

## 5. Format HDFS NameNode

### It is important to format the NameNode before starting Hadoop services for the first time:

`hdfs namenode -format`

## 6. Start Hadoop Cluster

### Navigate to the **hadoop-3.2.1/sbin** directory and execute the following commands to start the NameNode and DataNode:

`./start-dfs.sh`

### Once the namenode, datanodes, and secondary namenode are up and running, start the YARN resource and nodemanagers by typing:

`./start-yarn.sh`

### Type this simple command to check if all the daemons are active and running as Java processes:

`jps`

## 7. Access Hadoop UI from Browser

### Use your preferred browser and navigate to your localhost URL or IP. The default port number **9870** gives you access to the Hadoop NameNode UI:

`http://localhost:9870`

### The default port 9864 is used to access individual DataNodes directly from your browser:

`http://localhost:9864`

### The YARN Resource Manager is accessible on port 8088:

`http://localhost:8088`



