# Jarkom-Modul-2-IT23-2024

# no 1
```
apt-get update
apt-get install bind9 -y
```

# no 2
## Sriwijaya
```bash
echo 'zone "sudarsana.it23.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/sudarsana.it23.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it23.com

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
@       IN      NS      sudarsana.it23.com.
@       IN      A       10.75.1.6     ; IP Solok
www     IN      CNAME   sudarsana.it23.com.' > /etc/bind/jarkom/sudarsana.it23.com

service bind9 restart
```

# no 3 
## Sriwijaya
```bash
echo 'zone "pasopati.it23.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/pasopati.it23.com";
};' > /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it23.com

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
@       IN      NS      pasopati.it23.com.
@       IN      A       10.75.2.12     ; IP Kotalingga
www     IN      CNAME   pasopati.it23.com.' > /etc/bind/jarkom/pasopati.it23.com

service bind9 restart
```

# no 4
## Sriwijaya
```bash
echo 'zone "rujapala.it23.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/rujapala.it23.com";
};' > /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it23.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it23.com. root.rujapala.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it23.com.
@       IN      A       10.75.2.13     ; IP Tanjungkulai
www     IN      CNAME   rujapala.it23.com.' > /etc/bind/jarkom/rujapala.it23.com

service bind9 restart
```

# no 5
## Other Domains
```bash
ping sudarsana.it23.com
ping pasopati.it23.com
ping rujapala.it23.com
```

# no 6 

```bash
echo 'zone "2.75.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.75.10.in-addr.arpa";
};' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/2.75.10.in-addr.arpa

echo '
;
; BIND reverse data file for 10.75.2.12
;
$TTL    604800
@       IN      SOA     pasopati.it23.com. root.pasopati.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
2.75.10.in-addr.arpa   IN      NS      pasopati.it23.com.
12                     IN      PTR     pasopati.it23.com.' > /etc/bind/jarkom/2.75.10.in-addr.arpa

service bind9 restart
```

# no 7
## Settings named.conf.local
```bash
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
```

## Restart
```bash
service bind9 restart
```

## Majapahit named.conf.local
```bash
echo 'zone "sudarsana.it23.com" {
    type slave;
    masters { 10.75.1.2; };
    file "/var/lib/bind/sudarsana.it23.com";
};

zone "pasopati.it23.com" {
    type slave;
    masters { 10.75.1.2; };
    file "/var/lib/bind/pasopati.it23.com";
};

zone "rujapala.it23.com" {
    type slave;
    masters { 10.75.1.2; };
    file "/var/lib/bind/rujapala.it23.com";
};' >> etc/bind/named.conf.local
```

## var/lib/bind config
```bash
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
@       IN      NS      sudarsana.it23.com.
@       IN      A       10.75.1.6     ; IP Solok
www     IN      CNAME   sudarsana.it23.com.
;@       IN      AAAA    ::1
' > /var/lib/bind/sudarsana.it23.com

```

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
@       IN      NS      pasopati.it23.com.
@       IN      A       10.75.2.12     ; IP Kotalingga
www     IN      CNAME   pasopati.it23.com.
;@       IN      AAAA    ::1
' > /var/lib/bind/pasopati.it23.com
```

```bash
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
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
' > /etc/bind/jarkom/rujapala.it23.com
```

# no 8
## Sriwijaya
```bash
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

# no 9
## Sriwijaya
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
@       IN      NS      pasopati.it23.com.
@       IN      A       10.75.2.12     ; IP Kotalingga
www     IN      CNAME   pasopati.it23.com.
ns1     IN      A       10.75.2.2     ; IP Majapahit
panah   IN      NS      ns1
@       IN      AAAA    ::1
' > /etc/bind/jarkom/pasopati.it23.com
```
```bash
echo '
options {
    directory "var/cache/bind";
    allow-query { any; };
    auth-nxdomain no; #conform to RFC1035
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
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

# no 10
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

# no 12
## Kotalingga
```bash
apt-get update
apt-get install lynx
apt-get install apache2
apt-get install php
apt-get install libapache2-mod-php7.0 -y
apt get install unzip -y
apt get install wget -y
```
```bash
mkdir /var/www/pasopati.it23.com

a2ensite pasopati.it23.com.conf

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7' -O lb.zip

unzip lb.zip  -d  lb

mv lb/* /var/www/pasopati.it23.com

cp /var/www/pasopati.it23.com/worker/index.php /var/www/pasopati.it23.com/index.php

cp /var/www/pasopati.it23.com/index.php /var/www/html/index.php
rm /var/www/html/index.html
```
```bash
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/pasopati.it23.com

echo'<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/pasopati.it23.com
    ServerName pasopati.it23.com
    ServerAlias www.pasopati.it23.com
</VirtualHost>' > /etc/apache2/sites-available/pasopati.it23.com
```
# no 13
## Solok
```bash
apt-get update
apt-get install apache2 -y

a2enmod proxy
a2enmod proxy_balancer
a2enmod proxy_http
a2enmod lbmethod_byrequests
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

</VirtualHost>' >> /etc/apache2/sites-available /000-default.conf
```

# no 14
```bash
echo '
nameserver 192.168.122.1
' > /etc/resolv.conf
apt-get update
apt install nginx php php-fpm -y

mkdir /var/www/jarkom

echo " <?php
\$hostname = gethostname();
\$date = date('Y-m-d H:i:s');
\$php_version = phpversion();
\$username = get_current_user();

echo \"Hello World!<br\>\";
echo \"Saya adalah: \$username<br\>\";
echo \"Saat ini berada di: \$hostname<br\>\";
echo \"Versi PHP yang saya gunakan: \$php_version<br\>\";
echo \"Tanggal saat ini: \$date<b\r\>\";
?>" > /var/www/jarkom/index.php

echo '
 server {

        listen 80;

        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;

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

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
 } ' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom
rm -rf /etc/nginx/sites-enabled/default

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



