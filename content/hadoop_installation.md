---
layout: default
title: Hadoop Installation
---

Commands to run	Text configuration

# Commands to run

**Step 1: Add a local user**
Add a normal account for hadoop to work on. 

    $useradd hadoop

Configure the password on hadoop

    $hadoop passwd

Enter the password

Login to the hadoop account 

    $su hadoop

**Step 2: Installing Java**
Java is the primary prerequisite for running Hadoop. 

Check the system to see if java is installed by issuing the following command:

    $java -version
![](http://gdurl.com/ZUiP)

If Java is not installed download the java tar file using wget.

    $wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u72-b15/jdk-8u72-linux-x64.tar.gz"
![](http://gdurl.com/z-Sq)

Move the java tar file to the /usr/local directory

    $sudo mv jdk-8u72-linux-x64.tar.gz  /usr/local

Extract the files using tar

    $tar xzf  jdk-8u72-linux-x64.tar.gz

Set up PATH and JAVA_HOME variables

    $vi ~/.bashrc

Append these lines to the file

    export JAVA_HOME=/usr/local/jdk1.7.0_71 
    export PATH=$PATH:$JAVA_HOME/bin

Apply the changes using source to execute the commands in .bashrc file

    $source ~/.bashrc

**Step 3: Installing Hadoop**
Navigate to the /usr/local directory

    $cd /usr/local

Get the hadoop tar file

    $wget wget http://apache.claz.org/hadoop/common/hadoop-2.4.1/hadoop-2.4.1.tar.gz

Unpack the tar.gz file

    $tar xzf http://apache.claz.org/hadoop/common/hadoop-2.4.1/hadoop-2.4.1.tar.gz

Change the name of hadoop-2.4.1 folder to hadoop

    $mv hadoop-2.4.1/  hadoop

Set up hadoop environment variables and append following line to ~/.bashrc
    
    export HADOOP_HOME=/usr/local/hadoop

Verify that hadoop is working

    $hadoop version
![](http://gdurl.com/TY6J)

**Step 4 Configuring the network**

Turn off selinux or set it to permissive mode

    $sudo vi /etc/selinux/config

Change enabled to permissive or disabled.

Verify selinux changes

    $getenforce
The output should be permissive or disabled.


Configure each machine with a static ip address.

    vi /etc/sysconfig/network-scripts/ifcfg-eth0

Example configuration settings. Make changes necessary to your network settings.

    NM_CONTROLLED=yes
    BOOTPROTO=STATIC
    IPADDR=192.168.100.xxx
    NETMASK=255.255.255.0
    GATEWAY=192.168.100.1
    DNS1=192.168.100.1
    ONBOOT=yes
    HWADDR=C8:CB:B8:12:F4:78
    DEFROUTE=yes
    PEERDNS=yes
    PEERROUTES=yes
    IPV4_FAILURE_FATAL=yes
    IPV6INIT=no
    NAME="System eth0"

On each machine configure the /etc/hosts file to map to different machines in your local network.
Example:

    127.0.0.1           localhost localhost.localdomain localhost4 localhost4.localdomain4 masternode
    192.168.100.195	masternode localhost 
    192.168.100.196	numbertwo 
    192.168.100.197 	numberthree
    192.168.100.198	numberfour
    ::1                 localhost localhost.localdomain localhost6 localhost6.localdomain6


**Step 5 Configuring Hadoop nodes**

INSTALLING HADOOP IN PSEUDO DISTRIBUTED MODE

Append the following to the ~/.bashrc file

    export HADOOP_HOME=/usr/local/hadoop 
    export HADOOP_MAPRED_HOME=$HADOOP_HOME 
    export HADOOP_COMMON_HOME=$HADOOP_HOME 
    export HADOOP_HDFS_HOME=$HADOOP_HOME 
    export YARN_HOME=$HADOOP_HOME 
    export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native 
    export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin 
    export HADOOP_INSTALL=$HADOOP_HOME

Apply the changes

    $source ~/.bashrc

Hadoop configuration files are located in $HADOOP_HOME/etc/hadoop/
In order to set up the Hadoop infrastructure these configuration files must be changed. 

Core-site.xml configuration
Add these properties between the <configuration> tab

    <configuration>
       <property>
          <name>fs.default.name </name>
          <value> hdfs://localhost:9000 </value> 
       </property>
     </configuration>

Hdfs-site.xml configuration

    <configuration>
       <property>
          <name>dfs.replication</name>
          <value>1</value>
       </property>
       <property>
          <name>dfs.name.dir</name>
          <value>file:///home/hadoop/hadoopinfra/hdfs/namenode </value>
       </property>
       <property>
          <name>dfs.data.dir</name> 
          <value>file:///home/hadoop/hadoopinfra/hdfs/datanode </value> 
       </property>
   </configuration>

Yarn-site.xml configuration 

    <configuration>
        <property>
          <name>yarn.nodemanager.aux-services</name>
          <value>mapreduce_shuffle</value> 
       </property>
    </configuration>

Make a copy of the mapred-site.xml.template and name it mapred-site.xml

    :cp mapred-site.xml.template mapred-site.xml

Put these changes into the <configuration> tab.
    <configuration>
        <property> 
          <name>mapreduce.framework.name</name>
          <value>yarn</value>
       </property>
   </configuration>




Replicate these changes to the other nodes on the cluster.

    $scp -r $HADOOP_HOME/ /usr/local/hadoop/

Format the namenode

    $hdfs namenode -format

Start up the hadoop cluster by executing the scripts that are provided by hadoop.

    $cd $HADOOP_HOME/sbin
    $./start-all.sh


Depending on the mode that hadoop is configured by the xml configuration files, the information available for the namenode and secondary namenode can be accessed through the browser by entering the namenode and secondary namenode and the ports 50070 and 8088.

Example 

    <Masternodehostname>:8088
    <Masternodehostname>:50070
    <secondarynamenodehostname>:50090
