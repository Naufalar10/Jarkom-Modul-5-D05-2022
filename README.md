
# Jarkom-Modul-5-D05-2022

# Lapres Jarkom Modul 5 Kelompok D05 #

| Nama Praktikan  | NRP Praktikan |
| ------------- | ------------- |
| Ichlasul Hasanat  | 5025201091  |
| Haidar Fico Ramadhan Aryputra | 5025201185  |
| Naufal Ariq Putra Yosyam | 5025201112 |

## Prefix IP
????
##  Konfigurasi VLSM dalam GNS3
Ini merupakan perhitungan-perhitungan konfigurasi awal dalam VLSM:
![image](https://user-images.githubusercontent.com/7030663/206912273-feecf61c-96b3-49c5-9bf9-de2ee5ae37f9.png)<br>
![image](https://user-images.githubusercontent.com/7030663/206912281-b4546812-3edc-4ea3-8852-9d48dbfb95ff.png)<br>
![image](https://user-images.githubusercontent.com/7030663/206912286-1eef1255-3087-4c82-b119-0182dcd6d9e6.png)<br>
![image](https://user-images.githubusercontent.com/7030663/206912290-494b28bd-c8b0-4d75-9423-fd90081e79c4.png)<br>
Setelah konfigurasi awal ini, kami akan menjalankan ini pada Eden sebagai DNS server:
DNSServer.sh
```
echo ' zone "eden.D09.com" {
type master;
file "/etc/bind/eden/eden.D09.com";
}; ' > /etc/bind/named.conf.local
mkdir /etc/bind/eden
cp /etc/bind/db.local /etc/bind/eden/eden.D09.com
echo -e '
;
; BIND data file for local loopback interface
;
$TTL  604800
@  IN  SOA  eden.D09.com. root.eden.D09.com. (
2  ; Serial
604800  ; Refresh
86400  ; Retry
2419200  ; Expire
604800 )  ; Negative Cache TTL
;
@  IN  NS  eden.D09.com.
@  IN  A  192.189.7.130  ; IP Eden
@  IN  AAAA  ::1
' > /etc/bind/eden/eden.D09.com
service bind9 restart 
```
tambahan.sh
```
echo -e "
options {
directory "/var/cache/bind";
forwarders {
192.168.122.1;
};
// dnssec-validation auto;
allow-query{any;};
auth-nxdomain no;
listen-on-v6 { any; };
};
">/etc/bind/named.conf.options
```



## Nomor 1
**Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Strix menggunakan iptables, tetapi Loid tidak ingin menggunakan MASQUERADE.**
Untuk mengerjakan soal ini, kami menggunakan command ini pada node strix.
```
iptables -t nat -A POSTROUTING -s 192.189.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.2
```
TODO GANTI IP ADDRESS
## Nomor 2
**Kalian diminta untuk melakukan drop semua TCP dan UDP dari luar Topologi kalian pada server yang merupakan DHCP Server demi menjaga keamanan.**
Untuk mengerjakan soal ini,  kami akan men drop semua koneksi TCP dan UDP di luar jaringan kita. Command ini akan dijalankan pada node strix.
```
iptables -A FORWARD -p tcp -d 192.189.7.131 -i eth0 -j DROP # Drop semua TCP
iptables -A FORWARD -p udp -d 192.189.7.131 -i eth0 -j DROP # Drop semua UDP
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
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 192.189.7.138 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.189.7.138:80
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 192.189.7.138 -j DNAT --to-destination 192.189.7.139:80
iptables -A PREROUTING -t nat -p tcp --dport 443 -d 192.189.7.139 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.189.7.139:443
iptables -A PREROUTING -t nat -p tcp --dport 443 -d 192.189.7.139 -j DNAT --to-destination 192.189.7.138:443
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

