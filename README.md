SVB's SFTP server is old and broken and requires the hack mentioned on https://askubuntu.com/a/1093901 to work.
To achieve that, OpenSSH was compiled using `docker run -it heroku/heroku:18-build bash`:
OLD:
```
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/openssl_1.1.1f-1ubuntu2_amd64.deb
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl-dev_1.1.1f-1ubuntu2_amd64.deb
dpkg -i libssl1.1_1.1.1f-1ubuntu2_amd64.deb
dpkg -i openssl_1.1.1f-1ubuntu2_amd64.deb
dpkg -i libssl-dev_1.1.1f-1ubuntu2_amd64.deb

wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-7.9p1.tar.gz

tar xvf openssh-7.9p1.tar.gz
cd openssh-7.9p1
cat sshkey.h | sed -e 's/#define SSH_RSA_MINIMUM_MODULUS_SIZE\t1024/#define SSH_RSA_MINIMUM_MODULUS_SIZE 768'/ > x
mv x sshkey.h
./configure --prefix=/app/oldssh
make install
cd /app
tar -czvf oldssh.tgz oldssh
```
NEW
```
wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-9.3p1.tar.gz

tar xvf openssh-9.3p1.tar.gz
cd openssh-9.3p1
cat sshkey.h | sed -e 's/#define SSH_RSA_MINIMUM_MODULUS_SIZE\t1024/#define SSH_RSA_MINIMUM_MODULUS_SIZE 768'/ > x
mv x sshkey.h
./configure --prefix=/app/oldssh
make install
cd /app
tar -czvf oldssh.tgz oldssh
```

# Copy tgz from container into this repo
docker ps
docker cp d4407b85bc61:/app/oldssh.tgz bin
