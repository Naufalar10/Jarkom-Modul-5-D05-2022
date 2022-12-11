
# Jarkom-Modul-5-D05-2022

# Lapres Jarkom Modul 5 Kelompok D05 #

| Nama Praktikan  | NRP Praktikan |
| ------------- | ------------- |
| Ichlasul Hasanat  | 5025201091  |
| Haidar Fico Ramadhan Aryputra | 5025201185  |
| Naufal Ariq Putra Yosyam | 5025201112 |

## Prefix IP
```192.187```
##  Konfigurasi VLSM dalam GNS3
![1.1](https://github.com/Naufalar10/Jarkom-Modul-5-D05-2022/blob/main/jarkom_5/clusteringVLSM.png)

Dari pengelompokan subnet tersebut didapatkan subnet terbesar memiliki /21 bit, sehingga pohon pembagian IP dapat dibuat menjadi sebagai berikut:
![1.2](https://github.com/Naufalar10/Jarkom-Modul-5-D05-2022/blob/main/jarkom_5/gambar_2.png)

Tabel Netmask dan Broadcast ID
![1.3](https://github.com/Naufalar10/Jarkom-Modul-5-D05-2022/blob/main/jarkom_5/gambar_3.png)
VLSM Routing - Setting Network Configuration
![1.4](https://github.com/Naufalar10/Jarkom-Modul-5-D05-2022/blob/main/jarkom_5/gambar_4.png)

## Strix
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
      address 192.187.7.146
      netmask 255.255.255.252

auto eth2
iface eth2 inet static
      address 192.187.7.149
      netmask 255.255.255.252
```
## Westalis
```
auto eth0
iface eth0 inet static
      address 192.187.7.145
      netmask 255.255.255.252
      gateway 192.187.7.146

auto eth1
iface eth1 inet static
      address 192.187.7.129
      netmask 255.255.255.248

auto eth2
iface eth2 inet static
      address 192.187.7.1
      netmask 255.255.255.128

auto eth3
iface eth3 inet static
      address 192.187.0.1
      netmask 255.255.252.0
 ```
## Ostania
```
auto eth0
iface eth0 inet static
      address 192.187.7.150
      netmask 255.255.255.252
      gateway 192.187.7.149

auto eth1
iface eth1 inet static
      address 192.187.6.1
      netmask 255.255.255.0

auto eth2
iface eth2 inet static
      address 192.187.4.1
      netmask 255.255.254.0

auto eth3
iface eth3 inet static
      address 192.187.7.137
      netmask 255.255.255.248
 ```     
## DNS SERVER: Eden
```
auto eth0
iface eth0 inet static
      address 192.187.7.130
      netmask 255.255.255.248
      gateway 192.187.7.129
```      
## DHCP SERVER: Wise
```
auto eth0
iface eth0 inet static
      address 192.187.7.131
      netmask 255.255.255.248
      gateway 192.187.7.129
```
## Web Server 1: SSS
```
auto eth0
iface eth0 inet static
      address 192.187.7.139
      netmask 255.255.255.248
      gateway 192.187.7.137
 ```
## Web Server 2: Garden
```
auto eth0
iface eth0 inet static
      address 192.187.7.138
      netmask 255.255.255.248
      gateway 192.187.7.137
 ```
## VLSM Routing  Setting Host Configuration
![1.5](https://github.com/Naufalar10/Jarkom-Modul-5-D05-2022/blob/main/jarkom_5/gambar_5.png)


Karena permintaan soal adalah client mendapatkan alamat IP dinamis dari DHCP Server, maka konfigurasi pada tiap client (Forger, Desmond, Briar, Blackbell) adalah sama

## Konfigurasi Client: Forger, Desmond, Briar, Blackbell
```
auto eth0
iface eth0 inet dhcp
```

### VLSM Routing - Routing
## STRIX
```
route add -net 192.187.7.128 netmask 255.255.255.248 gw 192.187.7.145
route add -net 192.187.7.128 netmask 255.2.255.128 gw 192.187.7.145
route add -net 192.187.0.0 netmask 255.255.252.0 gw 192.187.7.145
route add -net 192.187.6.0 netmask 255.255.255.0 gw 192.187.7.150 
route add -net 192.187.4.0 netmask 255.255.254.0 gw 192.187.7.150
route add -net 192.187.7.136 netmask 255.255.255.248 gw 192.187.7.150
```
## WESTALIS
```
route add -net 192.187.7.148 netmask 255.255.255.252 gw 192.187.7.146   #A5 
route add -net 192.187.6.0 netmask 255.255.255.0 gw 192.187.7.146       #A6 
route add -net 192.187.4.0 netmask 255.255.254.0 gw 192.187.7.146       #A7 
route add -net 192.187.7.136 netmask 255.255.255.248 gw 192.187.7.146   #A8 
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.187.7.146                 #default
```
## OSTANIA
```
route add -net 192.187.7.128 netmask 255.255.255.248 gw 192.187.7.149   #A1 
route add -net 192.187.7.0 netmask 255.255.255.128 gw 192.187.7.149     #A2 
route add -net 192.187.0.0 netmask 255.255.252.0 gw 192.187.7.149       #A3 
route add -net 192.187.7.144 netmask 255.255.255.252 gw 192.187.7.149   #A4 
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.187.7.149                 #default
```

## VLSM Routing - IP Tables The Resonance
Pada router Strix jalankan perintah berikut ini (belum menjawab soal 1):
```
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.187.0.0/21
```
## VLSM Routing - Setting resolv.conf
Pada semua node selain Strix (termasuk router-router lain), jalankan perintah berikut ini:
```
  echo nameserver 192.168.122.1 > /etc/resolv.conf
```
## Konfigurasi DNS Server pada Node Eden
Terletak pada script DNSServer.sh pada node eden. Script ini sudah berada di .bashrc sehingga tidak perlu dibash lagi
```
echo ' zone "eden.D05.com" {
        type master;
        file "/etc/bind/eden/eden.D05.com";
}; ' > /etc/bind/named.conf.local

mkdir /etc/bind/eden

cp /etc/bind/db.local /etc/bind/eden/eden.D05.com

echo -e '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eden.D09.com. root.eden.D05.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      eden.D05.com.
@       IN      A       192.187.7.130   ; IP Eden
@       IN      AAAA    ::1
' > /etc/bind/eden/eden.D05.com

service bind9 restart
```
## Konfigurasi DHCP Server pada Node Wise
Terletak pada script DHCPServer.sh pada node Wise. Script ini sudah berada di .bashrc sehingga tidak perlu dibash lagi

# Konfigurasi pada isc-dhcp-server
```
echo -e '
INTERFACES="eth0"
' > /etc/default/isc-dhcp-server
```
# Konfigurasi pada dhcpd.conf untuk IP Dinamis
```
echo -e 'subnet 192.187.7.0 netmask 255.255.255.128{
    range 192.187.7.2 192.187.7.126;
    option routers 192.187.7.1;
    option broadcast-address 192.187.7.127;
    option domain-name-servers 192.187.7.130;
}
subnet 192.187.0.0 netmask 255.255.252.0{
    range 192.187.0.2 192.187.3.254;
    option routers 192.187.0.1;
    option broadcast-address 192.187.3.255;
    option domain-name-servers 192.187.7.130;
}
subnet 192.187.7.128 netmask 255.255.255.248{
        option routers 192.187.7.129;
}
subnet 192.187.6.0 netmask 255.255.255.0{
        range 192.187.6.2 192.187.6.254;
        option routers 192.187.6.1;
        option broadcast-address 192.187.6.255;
        option broadcast-address 192.187.6.255;
        option domain-name-servers 192.187.7.130;
}
subnet 192.187.4.0 netmask 255.255.254.0{
        range 192.187.4.2 192.187.5.254;
        option routers 192.187.4.1;
        option broadcast-address 192.187.5.255;
        option domain-name-servers 192.187.7.130;
}
' > /etc/dhcp/dhcpd.conf
service isc-dhcp-server start
```
## Konfigurasi Web Server pada Node SSS
Terletak pada script WebServerSSS.sh pada node SSS. Script ini sudah berada di .bashrc sehingga tidak perlu dibash lagi
```
echo '
<?php
        phpinfo();
?>
' > /var/www/html/index.php
```
## Konfigurasi Web Server pada Node Garden
Terletak pada script WebServerGarden.sh pada node Garden. Script ini sudah berada di .bashrc sehingga tidak perlu dibash lagi
```
echo '
<?php
        phpinfo();
?>
' > /var/www/html/index.php
```
## Konfigurasi DHCP Relay pada Node Ostania
Terletak pada script DHCPRelayOstania.sh pada node Ostania. Script ini belum berada di .bashrc sehingga perlu dibash lagi.
```
echo -e '
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.187.7.131"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
' > /etc/default/isc-dhcp-relay
Setelah bash, maka DHCP Relay perlu direstart, lakukan command berikut pada node Ostania

service isc-dhcp-relay restart
Konfigurasi DHCP Relay pada Node Westalis
Terletak pada script DHCPRelayWestalis.sh pada node Westalis. Script ini belum berada di .bashrc sehingga perlu dibash lagi.

echo -e '
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.187.7.131"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
' > /etc/default/isc-dhcp-relay
Setelah bash, maka DHCP Relay perlu direstart, lakukan command berikut pada node Westalis

service isc-dhcp-relay restart
```
Setelah itu, restart tiap node client (Forger, Desmond, Briar, dan Blackbell) dan coba untuk ping google.com maupun antar node


## Nomor 1
**Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Strix menggunakan iptables, tetapi Loid tidak ingin menggunakan MASQUERADE.**
Untuk mengerjakan soal ini, kami menggunakan command ini pada node strix.
```
iptables -t nat -A POSTROUTING -s 192.187.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.2
```
TODO GANTI IP ADDRESS
## Nomor 2
**Kalian diminta untuk melakukan drop semua TCP dan UDP dari luar Topologi kalian pada server yang merupakan DHCP Server demi menjaga keamanan.**
Untuk mengerjakan soal ini,  kami akan men drop semua koneksi TCP dan UDP di luar jaringan kita. Command ini akan dijalankan pada node strix.
```
iptables -A FORWARD -p tcp -d 192.187.7.131 -i eth0 -j DROP # Drop semua TCP
iptables -A FORWARD -p udp -d 192.187.7.131 -i eth0 -j DROP # Drop semua UDP
```
TODO GANTI IP ADDRESS + BUKTI BERHASIL
## Nomor 3
**Loid meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 2 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.**
Untuk mengerjakan soal ini, kami akan mengset IPTABLES untuk hanya menerima 2 koneksi dalam satu waktu. Hal ini akan dijalankan pada DHCP server WISE dan DNS server Eden.
```
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -j DROP
```
TODO GANTI IP ADDRESS + BUKTI BERHASIL
## Nomor 4
**Akses menuju Web Server hanya diperbolehkan disaat jam kerja yaitu Senin sampai Jumat pada pukul 07.00 - 16.00.**
Untuk mengerjakan soal ini, kami akan mengatur Garden dan SSS dengan command sebagai berikut:
```
iptables -A INPUT -m time --timestart 07:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -j REJECT
```
TODO GANTI IP ADDRESS + BUKTI BERHASIL
## Nomor 5
**Karena kita memiliki 2 Web Server, Loid ingin Ostania diatur sehingga setiap request dari client yang mengakses Garden dengan port 80 akan didistribusikan secara bergantian pada SSS dan Garden secara berurutan dan request dari client yang mengakses SSS dengan port 443 akan didistribusikan secara bergantian pada Garden dan SSS secara berurutan.**
Untuk mengerjakan soal ini, kami akan menggunakan commands sebagai berikut pada SSS dan Garden:
```
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 192.187.7.138 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.187.7.138:80
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 192.187.7.138 -j DNAT --to-destination 192.187.7.139:80
iptables -A PREROUTING -t nat -p tcp --dport 443 -d 192.187.7.139 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.187.7.139:443
iptables -A PREROUTING -t nat -p tcp --dport 443 -d 192.187.7.139 -j DNAT --to-destination 192.187.7.138:443
```
TODO GANTI IP ADDRESS + BUKTI BERHASIL
## Nomor 6
**Karena Loid ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.**
Untuk mengerjakan soal ini, pertama kami akan me-restart DHCP server di WISE:
```
service isc-dhcp-server restart
service isc-dhcp-server restart
```
Kemudian, kami akan mengisi syslog dengan:
```
iptables -N LOGGING
iptables -A INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: "
iptables -A LOGGING -j DROP
```
Terakhir, kami akan mengisi syslog semua node:
```
iptables -A INPUT -m time --timestart 07:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT

iptables -N LOGGING
iptables -A INPUT -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Rejected: "
iptables -A LOGGING -j REJECT
```
TODO GANTI IP ADDRESS + BUKTI BERHASIL

