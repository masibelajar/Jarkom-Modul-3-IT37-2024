# Jarkom-Modul-3-IT37-2024


| Nama                            | NRP           |
|---------------------------------|---------------|
| Rama Owarianto Putra Suharjito  | 5027231049    |
| Muhammad Rizq Taufan            | 5027231021    |

## Topologi:  
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/topologi.png)

Pulau Paradis dan Marley, sama-sama menghadapi ancaman besar dari satu sama lain. Keduanya membangun infrastruktur pertahanan yang kuat untuk melindungi sistem vital mereka. Dengan strategi yang matang, mereka bersiap menghadapi serangan dan mempertahankan wilayah masing-masing.

Bangsa Marley, dipimpin oleh Zeke, telah mempersiapkan Annie, Bertholdt, dan Reiner untuk menyerang menggunakan Laravel Worker. Di sisi lain, Klan Eldia dari Paradis telah mempersiapkan Armin, Eren, dan Mikasa sebagai PHP Worker untuk mempertahankan pulau tersebut. Warhammer bertindak sebagai Database Server, sementara Beast dan Colossal sebagai Load Balancer. 


## Konfigurasi

### Paradis
```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.235.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.235.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.235.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.235.4.1
	netmask 255.255.255.0
```
### Annie
```bash
auto eth0
iface eth0 inet static
	address 192.235.1.2
	netmask 255.255.255.0
	gateway 192.235.1.1
```
### Berthold
```bash
auto eth0
iface eth0 inet static
	address 192.235.1.3
	netmask 255.255.255.0
	gateway 192.235.1.1
```
### Reiner
```bash
auto eth0
iface eth0 inet static
	address 192.235.1.4
	netmask 255.255.255.0
	gateway 192.235.1.1
```
### Armin
```bash
auto eth0
iface eth0 inet static
	address 192.235.2.2
	netmask 255.255.255.0
	gateway 192.235.2.1
```
### Eren
```bash
auto eth0
iface eth0 inet static
	address 192.235.2.3
	netmask 255.255.255.0
	gateway 192.235.2.1
```
### Mikasa
```bash
auto eth0
iface eth0 inet static
	address 192.235.2.4
	netmask 255.255.255.0
	gateway 192.235.2.1
```
### Beast
```bash
auto eth0
iface eth0 inet static
	address 192.235.3.2
	netmask 255.255.255.0
	gateway 192.235.3.1
```
### Colossal
```bash
auto eth0
iface eth0 inet static
	address 192.235.3.3
	netmask 255.255.255.0
	gateway 192.235.3.1
```
### Warhammer
```bash
auto eth0
iface eth0 inet static
	address 192.235.3.4
	netmask 255.255.255.0
	gateway 192.235.3.1
```
### Fritz
```bash
auto eth0
iface eth0 inet static
	address 192.235.4.2
	netmask 255.255.255.0
	gateway 192.235.4.1
```
### Tybur
```bash
auto eth0
iface eth0 inet static
	address 192.235.4.3
	netmask 255.255.255.0
	gateway 192.235.4.1
```
### Zeke & Erwin
```bash
auto eth0
iface eth0 inet dhcp
```

## Depedency

### Paradis (DHCP Relay)
```bash
apt-get update
apt-get install isc-dhcp-relay -y

iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.235.0.0/16
```
### Tybur (DHCP Server)
```bash
auapt-get update
apt-get install isc-dhcp-server -y
```
### Fritz (DNS Server)
```bash
apt-get update
apt-get install bind9 -y
```
### Beast & Colossal (Load Balancer)
```bash
apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y
```
### Warhammer (Database Server)
```bash
apt-get update
apt-get install mariadb-server -y
```
### Annie, Bertholdt, Reiner (Laravel Worker)
```bash
apt-get update
apt-get install mariadb-server -y
apt-get install apache2-utils -y
apt-get install lynx -y
apt-get install htop -y
apt-get install jq -y
```
### Armin, Eren, Mikasa (PHP Worker)
```bash
apt-get update
apt-get install mariadb-server -y
apt-get install apache2-utils -y
apt-get install lynx -y
apt-get install htop -y
apt-get install jq -y
apt-get install wget -y
apt-get install unzip -y
apt-get install nginx -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y

service nginx start
service php7.3-fpm start
```
### Zeke dan Erwin (Client)
```bash
apt-get update
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install jq -y
```
## No.0
Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name marley.yyy.com untuk worker Laravel mengarah pada Annie. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name eldia.yyy.com untuk worker PHP (0) mengarah pada Armin.

#### Konfigurasi pada Fritz(DNS Server) untuk membuat domain Marley(Annie) dan Eldia(Armin)
`Fritz - script0.sh`
```
apt-get update
apt-get install bind9 -y

forward="options {
directory \"/var/cache/bind\";
forwarders {
  	   192.168.122.1;
};

allow-query{any;};
listen-on-v6 { any; };
};
"
echo "$forward" > /etc/bind/named.conf.options

echo "zone \"marley.it37.com\" {
	type master;
	file \"/etc/bind/jarkom/marley.it37.com\";
};

zone \"eldia.it37.com\" {
	type master;
	file \"/etc/bind/jarkom/eldia.it37.com\";
};
" > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

riegel="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    marley.it37.com. root.marley.it37.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    marley.it37.com.
@       IN    A    192.235.1.2
"
echo "$riegel" > /etc/bind/jarkom/marley.it37.com

granz="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    eldia.it37.com. root.eldia.it37.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    eldia.it37.com.
@       IN    A    192.235.2.2
"
echo "$granz" > /etc/bind/jarkom/eldia.it37.com

service bind9 restart
```


## No.1-5
Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.(1)
Jauh sebelum perang dimulai, ternyata para keluarga bangsawan, Tybur dan Fritz, telah membuat kesepakatan sebagai berikut:
  1. Semua Client harus menggunakan konfigurasi ip address dari keluarga Tybur (dhcp).
  2. Client yang melalui bangsa marley mendapatkan range IP dari [prefix IP].1.05 - [prefix IP].1.25 dan [prefix IP].1.50 - [prefix IP].1.100 (2)
  3. Client yang melalui bangsa eldia mendapatkan range IP dari [prefix IP].2.09 - [prefix IP].2.27 dan [prefix IP].2 .81 - [prefix IP].2.243 (3)
  4. Client mendapatkan DNS dari keluarga Fritz dan dapat terhubung dengan internet melalui DNS tersebut (4)
  5. Dikarenakan keluarga Tybur tidak menyukai kaum eldia, maka mereka hanya meminjamkan ip address ke kaum eldia selama 6 menit. Namun untuk kaum marley, keluarga Tybur meminjamkan ip address selama 
     30 menit. Waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit. (5)
 
#### Menghubungkan DHCP Relay dengan DHCP Server
`Paradis - script1.sh`
```
apt-get update
apt install isc-dhcp-relay -y

service isc-dhcp-relay start 

echo '# Defaults for isc-dhcp-relay initscript

# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.235.4.3" 

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay


echo net.ipv4.ip_forward=1 > /etc/sysctl.conf

service isc-dhcp-relay restart
```

`Tybur - script1.sh`
```
apt-get update
apt-get install isc-dhcp-server -y

echo 'INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

subnet="option domain-name \"example.org\";
option domain-name-servers ns1.example.org, ns2.example.org;

default-lease-time 600;
max-lease-time 7200;

ddns-update-style-none;

subnet 192.235.1.0 netmask 255.255.255.0 {
    range 192.235.1.5 192.235.1.25;
    range 192.235.1.50 192.235.1.1920;
    option routers 192.235.1.1;
    option broadcast-address 192.235.1.255;
    option domain-name-servers 192.235.4.2;
    default-lease-time 360;
    max-lease-time 5220;
}

subnet 192.235.2.0 netmask 255.255.255.0 {
    range 192.235.2.9 192.235.2.27;
    range 192.235.2.81 192.235.2.243;
    option routers 192.235.2.1;
    option broadcast-address 192.235.2.255;
    option domain-name-servers 192.235.4.2;
    default-lease-time 1800;
    max-lease-time 5220;
}

subnet 192.235.3.0 netmask 255.255.255.0 {
}

subnet 192.235.4.0 netmask 255.255.255.0 {
}

"
echo "$subnet" > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/1.1.png)
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/1.2.png)
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/1.3.png)
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/1.4.png)
## No.6
Armin berinisiasi untuk memerintahkan setiap worker PHP untuk melakukan konfigurasi virtual host untuk website berikut https://intip.in/BangsaEldia dengan menggunakan php 7.3 (6)

#### Menambahkan nameserver IP DHCP Server, menginstall PHP, mendownload kebutuhan untuk PHP di link yang disediakan, mengekstrak, memindah dan melakukan symlink pada file yang sudah di download, juga mengatur konfigurasi Nginx
`script untuk php worker (Armin,Eren,Mikasa) - script6.sh`
```
#!/bin/bash

echo nameserver 192.235.4.3 >> /etc/resolv.conf

apt-get update

apt-get install nginx -y
apt-get install lynx -y
apt-get install php7.3 php7.3-fpm php7.3-mysql -y   # Install PHP 7.3 dan modul yang diperlukan
apt-get install wget -y
apt-get install unzip -y
apt-get install rsync -y    # Install rsync untuk transfer file
service nginx start
service php7.3-fpm start    # Jalankan PHP-FPM versi 7.3

# Download file modul-3.zip dari GDrive
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1ufulgiWyTbOXQcow11JkXG7safgLq1y-' -O '/var/www/modul-3.zip'

# Ekstrak file zip
unzip -o /var/www/modul-3.zip -d /var/www/
rm /var/www/modul-3.zip

# Pindahkan isi dari folder modul-3 ke marley.it37.com menggunakan rsync
rsync -av /var/www/modul-3/ /var/www/marley.it37.com/

# Hapus folder modul-3 yang kosong
rm -r /var/www/modul-3

# Konfigurasi Nginx
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/marley.it37.com

# Periksa apakah symbolic link sudah ada
if [ -L /etc/nginx/sites-enabled/marley.it37.com ]; then
    rm /etc/nginx/sites-enabled/marley.it37.com
fi

# Buat symbolic link baru
ln -s /etc/nginx/sites-available/marley.it37.com /etc/nginx/sites-enabled/

# Periksa apakah default sudah ada
if [ -L /etc/nginx/sites-enabled/default ]; then
    rm /etc/nginx/sites-enabled/default
fi

# Membuat konfigurasi Nginx untuk marley.it37.com
echo 'server {
     listen 80;
     server_name _;

     root /var/www/marley.it37.com/;
     index index.php index.html index.htm;

     location / {
         try_files $uri $uri/ /index.php?$query_string;
     }

     location ~ \.php$ {
         include snippets/fastcgi-php.conf;
         fastcgi_pass unix:/run/php/php7.3-fpm.sock;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         include fastcgi_params;
     }
 }' > /etc/nginx/sites-available/marley.it37.com

service php7.3-fpm restart
service nginx restart
```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/6.1.png)

![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/6.2.png)

![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/6.3.png)

## No.7
Dikarenakan Armin sudah mendapatkan kekuatan titan colossal, maka bantulah kaum eldia menggunakan colossal agar dapat bekerja sama dengan baik. Kemudian lakukan testing dengan 6000 request dan 200 request/second. (7)

#### konfigurasi untuk nginx untuk Load Balancer 
`colossal - script7.sh`
```
#!/bin/bash

echo -e '
nameserver 192.235.4.3
nameserver 192.168.122.1
' > /etc/resolv.conf

# Update dan install paket 
apt-get update
apt-get install apache2-utils -y   # Untuk testing pake ab
apt-get install nginx -y           # Install Nginx sebagai load balancer
apt-get install lynx -y            # Install lynx untuk akses web via command line

# Konfigurasi Nginx untuk load balancing menggunakan PHP load balancer
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/colossal_lb

# Tambahkan konfigurasi load balancing
echo '
    upstream php_backend {
        server 192.235.2.2;  # Armin
        server 192.235.2.3;  # Eren
        server 192.235.2.4;  # Mikasa
    }

    server {
        listen 80;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        server_name _;

        location / {
            proxy_pass http://php_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
' > /etc/nginx/sites-available/colossal_lb

# Aktifkan konfigurasi load balancer
ln -sf /etc/nginx/sites-available/colossal_lb /etc/nginx/sites-enabled/

# Hapus default jika masih ada
if [ -f /etc/nginx/sites-enabled/default ]; then
    rm /etc/nginx/sites-enabled/default
fi

service nginx restart
```
#### test di client
```
ab -n 6000 -c 200 http://192.235.3.3/
```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/7.1.png)
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/7.2.png)


## No.8
Karena Erwin meminta “laporan kerja Armin”, maka dari itu buatlah analisis hasil testing dengan 1000 request dan 75 request/second untuk masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
  a. Nama Algoritma Load Balancer
  b. Report hasil testing pada Apache Benchmark
  c. Grafik request per second untuk masing masing algoritma. 
  d. Analisis (8)
#### konfigurasi untuk benchmarking load balancer
`colossal - script8.sh`
```
#!/bin/bash

# Konfigurasi Round Robin
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/round_robin

echo '
    upstream round-robin {
        server 192.235.2.2;
        server 192.235.2.3;
        server 192.235.2.4;
    }

    server {
        listen 81;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://round-robin;
        }
    }
' > /etc/nginx/sites-available/round_robin

# Konfigurasi Weighted Round Robin
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/weight_round_robin

echo '
    upstream weight_round-robin {
        server 192.235.2.2 weight=3;
        server 192.235.2.3 weight=2;
        server 192.235.2.4 weight=1;
    }

    server {
        listen 82;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://weight_round-robin;
        }
    }
' > /etc/nginx/sites-available/weight_round_robin

# Konfigurasi Generic Hash
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/generic_hash

echo '
    upstream generic_hash {
        hash $request_uri consistent;
        server 192.235.2.2 weight=3;
        server 192.235.2.3 weight=2;
        server 192.235.2.4 weight=1;
    }

    server {
        listen 83;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://generic_hash;
        }
    }
' > /etc/nginx/sites-available/generic_hash

# Konfigurasi IP Hash
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/ip_hash

echo '
    upstream ip_hash {
        ip_hash;
        server 192.235.2.2;
        server 192.235.2.3;
        server 192.235.2.4;
    }

    server {
        listen 84;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://ip_hash;
        }
    }
' > /etc/nginx/sites-available/ip_hash

# Konfigurasi Least Connections
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/least_connection

echo '
    upstream least_connection {
        least_conn;
        server 192.235.2.2;
        server 192.235.2.3;
        server 192.235.2.4;
    }

    server {
        listen 85;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://least_connection;
        }
    }
' > /etc/nginx/sites-available/least_connection

# Aktifkan semua konfigurasi load balancer
ln -sf /etc/nginx/sites-available/round_robin /etc/nginx/sites-enabled/
ln -sf /etc/nginx/sites-available/weight_round_robin /etc/nginx/sites-enabled/
ln -sf /etc/nginx/sites-available/generic_hash /etc/nginx/sites-enabled/
ln -sf /etc/nginx/sites-available/ip_hash /etc/nginx/sites-enabled/
ln -sf /etc/nginx/sites-available/least_connection /etc/nginx/sites-enabled/

service nginx restart
```
test masing masing benchmark load balancer

  - Round-Robin
  ```ab -n 1000 -c 75 http://192.235.3.3:81/```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.1.png)
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.2.png)

  - Weight Round-Robin
  ```ab -n 1000 -c 75 http://192.235.3.3:82/```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.3.png)
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.4.png)

  - Generic Hash
  ```ab -n 1000 -c 75 http://192.235.3.3:83/```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.5.png)
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.6.png)

  - IP Hash
  ```ab -n 1000 -c 75 http://192.235.3.3:84/```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.7.png)
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.8.png)

  - Least Connection
  ```ab -n 1000 -c 75 http://192.235.3.3:85/```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.9.png)
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/8.9.2.png)

#### Analisis
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/analisis8.png)


## No.9
Dengan menggunakan algoritma Least-Connection, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 1000 request dengan 10 request/second, kemudian tambahkan grafiknya pada “laporan kerja Armin”. (9)

#### Menggunakan script8.sh dari colossal, mengubah pada benchmark loadbalancer Least Connection berdasarkan jumlah worker yang dipakai dari 1worker - 2worker - 3worker
`colossal - script8.sh`
```
    upstream least_connection {
        least_conn;
        server 192.235.2.2;
        server 192.235.2.3;
        server 192.235.2.4;
        #menguncommand worker diatas
    }
```
#### test 
  - Least Connection (1 Worker)
  ```ab -n 1000 -c 75 http://192.235.3.3:85/```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/9.1.png)

  - Least Connection (2 Worker)
  ```ab -n 1000 -c 75 http://192.235.3.3:85/```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/9.2.png)

  - Least Connection (3 Worker)
  ```ab -n 1000 -c 75 http://192.235.3.3:85/```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/9.3.png)

#### Analisis
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/analisis9.png)


## No.10
Selanjutnya coba tambahkan keamanan dengan konfigurasi autentikasi di Colossal dengan dengan kombinasi username: “arminannie” dan password: “jrkmyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/supersecret/ (10)

#### Tetap menggunakan script8.sh di colossal dengan menambahkan konfigurasi seperti dibawah
`colossal - script10.sh`
```
# Membuat direktori untuk menyimpan file htpasswd
mkdir -p /etc/nginx/supersecret

# Membuat file htpasswd dengan username arminannie dan password jrkmit37
htpasswd -cb /etc/nginx/supersecret/htpasswd arminannie jrkmit37
```

## No.11
Lalu buat untuk setiap request yang mengandung /titan akan di proxy passing menuju halaman https://attackontitan.fandom.com/wiki/Attack_on_Titan_Wiki (11) 
hint: (proxy_pass)
#### Tetap menggunakan script10.sh di colossal dengan menambahkan konfigurasi seperti dibawah pada setiap benchmark load balancer
`colossal - script11.sh`
```
        location /titan {
            proxy_pass https://attackontitan.fandom.com/wiki/Attack_on_Titan_Wiki;
        }
```
![App Screenshot](https://github.com/masibelajar/Jarkom-Modul-3-IT37-2024/blob/main/img/11.png)
