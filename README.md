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
``` 

Request the commercial license 

https://alluxio.com/account/#download-manager

Setup alluxio
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
``` 
