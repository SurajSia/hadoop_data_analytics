# hadoop_data_analytics

# Hadoop Setup for Java on Ubuntu Budgie

This guide provides step-by-step instructions for setting up Hadoop for Java development on Ubuntu Budgie.

## Prerequisites

Ensure you have the following before starting:

- A working installation of Ubuntu Budgie.
- Basic knowledge of using the terminal.

## 1. Install Java

Hadoop requires Java to run. Install Java using the following commands:

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
```

## Verify the installation:

```bash
java --version
```

## 2. Download and Install Hadoop

Download the latest stable version of Hadoop:
```bash
cd /opt
sudo wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz

```
Extract the downloaded tarball:
```bash
sudo tar -xzvf hadoop-3.3.6.tar.gz

```
Rename the extracted directory for easier access:
```bash
sudo mv hadoop-3.3.6 hadoop

```

## 3. Configure Hadoop Environment Variables

Add Hadoop environment variables to your .bashrc file:

```bash

sudo nano ~/.bashrc
```
Add the following lines at the end of the file:
```bash
# Set Hadoop-related environment variables
export HADOOP_HOME=/opt/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin

# Set JAVA_HOME (replace with the output of `readlink -f /usr/bin/java | sed "s:/bin/java::"`)
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin

```
Apply the changes:

```bash
source ~/.bashrc

```
## 4. Configure Hadoop
### 4.1 Edit hadoop-env.sh

Set the JAVA_HOME in the Hadoop environment:
```bash
sudo nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
```
Uncomment the JAVA_HOME line and set it as follows:
```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```
### 4.2 Configure core-site.xml

Edit the core-site.xml file:
```bash
sudo nano $HADOOP_HOME/etc/hadoop/core-site.xml
```
Add the following configuration:
```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
### 4.3 Configure hdfs-site.xml

Edit the hdfs-site.xml file:

```bash
sudo nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```
Add the following configuration:
```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>file:///opt/hadoop/hadoopdata/hdfs/namenode</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>file:///opt/hadoop/hadoopdata/hdfs/datanode</value>
    </property>
</configuration>
```
### 4.4 Configure mapred-site.xml

Edit the mapred-site.xml file:

```bash
sudo nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
```
Add the following configuration:
```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```
### 4.5 Configure yarn-site.xml

Edit the yarn-site.xml file:

```bash
sudo nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
```
Add the following configuration:
```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```
## 5. Set Up SSH for Hadoop
### 5.1 Install and Enable SSH Server

Install the OpenSSH server:

```bash

sudo apt install openssh-server -y
```
Start and enable the SSH service:

```bash

sudo systemctl start ssh
sudo systemctl enable ssh
```
### 5.2 Configure Passwordless SSH

Generate SSH keys:

```bash

ssh-keygen -t rsa -P ""
```
When prompted to enter the file in which to save the key, simply press Enter.

Copy the public key to authorized_keys:

```bash

cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```
Test the SSH connection:

```bash

ssh localhost
```
If it prompts to continue, type yes and press Enter.
## 6. Format the Namenode

Before starting Hadoop, format the HDFS Namenode:

```bash

hdfs namenode -format
```
## 7. Start Hadoop Services

Start Hadoop services:

```bash

start-dfs.sh
start-yarn.sh
```
Check the status of Hadoop services:

```bash

jps
```
## 8. Access Hadoop Web Interfaces

  ###  Namenode UI: http://localhost:9870/
  ###  ResourceManager UI: http://localhost:8088/

## 9. Run a Sample Hadoop Job

Run a sample word count job:

```bash

hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount /input /output
```
## 10. Shutdown Hadoop Services

Stop Hadoop services:

```bash

stop-yarn.sh
stop-dfs.sh
```
## Summary

You now have a fully functional Hadoop setup on your Ubuntu Budgie system, ready to run Java-based MapReduce programs.

```css


You can save this content to a file named `README.md`. This file will guide you through setting up and configuring Hadoop on Ubuntu Budgie, including all necessary commands and configurations.

```
