# Jarkom-Modul-2-IT23-2024

# no 1
```
apt-get update
apt-get install bind9 -y
```

# no 2
### Sriwijaya
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
@       IN      SOA     sudarsana.it23.com. sudarsana.it23.com. (
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
### Sriwijaya
```
echo 'zone "pasopati.it23.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/pasopati.it23.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it23.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it23.com. pasopati.it23.com. (
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
#### Sriwijaya
```
echo 'zone "rujapala.it23.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/rujapala.it23.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it23.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it23.com. rujapala.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it23.com.
@       IN      A       10.75.2.2     ; IP Kotalingga
www     IN      CNAME   rujapala.it23.com.' > /etc/bind/jarkom/rujapala.it23.com

service bind9 restart
```

# no 5
### Other Domains
```
ping sudarsana.it23.com
ping pasopati.it23.com
ping rujapala.it23.com
```

# no 6 
