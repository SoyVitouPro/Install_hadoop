# Hadoop 3.3.6 Single-Node (Pseudo-Distributed) Setup Guide

This guide explains how to set up Hadoop 3.3.6 in **single-node (pseudo-distributed) mode**.  
It runs all Hadoop daemons (NameNode, DataNode, ResourceManager, NodeManager) on one machine.

---

## 1. Prerequisites
- Linux (Ubuntu/Debian recommended)
- Java 8 or 11 installed
- SSH enabled (localhost without password)
- User `hadoop` created (non-root)

Check:
```bash
java -version
ssh localhost
```

## 2. Download & Extract Hadoop
```bash
wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
tar -xvzf hadoop-3.3.6.tar.gz -C /home/hadoop/
cd ~/hadoop-3.3.6
```

## 3. Configure Environment Variables
- Edit ~/.bashrc
```bash
export HADOOP_HOME=$HOME/hadoop-3.3.6
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```
Apply
```bash
source ~/.bashrc
```
## 4. Configure Hadoop Files
```bash
cd $HADOOP_HOME/etc/hadoop
```
- hadoop-env.sh
```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```
- core-site.xml
```bash
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
  </property>
</configuration>
```

- hdfs-site.xml
```bash
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>file:///media/hadoop/nameNode</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>file:///media/hadoop/dataNode</value>
  </property>
</configuration>
```
- yarn-site.xml
```bash
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
</configuration>
```
- mapred-site.xml
```bash
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```
## 5. Format HDFS

```bash
hdfs namenode -format
```

## 6. Start Hadoop Services

```bash
start-dfs.sh
start-yarn.sh
```
- Check running daemons:
```bash
jps
```
- Expected:
  - NameNode
  - DataNode
  - ResourceManager
  - NodeManager
  - SecondaryNameNode

## 7. Verify Hadoop
- NameNode UI → http://localhost:9870
- ResourceManager UI → http://localhost:8088

## 8. Run Example Job
```bash
hdfs dfs -mkdir /input
hdfs dfs -put $HADOOP_HOME/etc/hadoop/*.xml /input
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount /input /output
hdfs dfs -cat /output/part-r-00000
```




