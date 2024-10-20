[![Gitter chat](https://badges.gitter.im/gitterHQ/gitter.png)](https://gitter.im/big-data-europe/Lobby)

# Changes

Version 2.0.0 introduces uses wait_for_it script for the cluster startup

# Hadoop Docker

## Supported Hadoop Versions
See repository branches for supported hadoop versions

## Quick Start

To deploy an example HDFS cluster, run:
```
  docker-compose up
```

Run example wordcount job:
```
  make wordcount
```

Or deploy in swarm:
```
docker stack deploy -c docker-compose-v3.yml hadoop
```

`docker-compose` creates a docker network that can be found by running `docker network list`, e.g. `dockerhadoop_default`.

Run `docker network inspect` on the network (e.g. `dockerhadoop_default`) to find the IP the hadoop interfaces are published on. Access these interfaces with the following URLs:

* Namenode: http://<dockerhadoop_IP_address>:9870/dfshealth.html#tab-overview
* History server: http://<dockerhadoop_IP_address>:8188/applicationhistory
* Datanode: http://<dockerhadoop_IP_address>:9864/
* Nodemanager: http://<dockerhadoop_IP_address>:8042/node
* Resource manager: http://<dockerhadoop_IP_address>:8088/

## Configure Environment Variables

The configuration parameters can be specified in the hadoop.env file or as environmental variables for specific services (e.g. namenode, datanode etc.):
```
  CORE_CONF_fs_defaultFS=hdfs://namenode:8020
```

CORE_CONF corresponds to core-site.xml. fs_defaultFS=hdfs://namenode:8020 will be transformed into:
```
  <property><name>fs.defaultFS</name><value>hdfs://namenode:8020</value></property>
```
To define dash inside a configuration parameter, use triple underscore, such as YARN_CONF_yarn_log___aggregation___enable=true (yarn-site.xml):
```
  <property><name>yarn.log-aggregation-enable</name><value>true</value></property>
```

The available configurations are:
* /etc/hadoop/core-site.xml CORE_CONF
* /etc/hadoop/hdfs-site.xml HDFS_CONF
* /etc/hadoop/yarn-site.xml YARN_CONF
* /etc/hadoop/httpfs-site.xml HTTPFS_CONF
* /etc/hadoop/kms-site.xml KMS_CONF
* /etc/hadoop/mapred-site.xml  MAPRED_CONF

If you need to extend some other configuration file, refer to base/entrypoint.sh bash script.

-----------------------
practice 
-------------

1.	Install Docker on Windows
Step 1: Download Docker Desktop : https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-win-amd64

2.	Go to the official Docker Desktop download page:
Docker Desktop for Windows
3.	Download the Docker Desktop installer (Docker Desktop Installer.exe).
4.	Run the installer: Double-click the Docker Desktop Installer.exe file you downloaded to start the installation.
Follow the setup instructions:
5.	During installation, make sure "Enable the WSL 2 feature" is checked. Docker Desktop uses the Windows Subsystem for Linux (WSL 2) as the default backend for running containers.
6.	Click OK when prompted to install the necessary components.
7.	Start Docker Desktop: Once the installation is complete, Docker Desktop will start automatically. You can also start it manually from the Start menu.
8.	Docker Desktop uses WSL 2 by default.
Open PowerShell as Administrator and enable the WSL feature:
Run this command:
wsl –install

Verify the Installation
1.	Open PowerShell or Command Prompt and type the following command to check if Docker is installed correctly:
docker --version

 



3.	Setting up Hadoop cluster using DockerPermalink
You can confirm that docker is running properly by launching a web server:
docker run -d -p 80:80 --name myserver nginx

From git bash or powershell 
Clone the repo:
https://github.com/lntdev/docker-hadoop.git
4. Deploy the docker cluster using the command:
docker-compose up -d
 
You can check that the containers are running using:	
docker ps
 

And the current status can also be checked using the web page http://localhost:9870:
 






3. Testing the Hadoop clusterPermalink
We will test the Hadoop cluster running the Word Count example.
•	Open a terminal session on the namenode/login to name node
docker exec -it namenode bash
This will open a session on the namenode container  for the root user.
•	Create some simple text files to be used by the wordcount program
•	cd /tmp
•	mkdir input
•	echo "Hello World" >input/f1.txt
•	echo "Hello Docker" >input/f2.txt
•	Create a hdfs directory named inut
•	hadoop fs -mkdir -p input

Put the input files in all the datanodes on HDFS
hdfs dfs -put ./input/* input




Step 2: Implement File Management Tasks in Hadoop
1. Adding Files and Directories
•	To create a directory in HDFS:
hdfs dfs -mkdir /mydirectory
•	To add a file from your local file system to HDFS:
hdfs dfs -put /localpath/myfile.txt /mydirectory/
•	To add multiple files:
hdfs dfs -put /localpath/*.txt /mydirectory/
2. Retrieving Files
•	To list files in a directory in HDFS:
hdfs dfs -ls /mydirectory
•	To get a file from HDFS to your local file system:
hdfs dfs -get /mydirectory/myfile.txt /localpath/
•	To get all files in a directory:
hdfs dfs -get /mydirectory/* /localpath/
3. Deleting Files
•	To delete a file in HDFS:
hdfs dfs -rm /mydirectory/myfile.txt
•	To delete a directory (and its contents) in HDFS:
hdfs dfs -rm -r /mydirectory



•	Shutdown the Hadoop cluster by running on the Windows host
docker-compose down


 


