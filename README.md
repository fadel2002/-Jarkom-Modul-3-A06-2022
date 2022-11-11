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


## Nomor 7

### Soal

Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan
alamat IP yang tetap dengan IP [prefix IP].3.13 

### Cara Pengerjaan

Pada Eden, menjalaskan command ``` ip a ``` kemudian melihat ``` link/ether ``` pada eth0

```bash
link/ether 82:ca:27:cc:c1:74
```

Link/ether tersebut akan digunakan untuk membuat fixed address. Pada Westalis (DHCP Server) ditambahkan konfigurasi 
berikut pada file ``` /etc/dhcp/dhcpd.conf ```. Pada ``` hardware ethernet ``` isinya adalah nilai link/ether.

```bash
host Eden {
    hardware ethernet 82:ca:27:cc:c1:74;
    fixed-address 10.2.3.13;
}
```

Kemudian pada Eden, isi file ``` /etc/network/interfaces ``` diubah menjadi

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

Kemudian membuat acl waktu pada file ``` /etc/squid/acl.conf ``` dengan isi

```bash
acl AVAILABLE_WORKING time MTWHF 08:00-17:00
```

Kemudian membuat file ``` /etc/squid/sites.whitelist.working_hour.txt ``` yang berisi daftar whitelist situs yang bisa dibuka pada saat jam kerja yaitu

```bash
.loid-work.com
.franky-work.com
```

Kemudian isi dari file konfigurasi squid ``` /etc/squid/squid.conf ``` dibuat menjadi

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
