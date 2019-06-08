SVB's SFTP server is old and broken and requires the hack mentioned on https://askubuntu.com/a/1093901 to work.
To achieve that, OpenSSH was compiled using `docker run -it heroku/heroku:18-build bash`:
```
wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-7.9p1.tar.gz
tar xvf openssh-7.9p1.tar.gz
cat sshkey.h | sed -e 's/#define SSH_RSA_MINIMUM_MODULUS_SIZE\t1024/#define SSH_RSA_MINIMUM_MODULUS_SIZE 768'/ > x
mv x sshkey.h
./configure --prefix=/app/oldssh
make install
cd /app
tar -czvf oldssh.tgz oldssh
```
