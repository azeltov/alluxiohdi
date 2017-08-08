# alluxiohdi


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
