# Lapres Jarkom Kelompok A06

## Anggota Kelompok

1. Hans Sean Nathanael - 5025201019
2. Mohammad Fany Faizul Akbar - 5025201225
3. Fadel Pramaputra Maulana - 5025201260

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

## Nomor 1

### Soal

### Cara Pengerjaan

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

Melakukan testing

Pada SSS

![Dokumentasi 4-1](image/Nomor%204/1.png)

![Dokumentasi 4-2](image/Nomor%204/2.png)

Pada Garden

![Dokumentasi 4-3](image/Nomor%204/3.png)

![Dokumentasi 4-4](image/Nomor%204/4.png)

Pada Eden

![Dokumentasi 4-5](image/Nomor%204/5.png)

![Dokumentasi 4-6](image/Nomor%204/6.png)

Pada NewstonCastle

![Dokumentasi 4-7](image/Nomor%204/7.png)

![Dokumentasi 4-8](image/Nomor%204/8.png)

Pada KemonoPark

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

Tidak berhasil membatasi HTTPS.

![Dokumentasi squid 1-1](image/squid/nomor%201/nomor%201-1.png)

## Nomor 2

### Soal

Adapun pada hari dan jam kerja sesuai nomor (1), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP
tujuan domain dibebaskan)

### Cara Pengerjaan

Menginstall squid pada Berlint (Proxy Server)

```bash
apt-get update
apt-get install squid -y
```

Kemudian membuat acl waktu pada file `/etc/squid/acl.conf` dengan isi

```bash
acl AVAILABLE_WORKING time MTWHF 08:00-17:00
```

Kemudian membuat file `/etc/squid/sites.whitelist.working_hour.txt` yang berisi daftar whitelist situs yang bisa dibuka pada saat jam kerja yaitu

```bash
.loid-work.com
.franky-work.com
```

Kemudian isi dari file konfigurasi squid `/etc/squid/squid.conf` dibuat menjadi

```bash
include /etc/squid/acl.conf

acl WORKING_HOUR_WHITELIST dstdomain \"/etc/squid/sites.whitelist.working_hour.$

http_port 8080
visible_hostname Berlint

http_access allow WORKING_HOUR_WHITELIST AVAILABLE_WORKING
http_access deny all
```

![Dokumentasi squid 2-1](image/squid/nomor%202/nomor%202-1.png)

![Dokumentasi squid 2-2](image/squid/nomor%202/nomor%202-2.png)

![Dokumentasi squid 2-3](image/squid/nomor%202/nomor%202-3.png)

![Dokumentasi squid 2-4](image/squid/nomor%202/nomor%202-4.png)

## Nomor 3

### Soal

Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)

### Cara Pengerjaan

Pada file konfigurasi squid ditambahkan konfigurasi

```bash
http_access deny all
```

![Dokumentasi squid 3-1](image/squid/nomor%203/nomor%203-1.png)

## Nomor 4

### Soal

Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit
per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan
speed maksimal yaitu 128 Kbps)

### Cara Pengerjaan

Tidak berhasil mengatur bandwidth.

## Nomor 5

### Soal

Setelah diterapkan, ternyata peraturan nomor (4) mengganggu produktifitas saat hari kerja, dengan demikian pembatasan
kecepatan hanya diberlakukan untuk pengaksesan internet pada hari libur

### Cara Pengerjaan

Tidak berhasil mengatur bandwidth pada waktu tertentu.

## Dokumentasi

![Dokumentasi 1](image/dokumentasi%201.png)

![Dokumentasi 2](image/dokumentasi%202.png)
