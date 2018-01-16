# guide-to-newvpp
Build from source: 
1. ##Get the latest source code 
     git clone https://github.com/shadowsocks/shadowsocks-libev.git
     cd shadowsocks-libev
     git submodule update --init --recursive 

2. ##Build and install with recent libsodium 
     sudo apt-get update 
     sudo apt-get install python-pip m2crypto 
     wget -N --no-check-certificate https://download.libsodium.org/libsodium/releases/LATEST.tar.gz
     tar -xzvf libsodium-*.tar.gz
     cd libsodium-*
     ./configure
     make && make install
     echo "/usr/local/lib" >> /etc/ld.so.conf
     ldconfig 

3. ##Installation of MbedTLS
     wget https://tls.mbed.org/download/mbedtls-$MBEDTLS_VER-gpl.tgz
     tar xvf mbedtls-$MBEDTLS_VER-gpl.tgz
     pushd mbedtls-$MBEDTLS_VER
     make SHARED=1 CFLAGS=-fPIC
     sudo make DESTDIR=/usr install
     popd
     sudo ldconfig 

4. ##Build dependencies 
     sudo apt-get install build-essential autoconf libtool libssl-dev gcc
     sudo apt-get install asciidoc libev-dev libmbedtls-dev libpcre3-dev libudns-dev pkg-config xmlto 
          debhelper apg pwgen rng-tools dh-systemd libc-ares2 libc-ares-dev pcre-dev


4. ##Build deb package from source 
     cd shadowsocks-libev
     ./autogen.sh && ./configure && make
     make install

5. ##Create file shadowsocks.json
{
    "server":"138.68.18.65",
    "server_port":443,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"",
    "timeout":300,
    "method":"chacha20-ietf-poly1305",
    "fast_open": false
}

6. setsid ss-server -c /etc/shadowsocks.json
   netstat -lnp

don't install libsodium-dev 
Ubuntu 16.10 64bit


##kcptun
  cd ~ && mkdir kcptun
  wget https://github.com/xtaci/kcptun/releases/download/latest/kcptun-linux-amd64-*.tar.gz
  tar -zxvf kcptun-linux-amd64-*.tar.gz
##server config.json
{
    "listen": "138.68.18.65:29900",
    "target": "138.68.18.65:443",
    "key": "",
    "crypt": "salsa20",
    "mode": "fast3",
    "mtu": 1400,
    "sndwnd": 2048,
    "rcvwnd": 2048,
    "datashard": 10,
    "parityshard": 3,
    "dscp": 46,
    "nocomp": false
}
##client config.json
{
    "localaddr": "0.0.0.0:12948",
    "remoteaddr": "138.68.18.65:29900",
    "key": "",
    "crypt": "salsa20",
    "mode": "fast3",
    "mtu": 1400,
    "sndwnd": 2048,
    "rcvwnd": 2048,
    "datashard": 10,
    "parityshard": 3,
    "dscp": 46,
    "nocomp": false
}
##start service
nohup ./server_linux_amd64 -c ./kcptun/config.json

-mtu 1200
-?dscp 46
-?sndwnd 128
-?rcvwnd 512
-datashard 10/70/5/0
-?parityshard 3/30/5/0
-?autoexpire <5
-?conn 10

dscp=0;nodelay=0;parityshard=30;interval=20;nocomp;rcvwnd=512;crypt=salsa20;nc=1;resend=0;
autoexpire=5;conn=5;mode=manual;key=coming;remoteaddr=66.112.210.159:3499;mtu=500;datashard=70;sndwnd=128

{
"listen": "66.112.210.159:3499",
"target": "66.112.210.159:8388",
"key": "coming",
"crypt": "salsa20",
"mode": "manual",
"nodelay": 1,
"resend": 0,
"nc": 1,
"interval": 40,
"mtu": 500,
"sndwnd": 1024,
"rcvwnd": 1024,
"datashard": 70,
"parityshard": 30,
"dscp": 0,
"conn": 5,
"autoexpire": 5,
"nocomp": true
}

-mode manual -nodelay 1 -resend 0 -nc 1 -interval 40
-mode manual -nodelay 0 -resend 0 -nc 1 -interval 20

-mode fast -datashard 5 -parityshard 5

Additonally, For CentOS 6, 

1. ##"Autoconf version 2.67 or higher is required"

$ wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
$ tar xvfvz autoconf-2.69.tar.gz
$ cd autoconf-2.69
$ ./configure
$ make
$ sudo make install

##After installation, verify autoconf version:
$ autoconf --version 

2. yum install pcre-devel 

3. ##"configure: error: Couldn't find libudns. Try installing libudns-dev or udns-devel." etc. 
   yum install epel-release
   yum --enablerepo=epel install udns-devel libev-devel
  

unzip Joomla_3-7.4-Stable-Full_Package.zip -d /var/zpanel/hostdata/zadmin/public_html/
tar -xvf drupal-8.3.5.tar.gz -C /var/zpanel/hostdata/zadmin/public_html/

chmod a+w /var/zpanel/hostdata/zadmin/public_html/*/sites/default/settings.php
chmod go-w /var/zpanel/hostdata/zadmin/public_html/*/sites/default/settings.php

/var/zpanel/hostdata/zadmin/public_html/*/sites/default/files/customizer_css


**Important**
change file permission to 775, /var/zpanel/hostdata/zadmin/public_html/*/sites/default/files/.htaccess

##"Drupal requires you to enable the PHP extensions in the following list 
##(see the system requirements page for more information):      dom     gd"
   yum install libapache2-mod-auth-mysql php5-mysql phpmyadmin 
   /etc/init.d/httpd restart 

## Edit /etc/php.ini, uncomment "upload_tmp_dir"
  upload_tmp_dir = /var/zpanel/hostdata/zadmin/public_html/*/wp-content/temp 

**Important**
temp to 777

## Restart services 
/etc/init.d/mysqld restart
/etc/init.d/httpd restart 
