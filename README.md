# Lapres Jarkom Kelompok A06

## Anggota Kelompok

1. Hans Sean Nathanael - 5025201019
2. Mohammad Fany Faizul Akbar - 5025201225
3. Fadel Pramaputra Maulana - 5025201260

## Topologi

![Topologi](image/Nomor%201%20dan%202/2.png)

## Konfigurasi Node

### Ostania

```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.2.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.2.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.2.3.1
	netmask 255.255.255.0
```

### SSS

```bash
auto eth0
iface eth0 inet dhcp
```

### Garden

```bash
auto eth0
iface eth0 inet dhcp
```

### Eden

```bash
auto eth0
iface eth0 inet dhcp
hwaddress ether 82:ca:27:cc:c1:74
```

### NewstonCastle

```bash
auto eth0
iface eth0 inet dhcp
```

### KemonoPark

```bash
auto eth0
iface eth0 inet dhcp
```

### WISE

```bash
auto eth0
iface eth0 inet static
	address 10.2.2.2
	netmask 255.255.255.0
	gateway 10.2.2.1
```

### Berlint

```bash
auto eth0
iface eth0 inet static
	address 10.2.2.3
	netmask 255.255.255.0
	gateway 10.2.2.1
```

### Westalis

```bash
auto eth0
iface eth0 inet static
	address 10.2.2.4
	netmask 255.255.255.0
	gateway 10.2.2.1
```

## Nomor 1 dan 2

### Soal

![Soal 1 dan 2](image/Nomor%201%20dan%202/1.png)
Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server (1), dan Ostania sebagai DHCP Relay (2). Loid dan Franky menyusun peta tersebut dengan hati-hati dan teliti.
### Cara Pengerjaan

DNS Server (WISE)

WISE sebagai DNS Server perlu menginstall `bind9`.
```
apt-get update
apt-get install bind9 -y
```
DHCP Server (Westalis)

Westalis sebagai DHCP Server perlu menginstall `isc-dhcp-server`.

```
apt-get update
apt-get install isc-dhcp-server -y
```

Proxy Server (Berlint)

Berlint sebagai Proxy Server perlu menginstall `squid`.
```
apt-get update
apt-get install squid -y
```
DHCP Relay (Ostania)

Ostania sebagai DHCP Relay perlu menginstall `isc-dhcp-relay`.
```
apt-get update
apt-get install isc-dhcp-relay -y
```

## Nomor 3

### Soal

Ada beberapa kriteria yang ingin dibuat oleh Loid dan Franky, yaitu:
Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155!

### Cara Pengerjaan

Pada DHCP relay (Ostania) pada file `/etc/default/isc-dhcp-relay` diberikan konfigurasi sebagai berikut

```bash
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="10.2.2.4"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```

`SERVERS` merupakan IP dari DCHP server. `INTERFACES` merupakan interface yang terhubung dengan DHCP relay.

Pada DHCP server (Westalis) pada file `/etc/default/isc-dhcp-server` diberikan konfigurasi berikut

```bash
# Defaults for isc-dhcp-server initscript
# sourced by /etc/init.d/isc-dhcp-server
# installed at /etc/default/isc-dhcp-server by the maintainer scripts

#
# This is a POSIX shell fragment
#

# Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
#DHCPD_CONF=/etc/dhcp/dhcpd.conf

# Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
#DHCPD_PID=/var/run/dhcpd.pid

# Additional options to start dhcpd with.
#       Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
#OPTIONS=""

# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#       Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACES="eth0"
```

`INTERFACES` merupakan interface yang menhubungi DHCP relay dan DHCP server.

Pada DHCP server (Westalis) dilakukan konfigurasi server pada file `/etc/dhcp/dhcpd.conf` sebagai berikut

```bash
subnet 10.2.1.0 netmask 255.255.255.0 {
    range 10.2.1.50 10.2.1.88;
    range 10.2.1.120 10.2.1.155;
    option routers 10.2.1.1;
    option broadcast-address 10.2.1.255;
    option domain-name-servers 10.2.2.2;
    default-lease-time 300;
    max-lease-time 6900;
}

subnet 10.2.2.0 netmask 255.255.255.0 {
    range 10.2.2.5 10.2.2.254;
    option routers 10.2.2.1;
    option broadcast-address 10.2.2.255;
    option domain-name-servers 10.2.2.2;
    default-lease-time 600;
    max-lease-time 7200;
}
```

Pada subnet `10.2.1.0` sudah ditentukan range IP dari `10.2.1.50 - 10.2.1.88` dan `10.2.1.120 - 10.2.1.155`

## Nomor 4

### Soal

Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85!

### Cara Pengerjaan

Pada DHCP Server (Westalis) pada file `/etc/dhcp/dhcpd.conf` menambahkan konfigurasi sebagai berikut

```bash
subnet 10.2.3.0 netmask 255.255.255.0 {
    range 10.2.3.10 10.2.3.30;
    range 10.2.3.60 10.2.3.85;
    option routers 10.2.3.1;
    option broadcast-address 10.2.3.255;
    option domain-name-servers 10.2.2.2;
    default-lease-time 600;
    max-lease-time 6900;
}
```

Pada subnet `10.3.1.0` sudah ditentukan range IP dari `10.2.3.10 - 10.2.3.30` dan `10.2.3.60 - 10.2.3.85`

## Nomor 5

### Soal

Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.

### Cara Pengerjaan

Agar client dapat terhubung dengan internet maka pada DNS Server (WISE) pada file `/etc/bind/named.conf.options` mengubah konfigurasi menjadi seperti berikut

```bash
options {
        directory \"/var/cache/bind\";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

        forwarders {
                192.168.122.1;
        };

        allow-query{any;};

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        // dnssec-validation auto;

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```

### **Melakukan testing**

**Pada SSS**

![Dokumentasi 4-1](image/Nomor%204/1.png)

![Dokumentasi 4-2](image/Nomor%204/2.png)

**Pada Garden**

![Dokumentasi 4-3](image/Nomor%204/3.png)

![Dokumentasi 4-4](image/Nomor%204/4.png)

**Pada Eden**

![Dokumentasi 4-5](image/Nomor%204/5.png)

![Dokumentasi 4-6](image/Nomor%204/6.png)

**Pada NewstonCastle**

![Dokumentasi 4-7](image/Nomor%204/7.png)

![Dokumentasi 4-8](image/Nomor%204/8.png)

**Pada KemonoPark**

![Dokumentasi 4-9](image/Nomor%204/9.png)

![Dokumentasi 4-10](image/Nomor%204/10.png)

## Nomor 6

### Soal

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit!

### Cara Pengerjaan

Pada subnet interface switch 1 dan 3 ditambahkan konfigurasi berikut pada file `/etc/dhcp/dhcpd.conf`

```bash
subnet 10.2.1.0 netmask 255.255.255.0 {
	...
    default-lease-time 300;
    max-lease-time 6900;
}

subnet 10.2.3.0 netmask 255.255.255.0 {
	...
    default-lease-time 600;
    max-lease-time 6900;
}
```

## Nomor 7

### Soal

Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan
alamat IP yang tetap dengan IP [prefix IP].3.13

### Cara Pengerjaan

Pada Eden, menjalaskan command `ip a` kemudian melihat `link/ether` pada eth0

```bash
link/ether 82:ca:27:cc:c1:74
```

Link/ether tersebut akan digunakan untuk membuat fixed address. Pada Westalis (DHCP Server) ditambahkan konfigurasi
berikut pada file `/etc/dhcp/dhcpd.conf`. Pada `hardware ethernet` isinya adalah nilai link/ether.

```bash
host Eden {
    hardware ethernet 82:ca:27:cc:c1:74;
    fixed-address 10.2.3.13;
}
```

Kemudian pada Eden, isi file `/etc/network/interfaces` diubah menjadi

```bash
auto eth0
iface eth0 inet dhcp
hwaddress ether 82:ca:27:cc:c1:74
```

Kemudian pada Westalis service dhcp server direstart dan node Eden direstart.

![Dokumentasi 7-1](image/Nomor%207/nomor%207-1.png)

## Squid

## Nomor 1

### Soal

Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses
24 jam penuh)

### Cara Pengerjaan

Install squid pada Berlint (Proxy Server)

```bash
apt-get update
apt-get install squid -y
```

Kemudian konfigurasikan di `/etc/squid/squid.conf` `acl` dan `http_access` untuk hari dan jam kerja seperti sebagai berikut.
```
acl AVAILABLE_WORKING time MTWHF 08:00-17:00
acl WORKING_SITES dstdomain "/etc/squid/sites.whitelist.working_hour.txt"
http_access allow !WORKING_SITES !AVAILABLE_WORKING
``` 

Dalam `/etc/squid/sites.whitelist.working_hour.txt` diisi dengan situs yang hanya dapat diakses saat hari dan jam kerja.
```
.franky-work.com
.loid-work.com
```

**Jam Kerja**

![Dokumentasi squid 1-1](image/squid/nomor%201/1.png)

![Dokumentasi squid 1-2](image/squid/nomor%201/2.png)

**Tidak Jam Kerja**

![Dokumentasi squid 1-3](image/squid/nomor%201/3.png)

![Dokumentasi squid 1-4](image/squid/nomor%201/4.png)

## Nomor 2

### Soal

Adapun pada hari dan jam kerja sesuai nomor (1), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP
tujuan domain dibebaskan)

### Cara Pengerjaan

Tambahkan file konfigurasi squid berikut ke `/etc/squid/squid.conf` !

```bash
http_access allow WORKING_SITES AVAILABLE_WORKING
```

![Dokumentasi squid 2-1](image/squid/nomor%202/nomor%202-1.png)

![Dokumentasi squid 2-2](image/squid/nomor%202/nomor%202-2.png)

![Dokumentasi squid 2-3](image/squid/nomor%202/nomor%202-3.png)

![Dokumentasi squid 2-4](image/squid/nomor%202/nomor%202-4.png)

## Nomor 3

### Soal

Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)

### Cara Pengerjaan

Tambahkan acl untuk port HTTPS ke `/etc/squid/squid.conf`
```
acl ssl_ports port 443
```
Lalu ubah konfigurasi berikut.
```
http_access allow !WORKING_SITES !AVAILABLE_WORKING
```
Menjadi :
```bash
http_access allow ssl_ports !WORKING_SITES !AVAILABLE_WORKING
```

**HTTP**

![Dokumentasi squid 3-1](image/squid/nomor%203/nomor%203-1.png)

**HTTPS**

![Dokumentasi squid 3-2](image/squid/nomor%201/4.png)

## Nomor 4

### Soal

Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit
per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan
speed maksimal yaitu 128 Kbps)

### Cara Pengerjaan

Untuk membatasi akses internet dengan kecepatan maksimum 128 Kbps, maka `delay_parameters` diatur dengan nilai 128/8 Kbps menjadi `delay_parameters 1 16000/16000`. Isi file `/etc/squid/squid.conf` dengan konfigurasi berikut.
```
delay_pools 1
delay_class 1 1
# delay_access 1 allow all --> ini syntax untuk nomer 4 dimana tidak ada pembatasan hari
delay_access 1 allow WEEKEND
delay_access 1 deny all
delay_parameters 1 16000/16000
```

Untuk testing digabung dengan testing No.5.

## Nomor 5

### Soal

Setelah diterapkan, ternyata peraturan nomor (4) mengganggu produktifitas saat hari kerja, dengan demikian pembatasan
kecepatan hanya diberlakukan untuk pengaksesan internet pada hari libur

### Cara Pengerjaan

Sama seperti no.4, namun untuk delay access nya menggunakan acl `WEEKEND`. Untuk konfigurasi acl `WEEKEND` adalah sebagai berikut.
```
acl WEEKEND time AS 00:00-23:59
```
Untuk konfigurasi `squid` sudah sama dengan konfigurasi yang ada pada no.4.

**Testing Weekday**

![Dokumentasi 1 squid No 4 dan 5](image/squid/nomor%204%20dan%205/1.png)

**Testing Weekend**

![Dokumentasi 2 squid No 4 dan 5](image/squid/nomor%204%20dan%205/2.png)
## Dokumentasi

![Dokumentasi 1](image/dokumentasi%201.png)

![Dokumentasi 2](image/dokumentasi%202.png)

## Kendala

Tidak ada.
