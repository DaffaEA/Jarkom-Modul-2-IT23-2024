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
@       IN      A       10.75.1.4     ; IP Solok
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
@       IN      A       10.75.2.2     ; IP Kotalingga
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
@       IN      A       10.75.2.12     ; IP Tanjungkulai
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
; BIND reverse data file for 10.75.2.2
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

service bind9 restart
```

# no 7
## Settings named.conf.local
```bash
zone "sudarsana.it23.com" {
    type master;
    notify yes;
    also-notify { 10.75.2.3; };
    allow-transfer { 10.75.2.3; };
    file "/etc/bind/jarkom/sudarsana.it23.com";
};

zone "pasopati.it23.com" {
    type master;
    notify yes;
    also-notify { 10.75.2.3; };
    allow-transfer { 10.75.2.3; };
    file "/etc/bind/jarkom/pasopati.it23.com";
};

zone "rujapala.it23.com" {
    type master;
    notify yes;
    also-notify { 10.75.2.3; };
    allow-transfer { 10.75.2.3; };
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
    masters { 10.75.2.1; };
    file "/var/lib/bind/sudarsana.it23.com";
};

zone "pasopati.it23.com" {
    type slave;
    masters { 10.75.2.1; };
    file "/var/lib/bind/pasopati.it23.com";
};

zone "rujapala.it23.com" {
    type slave;
    masters { 10.75.2.1; };
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
@       IN      A       10.75.1.4     ; IP Solok
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
@       IN      A       10.75.2.2     ; IP Kotalingga
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
@       IN      A       10.75.2.12     ; IP Tanjungkulai
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
@       IN      A       10.75.1.4     ; IP Solok
www     IN      CNAME   sudarsana.it23.com.
medkit  IN     A         10.75.2.13 ; IP Bedahulu
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
@       IN      A       10.75.2.2     ; IP Kotalingga
www     IN      CNAME   pasopati.it23.com.
ns1     IN      A       10.75.2.3     ; IP Majapahit
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
    type slave;
    masters { 10.75.2.1; };
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
@       IN      NS      sudarsana.it23.com.
@       IN      A       10.75.2.2     ; IP Kotalingga
www     IN      CNAME   panah.pasopati.it23.com.' > /etc/bind/jarkom/panah.pasopati.it23.com
```
