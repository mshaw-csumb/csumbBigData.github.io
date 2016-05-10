---
layout: default
title: Ambari Installation
---

# Ambari Installation CentOS 6.7

Before configuring the server, the JDBC driver may need to be installed.

    $yum install mysql-connector-java*

## Set up the FQDN for all hosts
Ambari needs the Fully Qualified Domain Names of the hosts in the cluster in order to work.

Configure the /etc/hosts file and /etc/sysconfig/network files to configure the FQDN.

![](http://gdurl.com/cIKe)
![](http://gdurl.com/0wpe)
![](http://gdurl.com/d5a8)

**Adding the repository**

Navigate to the /etc/yum.repos.d/

    $cd /etc/yum.repos.d/
    $vi ambari.repo

Pass these lines into ambari.repo

    #VERSION_NUMBER=2.2.1.0-161

    [Updates-ambari-2.2.1.0]
    name=ambari-2.2.1.0 - Updates
    baseurl=http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.2.1.0
    gpgcheck=1
    gpgkey=http://public-repo-1.hortonworks.com/ambari/centos6/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins
    enabled=1
    priority=1

**Installing from the repository**

    $sudo yum update
    $sudo yum install ambari-server -y

![](http://gdurl.com/vLAd)

**Set up Ambari-Server**

    $ambari-server setup
    Y(Customize ambari-server daemon)
    Enter(default user account used)
    2(JDK 1.7)

![](http://gdurl.com/YRRv)

    $sudo ambari-server start
![](http://gdurl.com/NZd8)

**Use the Install Wizard to install hadoop and services**
* In a web browser go to http://<Name of host>:8080
* Use username and password admin admin to access the console.
* In the upper left corner click the Ambari symbol. 
![](http://gdurl.com/2GNP)

Enter desired cluster name and hit next.
![](http://gdurl.com/h4Lk)

Select the HDP 2.4 Stack and select next.
![](http://gdurl.com/VURj)

Enter all of the hosts that you would like to add the the cluster
![](http://gdurl.com/fhar)

Navigate to the /root/.ssh/id_rsa file and select the private rsa key.
![](http://gdurl.com/bDaO)

After Uploading the correct key enter the SSH Root account Account and click Register and Confirm.
![](http://gdurl.com/x95Q)

This is the screen that you want to get to. If you do not get to this screen read through the “/var/run/ambari-server/bootstrap/*/bootstrap.err” file to correct any errors.
![](http://gdurl.com/v5P7)

After the installation, hosts will be checked for any additional errors.
Use the HostCleanup script to correct these errors and manually correct errors not fixed by the script.

    $python /usr/lib/python2.6/site-packages/ambari_agent/HostCleanup.py --silent
![](http://gdurl.com/6tdW)

In order to disable transparent huge page we will need to write a script.

    $vi /etc/init.d/disable-transparent-hugepages

Paste the contents into the file.
---------------------------------------------------------------------------------------------------------------------
    ### BEGIN INIT INFO
    # Provides:      	disable-transparent-hugepages
    # Required-Start:	$local_fs
    # Required-Stop:
    # X-Start-Before:	ambari-server ambari-agent
    # Default-Start: 	2 3 4 5
    # Default-Stop:  	0 1 6
    # Short-Description: Disable Linux transparent huge pages
    # Description:   	Disable Linux transparent huge pages, to improve
    #                	database performance.
    ### END INIT INFO
    case $1 in
       start)
       if [ -d /sys/kernmel/mm/transparent_hugepage ]; then
  	    thp_path=/sys/kernel/mm/transparent_hugepage
       elif [ -d /sys/kernel/mm/redhat_transparent_hugepage ]; then
  	    thp_path=/sys/kernel/mm/redhat_transparent_hugepage
       else
  	    return 0
       fi
       echo 'never' > ${thp_path}/enabled
       echo 'never' >${thp_path}/defrag
       unset thp_path
       ;;
    esac

Change the permissions

    $sudo chmod 755 /etc/init.d/disable-transparent-hugepages

Copy the file to all other host machines in the cluster.

    $scp /etc/init.d/disable-transparent-hugepages <OtherMachineName>:/etc/init.d/

##Run these commands on all the machines on the cluster.

Add the file to chkconfig to run the script before ambari-server starts

    $sudo chkconfig --add disable-transparent-hugepages

Start the Script

    $/etc/init.d/disable-transparent-hugepages start

Once you see this and all checks pass on the hosts you can click the next Icon. 
![](http://gdurl.com/10vE)

Choose the services that you would like to install.
![](http://gdurl.com/pbZc)

Ambari will assign the roles of masters and managers by itself. Correct any issues with the roles.
![](http://gdurl.com/sOog)

Select/deselect machines to assign slaves and clients.
![](http://gdurl.com/hdi1)
![](http://gdurl.com/UnpX)

Add this entry into the /etc/hosts file before continuing with the next step. 

    54.230.144.71    public-repo-1.hortonworks.com

Review your configurations before you click deploy.
![](http://gdurl.com/8H2E)
![](http://gdurl.com/ut7y)

Once the Installation is Done you will be brought to this page. 
Your installation is complete!
![](http://gdurl.com/me79)

#Starting the Services
![](http://gdurl.com/0Qf2)
![](http://gdurl.com/ogWs)

Under the Actions tab is the option to Start All or Stop all services.
You can manually start services by clicking the service and clicking on the services in the Summary section.
![](http://gdurl.com/9Fj1)

The dashboard will bring you to the first page that we came to after the installation. 
![](http://gdurl.com/VGma)

**Monitoring Health of clusters**

The dashboard page will show the Metrics, Heatmaps, and Configuration History with the options to add and edit the information shown. From here we are able to see a general layout of the cluster. There is monitoring and information about the disk usage.
![](http://gdurl.com/Hz8t)

Heatmaps show a heatmap of the Host and services and the usage of resources.
![](http://gdurl.com/ICmm)

The Config tab show information about datanode and namenodes. The Java heap size and number of threads can be edited from here.
![](http://gdurl.com/K-nR)