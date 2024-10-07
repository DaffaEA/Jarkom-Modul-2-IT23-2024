# Jarkom-Modul-2-IT23-2024

# Write Up Jarkom Modul 2

## Membuat Topologi
Menambahkan satu persatu Router, Switch dan Node" lain yang dibutuhkan, dan harus memperhatikan pembagian topologi setiap kelompok yang berbeda-beda.

Kami disini mendapat topologi No. 5, kami membuat Topologi yg sudah sesuai dengan ketentuan soal, dengan mengubah nama dan symbol dari masing" komponen topologi agar sesuai dengan ketentuan yang diberikan.

### Agar Semua Node bisa terhubung ke internet :

- Add Node dari image docker `kuuhaku86/gns3-ubuntu:1.0.1` yang sebelumnya sudah di import.
- Atur dan sesuaikan dengan topologi (symbol, hostname, dkk)
- Configure masing Node sesuai dengan fungsinya, perhatikan juga port yang digunakan dan tersambung kemana, agar alur jaringan bisa sesuai
- Config : 
```
#Nusantara
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.75.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.75.2.1
	netmask 255.255.255.0

up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.75.0.0/16

#Sriwijaya (DNS Master)
auto eth0
iface eth0 inet static
	address 10.75.1.2
	netmask 255.255.255.0
	gateway 10.75.1.1
        up echo nameserver 192.168.122.1 > /etc/resolv.conf

#apt-get update && apt-get install bind9 dnsutils -y

#Majapahit (DNS Slave)
auto eth0
iface eth0 inet static
	address 10.75.2.2
	netmask 255.255.255.0
	gateway 10.75.2.1
        up echo nameserver 192.168.122.1 > /etc/resolv.conf
        
#apt-get update && apt-get install bind9 dnsutils -y

#Sanjaya (client)
auto eth0
iface eth0 inet static
	address 10.75.1.3
	netmask 255.255.255.0
	gateway 10.75.1.1
        up echo nameserver 10.75.1.2 > /etc/resolv.conf
        up echo nameserver 10.75.2.2 >> /etc/resolv.conf

#Samaratungga (client)
auto eth0
iface eth0 inet static
    address 10.75.1.4
    netmask 255.255.255.0
    gateway 10.75.1.1
        up echo nameserver 10.75.1.2 > /etc/resolv.conf
        up echo nameserver 10.75.2.2 >> /etc/resolv.conf

#Ken Arok (client)
auto eth0
iface eth0 inet static
    address 10.75.1.5
    netmask 255.255.255.0
    gateway 10.75.1.1
        up echo nameserver 10.75.1.2 > /etc/resolv.conf
        up echo nameserver 10.75.2.2 >> /etc/resolv.conf

#Solok (client)
auto eth0
iface eth0 inet static
    address 10.75.1.6
    netmask 255.255.255.0
    gateway 10.75.1.1
        up echo nameserver 10.75.1.2 > /etc/resolv.conf
        up echo nameserver 10.75.2.2 >> /etc/resolv.conf

#Kotalingga (web server)
auto eth0
iface eth0 inet static
    address 10.75.2.12
    netmask 255.255.255.0
    gateway 10.75.2.1
        up echo nameserver 10.75.1.2 > /etc/resolv.conf
        up echo nameserver 10.75.2.2 >> /etc/resolv.conf

#Tanjungkulai (web server)
auto eth0
iface eth0 inet static
    address 10.75.2.13
    netmask 255.255.255.0
    gateway 10.75.2.1
        up echo nameserver 10.75.1.2 > /etc/resolv.conf
        up echo nameserver 10.75.2.2 >> /etc/resolv.conf

#Bedahulu (web server)
auto eth0
iface eth0 inet static
    address 10.75.2.14
    netmask 255.255.255.0
    gateway 10.75.2.1
        up echo nameserver 10.75.1.2 > /etc/resolv.conf
        up echo nameserver 10.75.2.2 >> /etc/resolv.conf
```

DNS Forwarder (Majapahit)
```
Edit file /etc/bind/named.conf.options pada server Majapahit
```
Uncomment pada bagian ini
```
forwarders {
    "IP nameserver dari Nusantara";
};
```
Comment pada bagian ini
```
// dnssec-validation auto;
```
Dan tambahkan
```
allow-query{any;};
```

![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/1.png)

# No 1
Pada GNS Master dan Slave (Sriwijaya dan Majapahit)
```
apt-get update && apt-get install bind9 dnsutils -y
```

# no 2
## Sriwijaya
```
#!/bin/bash

# Konfigurasi zona untuk sudarsana.it23.com
cat <<EOF > /etc/bind/named.conf.local
zone "sudarsana.it23.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/sudarsana.it23.com";
};
EOF

# Buat direktori untuk file zona jika belum ada
mkdir -p /etc/bind/jarkom

# Salin template file zona
cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it23.com

# Konfigurasi file zona sudarsana.it23.com
cat <<EOF > /etc/bind/jarkom/sudarsana.it23.com
;
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     sudarsana.it23.com. root.sudarsana.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it23.com.
@       IN      A       10.75.1.6     ; IP Solok
www     IN      CNAME   sudarsana.it23.com.
EOF

# Restart layanan bind9
service bind9 restart
```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/2.png)

# no 3 
## Sriwijaya
```
#!/bin/bash

# Konfigurasi zona untuk pasopati.it23.com
cat <<EOF > /etc/bind/named.conf.local
zone "pasopati.it23.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/pasopati.it23.com";
};
EOF

# Salin template file zona
cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it23.com

# Konfigurasi file zona pasopati.it23.com
cat <<EOF > /etc/bind/jarkom/pasopati.it23.com
;
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     pasopati.it23.com. root.pasopati.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it23.com.
@       IN      A       10.75.2.12     ; IP Kotalingga
www     IN      CNAME   pasopati.it23.com.
EOF

# Restart layanan bind9
service bind9 restart
```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/3.png)

# no 4
## Sriwijaya
```
#!/bin/bash

# Konfigurasi zona untuk rujapala.it23.com
cat <<EOF > /etc/bind/named.conf.local
zone "rujapala.it23.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/rujapala.it23.com";
};
EOF

# Salin template file zona
cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it23.com

# Konfigurasi file zona rujapala.it23.com
cat <<EOF > /etc/bind/jarkom/rujapala.it23.com
;
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     rujapala.it23.com. root.rujapala.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it23.com.
@       IN      A       10.75.2.13     ; IP Tanjungkulai
www     IN      CNAME   rujapala.it23.com.
EOF

# Restart layanan bind9
service bind9 restart
```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/4.png)

# no 5
## Other Domains
```
ping sudarsana.it23.com
ping pasopati.it23.com
ping rujapala.it23.com

ping www.sudarsana.it23.com
ping www.pasopati.it23.com
ping www.rujapala.it23.com
```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/5.png)

# no 6 

```bash
#!/bin/bash

# Tambahkan konfigurasi zona untuk 2.75.10.in-addr.arpa
cat <<EOF >> /etc/bind/named.conf.local
zone "2.75.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.75.10.in-addr.arpa";
};
EOF

# Salin template file zona
cp /etc/bind/db.local /etc/bind/jarkom/2.75.10.in-addr.arpa

# Konfigurasi file zona reverse untuk 2.75.10.in-addr.arpa
cat <<EOF > /etc/bind/jarkom/2.75.10.in-addr.arpa
;
; BIND reverse data file for 10.75.2.12
;
$TTL    604800
@       IN      SOA     pasopati.it23.com. pasopati.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
2.75.10.in-addr.arpa   IN      NS      pasopati.it23.com.
2                      IN      PTR     pasopati.it23.com.' > /etc/bind/jarkom/2.75.10.in-addr.arpa

# Restart layanan bind9
service bind9 restart

```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/6.png)

# no 7
## Settings named.conf.local
```bash
# Konfigurasi zona untuk Sriwijaya
cat <<EOF > /etc/bind/named.conf.local
zone "sudarsana.it23.com" {
    type master;
    notify yes;
    also-notify { 10.75.2.2; };
    allow-transfer { 10.75.2.2; };
    file "/etc/bind/jarkom/sudarsana.it23.com";
};

zone "pasopati.it23.com" {
    type master;
    notify yes;
    also-notify { 10.75.2.2; };
    allow-transfer { 10.75.2.2; };
    file "/etc/bind/jarkom/pasopati.it23.com";
};

zone "rujapala.it23.com" {
    type master;
    notify yes;
    also-notify { 10.75.2.2; };
    allow-transfer { 10.75.2.2; };
    file "/etc/bind/jarkom/rujapala.it23.com";
};

zone "2.75.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.75.10.in-addr.arpa";
};
EOF

# Restart layanan bind9
service bind9 restart
```

## Restart
```bash
service bind9 restart
```

## Majapahit named.conf.local
```bash
# Konfigurasi zona untuk Sriwijaya
cat <<EOF > /etc/bind/named.conf.local
zone "sudarsana.it23.com" {
    type master;
    notify yes;
    also-notify { 10.75.2.2; };
    allow-transfer { 10.75.2.2; };
    file "/etc/bind/jarkom/sudarsana.it23.com";
};

zone "pasopati.it23.com" {
    type master;
    notify yes;
    also-notify { 10.75.2.2; };
    allow-transfer { 10.75.2.2; };
    file "/etc/bind/jarkom/pasopati.it23.com";
};

zone "rujapala.it23.com" {
    type master;
    notify yes;
    also-notify { 10.75.2.2; };
    allow-transfer { 10.75.2.2; };
    file "/etc/bind/jarkom/rujapala.it23.com";
};

zone "2.75.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.75.10.in-addr.arpa";
};
EOF

# Restart layanan bind9
service bind9 restart
```

## var/lib/bind config
```bash
#!/bin/bash

# Konfigurasi file zona sudarsana.it23.com
cat <<EOF > /var/lib/bind/sudarsana.it23.com
;
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     sudarsana.it23.com. root.sudarsana.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it23.com.
@       IN      A       10.75.1.6     ; IP Solok
www     IN      CNAME   sudarsana.it23.com.
;@       IN      AAAA    ::1
EOF

# Konfigurasi file zona pasopati.it23.com
cat <<EOF > /var/lib/bind/pasopati.it23.com
;
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     pasopati.it23.com. root.pasopati.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it23.com.
@       IN      A       10.75.2.12     ; IP Kotalingga
www     IN      CNAME   pasopati.it23.com.
;@       IN      AAAA    ::1
EOF

# Konfigurasi file zona rujapala.it23.com
cat <<EOF > /etc/bind/jarkom/rujapala.it23.com
;
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     rujapala.it23.com. root.rujapala.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it23.com.
@       IN      A       10.75.2.13     ; IP Tanjungkulai
www     IN      CNAME   rujapala.it23.com.
;@       IN      AAAA    ::1
EOF

# Restart layanan bind9
service bind9 restart

```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/7.png)
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/8.png)

# no 8
## Sriwijaya
```bash
#!/bin/bash

# Konfigurasi file zona sudarsana.it23.com
cat <<EOF > /etc/bind/jarkom/sudarsana.it23.com
;
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     sudarsana.it23.com. root.sudarsana.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it23.com.
@       IN      A       10.75.1.6     ; IP Solok
www     IN      CNAME   sudarsana.it23.com.
cakra  IN     A         10.75.2.14 ; IP Bedahulu
@        IN     AAAA     ::1
' > /etc/bind/jarkom/sudarsana.it23.com
```
## Restart
```bash
service bind9 restart

```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/9.png)

# no 9
## Sriwijaya
```bash
#!/bin/bash

# Konfigurasi file zona pasopati.it23.com
cat <<EOF > /etc/bind/jarkom/pasopati.it23.com
;
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     pasopati.it23.com. root.pasopati.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it23.com.
@       IN      A       10.75.2.12     ; IP Kotalingga
www     IN      CNAME   pasopati.it23.com.
ns1     IN      A       10.75.2.2     ; IP Majapahit
panah   IN      NS      ns1
@       IN      AAAA    ::1
EOF

# Konfigurasi opsi di named.conf.options
cat <<EOF > /etc/bind/named.conf.options
options {
    directory "var/cache/bind";
    allow-query { any; };
    auth-nxdomain no; #conform to RFC1035
    listen-on-v6 { any; };
};
EOF

# Restart layanan bind9
service bind9 restart

```

## Majapahit
```bash
echo '
options {
    directory "var/cache/bind";
    allow-query { any; };
    auth-nxdomain no; #conform to RFC1035
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```

```bash
echo 'zone "panah.pasopati.it23.com" {
    type master;
    file "/etc/bind/jarkom/panah.pasopati.it23.com";
};' >> etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/panah.pasopati.it23.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it23.com. root.sudarsana.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it23.com.
@       IN      A       10.75.2.12     ; IP Kotalingga
www     IN      CNAME   panah.pasopati.it23.com.' > /etc/bind/jarkom/panah.pasopati.it23.com
```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/10.png)

# no 10
## Majapahit
```bash
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it23.com. root.pasopati.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@               IN      NS          panah.pasopati.it23.com.
@               IN      A           10.75.2.12     ; IP Kotalingga
www             IN      CNAME       panah.pasopati.it23.com.
log             IN      A           10.75.2.12
www.log         IN      CNAME       panah.pasopati.it23.com
@               IN      AAAA        ::1
' > /etc/bind/jarkom/panah.pasopati.it23.com
```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/11.png)

# no 11
## Majapahit
```bash
echo 'options {
    directory "/var/cache/bind";

    forwarders {
        192.168.122.1;
    }

    //dnssec-validation auto;
    allow-query{any;};

    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/12.png)

# no 12
## Kotalingga
```bash
apt-get update
apt-get install apache2 libapache2-mod-php7.0 php wget unzip -y
```
```bash
<<<<<<< HEAD
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/pasopati.it23.com.conf

<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/pasopati.it23.com
    ServerName pasopati.it23.com
    ServerAlias www.pasopati.it23.com
</VirtualHost>
```
```bash
=======
>>>>>>> d1e0dada26717e798be5c41cc0b3af05458227f8
mkdir /var/www/pasopati.it23.com

a2ensite pasopati.it23.com.conf

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7' -O lb.zip

unzip lb.zip  -d  lb

mv lb/* /var/www/pasopati.it23.com

cp /var/www/pasopati.it23.com/worker/index.php /var/www/pasopati.it23.com/index.php

cp /var/www/pasopati.it23.com/index.php /var/www/html/index.php
rm /var/www/html/index.html
```
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/13.png)

## ulangi untuk setiap webserver (sudarsana, pasopati, rujapala)
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/14.png)
![github-small](https://github.com/DaffaEA/Jarkom-Modul-2-IT23-2024/blob/main/image/15.png)

# no 13
## Solok
```bash
apt-get install lynx apache2 apache2-utils php7.0 php7.0-fpm -y

a2enmod proxy proxy_balancer proxy_http lbmethod_byrequests lbmethod_bytraffic lbmethod_bybusyness
```
```bash
echo '<VirtualHost *:80>
    <Proxy balancer://mycluster>
        BalancerMember http://10.75.2.12
        BalancerMember http://10.75.2.13
        BalancerMember http://10.75.2.14
        ProxySet lbmethod=byrequests
    </Proxy>

    ProxyPass / balancer://mycluster/
    ProxyPassReverse / balancer://mycluster/

</VirtualHost>' >> /etc/apache2/sites-available/000-default.conf
```
(image 16 - 19)

# no 14
```bash

apt-get update
apt install nginx php php-fpm -y

echo 'server {
    listen 80;

    root /var/www/pasopati.it23.com;

    index index.php index.html index.htm;
    server_name _ pasopati.it23.com www.pasopati.it23.com;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}' > /etc/nginx/sites-available/pasopati.it23.com

ln -s /etc/nginx/sites-available/pasopati.it23.com /etc/nginx/sites-enabled/pasopati.it23.com
rm /etc/nginx/sites-enabled/default

service nginx restart
service php7.0-fpm stop
service php7.0-fpm start
```
## Solok (lb)
```bash
apt-get install nginx -y
```
```bash
apt-get update
apt-get install bind9 nginx -y

echo ' # Default menggunakan Round Robin
 upstream myweb  {
        server 10.75.2.12;
        server 10.75.2.13;
        server 10.75.2.14; 
 }

 server {
        listen 80;
        server_name _;

        location / {
        proxy_pass http://myweb;
        }
 }' > /etc/nginx/sites-available/lb

ln -s /etc/nginx/sites-available/lb /etc/nginx/sites-enabled/lb

rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```



