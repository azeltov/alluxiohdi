# Alluxio setup on hdinsight 3.6 (spark 2.x)


## Setup Passwordless SSH on all Nodes except zoo-keeper

Generate Keys : accept defaults
``` 
ssh-keygen

cat .ssh/id_rsa.pub | ssh sshuser@hn1-maxluk  'cat >> .ssh/authorized_keys'
ssh hn1-maxluk

for i in wn{1..2}-maxluk; do ssh-copy-id $i;done
for i in wn{3..38}-maxluk; do ssh-copy-id $i;done

ssh-copy-id wn44-maxluk
ssh wn44-maxluk

ssh-copy-id hn0-maxluk
ssh hn0-maxluk

``` 

Request the commercial license 

https://alluxio.com/account/#download-manager

## Setup alluxio
``` 
mkdir alluxio
cd alluxio/
wget http://downloads.alluxio.org/downloads/files/1.5.0/alluxio-1.5.0-hadoop-2.7-bin.tar.gz
tar -xvf alluxio-1.5.0-hadoop-2.7-bin.tar.gz
cd alluxio-1.5.0-hadoop-2.7/
./bin/alluxio bootstrapConf hn0-maxluk
   
sshuser@hn0-maxluk:~/alluxio/alluxio-1.5.0-hadoop-2.7$   ./bin/alluxio bootstrapConf hn0-maxluk
/home/sshuser/alluxio/alluxio-1.5.0-hadoop-2.7/bin/../conf/alluxio-env.sh is created.


sshuser@hn0-maxluk:~/alluxio/alluxio-1.5.0-hadoop-2.7$ cd conf/
sshuser@hn0-maxluk:~/alluxio/alluxio-1.5.0-hadoop-2.7/conf$ ll
total 40
drwxrwxr-x  2 sshuser sshuser 4096 Aug  8 16:33 ./
drwxrwxr-x 20 sshuser sshuser 4096 Jun 12 08:44 ../
-rw-rw-r--  1 sshuser sshuser  953 Aug  8 16:33 alluxio-env.sh
-rw-rw-r--  1 sshuser sshuser 2709 Jun 12 08:17 alluxio-env.sh.template
-rw-rw-r--  1 sshuser sshuser 1229 Jun 12 08:17 alluxio-site.properties.template
-rw-rw-r--  1 sshuser sshuser 3737 Jun 12 08:17 core-site.xml.template
-rw-rw-r--  1 sshuser sshuser 3263 Jun 12 08:17 log4j.properties
-rw-rw-r--  1 sshuser sshuser 1058 Jun 12 08:17 masters
-rw-rw-r--  1 sshuser sshuser 2966 Jun 12 08:17 metrics.properties.template
-rw-rw-r--  1 sshuser sshuser  587 Jun 12 08:17 workers


sshuser@hn0-maxluk:~/alluxio/alluxio-1.5.0-hadoop-2.7/conf$ for i in wn{0..38}-maxluk; do echo $i;done
wn0-maxluk
wn1-maxluk
wn2-maxluk
wn3-maxluk
wn4-maxluk
wn5-maxluk
wn6-maxluk
wn7-maxluk
wn8-maxluk
wn9-maxluk
wn10-maxluk
wn11-maxluk
wn12-maxluk
wn13-maxluk
wn14-maxluk
wn15-maxluk
wn16-maxluk
wn17-maxluk
wn18-maxluk
wn19-maxluk
wn20-maxluk
wn21-maxluk
wn22-maxluk
wn23-maxluk
wn24-maxluk
wn25-maxluk
wn26-maxluk
wn27-maxluk
wn28-maxluk
wn29-maxluk
wn30-maxluk
wn31-maxluk
wn32-maxluk
wn33-maxluk
wn34-maxluk
wn35-maxluk
wn36-maxluk
wn37-maxluk
wn38-maxluk

sshuser@hn0-maxluk:~/alluxio/alluxio-1.5.0-hadoop-2.7/conf$ vi workers
# An Alluxio Worker will be started on each of the machines listed below.
wn0-maxluk
wn1-maxluk
wn2-maxluk
wn3-maxluk
wn4-maxluk
wn5-maxluk
wn6-maxluk
wn7-maxluk
wn8-maxluk
wn9-maxluk
wn10-maxluk
wn11-maxluk
wn12-maxluk
wn13-maxluk
wn14-maxluk
wn15-maxluk
wn16-maxluk
wn17-maxluk
wn18-maxluk
wn19-maxluk
wn20-maxluk
wn21-maxluk
wn22-maxluk
wn23-maxluk
wn24-maxluk
wn25-maxluk
wn26-maxluk
wn27-maxluk
wn28-maxluk
wn29-maxluk
wn30-maxluk
wn31-maxluk
wn32-maxluk
wn33-maxluk
wn34-maxluk
``` 

Add hn0-maxluk to conf/masters file
``` 
sshuser@hn0-maxluk:~/alluxio/alluxio-1.5.0-hadoop-2.7$ vi conf/masters
sshuser@hn0-maxluk:~/alluxio/alluxio-1.5.0-hadoop-2.7$ cp conf/alluxio-site.properties.template conf/alluxio-site.properties

``` 

Bump down yarn to 85g. Alluxio is using the other 15G
``` 
sshuser@hn0-maxluk:~/alluxio/alluxio-1.5.0-hadoop-2.7$ vi conf/alluxio-env.sh
ALLUXIO_MASTER_HOSTNAME=${ALLUXIO_MASTER_HOSTNAME:-"hn0-maxluk"}
ALLUXIO_WORKER_MEMORY_SIZE=${ALLUXIO_WORKER_MEMORY_SIZE:-"15360MB"}
ALLUXIO_RAM_FOLDER=${ALLUXIO_RAM_FOLDER:-"/mnt/ramdisk"}

export ALLUXIO_CLASSPATH=/usr/hdp/2.6.1.3-4/hadoop/client/azure-storage-4.2.0.jar:/usr/hdp/2.6.1.3-4/hadoop/hadoop-azure-2.7.3.2.6.1.3-4.jar
``` 

Specify the wasb address alluxio.underfs.address and alluxio.underfs.hdfs.prefixes
``` 

sshuser@hn0-maxluk:~/alluxio/alluxio-1.5.0-hadoop-2.7$ cat conf/alluxio-site.properties
#
# The Alluxio Open Foundation licenses this work under the Apache License, version 2.0
# (the "License"). You may not use this work except in compliance with the License, which is
# available at www.apache.org/licenses/LICENSE-2.0
#
# This software is distributed on an "AS IS" basis, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
# either express or implied, as more fully set forth in the License.
#
# See the NOTICE file distributed with this work for information regarding copyright ownership.
#

# Site specific configuration properties for Alluxio
# Details about all configuration properties http://www.alluxio.org/documentation/en/Configuration-Settings.html

# Common properties
# alluxio.master.hostname=localhost
alluxio.underfs.address=wasb://maxluk-alluxio-2017-08-02t17-39-40-247z@maxluksparkeus.blob.core.windows.net/alluxio

alluxio.underfs.hdfs.prefixes=hdfs://,wasb://

#${alluxio.work.dir}/underFSStorage

# Security properties
# alluxio.security.authorization.permission.enabled=true
# alluxio.security.authentication.type=SIMPLE

# Worker properties
# alluxio.worker.memory.size=1GB
# alluxio.worker.tieredstore.levels=1
# alluxio.worker.tieredstore.level0.alias=MEM
# alluxio.worker.tieredstore.level0.dirs.path=/mnt/ramdisk

# User properties
# alluxio.user.file.readtype.default=CACHE_PROMOTE
# alluxio.user.file.writetype.default=MUST_CACHE
``` 
copy core-site.xml to alluxio conf directory

``` 
cp /etc/hadoop/conf/core-site.xml .
mv core-site.xml conf/
``` 

Copy the binaries to all the worker nodes

``` 
./bin/alluxio copyDir /home/sshuser/alluxio/alluxio-1.5.0-hadoop-2.7
``` 

## Start alluxio:
``` 

./bin/alluxio format
./bin/alluxio-start.sh
./bin/alluxio-start.sh all
``` 
## Troubleshoot alluxio:
``` 
ps -ef | grep alluxio
jps
cd logs/
vi master.out
``` 
