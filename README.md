# Jarkom_Modul3_Lapres_T13

* Anis Saidatur Rochma 05311840000002
* Desya Ananda Puspita Dewi 05311840000046
---

### Soal 1
Setting UML agar memiliki topologi seperti berikut

![](/img/topo.jpg)
  
  - Setting router SURABAYA pada `/etc/sysctl.conf `lalu un-comment `net.ipv4.ip_forward` kemudian diaktifkan dengan command sysctl -p
  - Jalankan `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16` pada uml SURABAYA
  - Kemudian tambahkan interfaces di tiap uml dengan `nano /etc/network/interfaces` seperti berikut
  - uml SURABAYA
  ```
  // SURABAYA (Sebagai Router)
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.76.78
  netmask 255.255.255.252
  gateway 10.151.76.77

  auto eth1
  iface eth1 inet static
  address 192.168.0.1
  netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
  address 192.168.1.1
  netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
  address 10.151.77.153
  netmask 255.255.255.248
  ```
  - uml MALANG
  ```
  // MALANG (Sebagai DNS Server)
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.77.154
  netmask 255.255.255.248
  gateway 10.151.77.153
  ```
  - uml MOJOKERTO
  ```
  // MOJOKERTO (Sebagai Proxy server)
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.77.155
  netmask 255.255.255.248
  gateway 10.151.77.153
  ```
  - uml TUBAN
  ```
  // TUBAN (Sebagai DHCP Server)
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.77.156
  netmask 255.255.255.248
  gateway 10.151.77.153
  ```
  - uml SIDOARJO
  ```
  // SIDOARJO (Sebagai Klien)
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet dhcp
  #iface eth0 inet static
  #address 192.168.0.2
  #netmask 255.255.255.0
  #gateway 192.168.0.1
  ```
  - uml GRESIK
  ```
  // GRESIK (Sebagai Klien)
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet dhcp
  #iface eth0 inet static
  #address 192.168.0.3
  #netmask 255.255.255.0
  #gateway 192.168.0.1
  ```
  - uml BANYUWANGI
  ```
  // BANYUWANGI (Sebagai klien)  
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet dhcp
  #iface eth0 inet static
  #address 192.168.0.4
  #netmask 255.255.255.0
  #gateway 192.168.0.1
  ```
  - uml MADIUN
  ```
  // MADIUN (Sebagai klien)
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet dhcp
  #iface eth0 inet static
  #address 192.168.0.5
  #netmask 255.255.255.0
  #gateway 192.168.0.1
  ```
  - Lakukan `service networking service` pada setiap uml
  - Jalankan `apt-get update` pada uml  
  
### Soal 2
Setting Router **SURABAYA** agar menjadi *DHCP relay* antar DHCP server dan client.
  - Install `isc-dhcp-relay` pada uml SURABAYA dengan command `apt-get install isc-dhcp-relay`
  - Setting file `/etc/default/isc-dhcp-relay` seperti berikut
  
  ![](/img/2-2.jpg)

  - Jalankan `service isc-dhcp-relay restart` untuk menjalankan dhcp-relay
  
### Soal 3
Setting DHCP server *(TUBAN)* agar *subnet 1* mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.
  - Install `isc-dhcp-server` pada uml TUBAN dengan command `apt-get install isc-dhcp-server`
  - Setting file `/etc/default/isc-dhcp-server` seperti berikut
  
  ![](/img/3-1.jpg)

  - Jalankan `service isc-dhcp-server restart` untuk menjalankan dhcp-server
  - Edit file `/etc/dhcp/dhcpd.conf` dengan menambahkan subnet 1 dengan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.
  
  ```
  subnet 10.151.77.152 netmask 255.255.255.248 {
} 

subnet 192.168.0.0 netmask 255.255.255.0{
        range 192.168.0.10 192.168.0.100;     // ip 192.168.0.10 hingga 192.168.0.100
        range 192.168.0.110 192.168.0.200;    // ip 192.168.0.110 hingga 192.168.0.200
        option routers 192.168.0.1;
        option broadcast-address 192.168.0.255;
        option domain-name-servers 10.151.77.154, 202.46.129.2;
        default-lease-time 300;
        max-lease-time 7200;
}
  ```
  ![](/img/3-2.jpg)
  
  - Jalankan `service isc-dhcp-server restart` untuk menjalankan dhcp-server
  - Periksa uml GRESIK dan SIDOARJO yang memiliki IP tersebut
  
  ![](/img/gresik.jpg)
  ![](/img/sidoarjo.jpg)

### Soal 4
Setting DHCP server **(TUBAN)** agar *subnet 3* mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.
  - Edit file `/etc/dhcp/dhcpd.conf` dengan menambahkan subnet 3 dengan range IP dari 192.168.1.50 sampai 192.168.1.70
  
  ```
  subnet 192.168.1.0 netmask 255.255.255.0{
        range 192.168.1.50 192.168.1.70;     // ip 192.168.1.50 hingga 192.168.1.70
        option routers 192.168.0.1;
        option broadcast-address 192.168.0.255;
        option domain-name-servers 10.151.77.154, 202.46.129.2;
        default-lease-time 600;
        max-lease-time 7200;
}
  ```
  ![](/img/4-1.jpg)
  
  - Jalankan `service isc-dhcp-server restart` untuk menjalankan dhcp-server
  - Periksa uml BANYUWANGI dan MADIUN yang memiliki IP tersebut
  
  ![](/img/banyuwangi.jpg)
  ![](/img/madiun.jpg)

### Soal 5
Setting DHCP server **(TUBAN)** agar client mendapatkan *DNS MALANG* dan *DNS 202.46.129.2* dari DHCP
  - Edit file `/etc/dhcp/dhcpd.conf` dan pada bagian option domain-name-servers tambahkan `,` antar IP
  ```
   option domain-name-servers 10.151.77.154, 202.46.129.2;
  ```  
  - Jalankan `service isc-dhcp-server restart` untuk menjalankan dhcp-server

### Soal 6
Setting DHCP server **(TUBAN)** agar *subnet 1* mendapatkan peminjaman alamat IP selama 5 menit dan subnet 3 mendapatkan peminjaman alamat IP selama 10 menit.
  - Edit file `/etc/dhcp/dhcpd.conf` dan pada bagian default-lease-time 
  - Pada subnet 1 menggunakan lease time 300 yang sama dengan 5 menit
  ```
    default-lease-time 300;
  ```  
  - Pada subnet 3 menggunakan lease time 600 yang sama dengan 10 menit
  ```
    default-lease-time 600;
  ```  
  - Jalankan `service isc-dhcp-server restart` untuk menjalankan dhcp-server

### Soal 7
Setting agar akses Proxy server **(MOJOKERTO)** memerlukan autentikasi dengan user: userta_t03 dan password: inipassw0rdta_t03
  - Install proxy  squid dengan menggunakan command `apt-get install squid`
  - Cek status squid dengan command `service squid status`
  - Edit file `/etc/squid/squid.conf` dan tambahkan
  ```
  http_port 8080
  visible_hostname mojokerto

  http_Access allow all
  ```
  - Jalankan `service squid restart` untuk menjalankan squid
  - Lakukan command `htpasswd -c /etc/squid/passwd [ nama user]` dan masukkan passwordnya 
  ![](/img/7-1.jpg)
  
  - Edit file `/etc/squid/squid.conf` dan tambahkan
  ```
  http_port 8080
  visible_hostname mojokerto

  auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
  auth_param basic children 5
  auth_param basic realm Proxy
  auth_param basic credentialsttl 2 hours
  auth_param basic casesensitive on
  acl USERS proxy_auth REQUIRED
  http_access allow USERS
  ```
  ![](/img/7-2.jpg)
  
  - Jalankan `service squid restart` untuk menjalankan squid
  - Aktifkan proxy melalui private browser, dan lakukan setting proxy
  ![](/img/proxy.jpg)
  
  - Masukkan nama username dan password
  ![](/img/7-3.jpg)
  
  - Proxy berhasil dojalankan dengan cara membuka sebuah laman
  ![](/img/7-4.jpg)

### Soal 8
Setting agar Proxy server **(MOJOKERTO)** hanya dapat diakses pada hari Selasa-Rabu pukul 13.00-18.00.
  - Buat file baru `/etc/squid/acl.conf` dan tambahkan waktu sesuai ketentuan, yaitu:
    - MTWHF adalah hari-hari dimana user diperbolehkan menggunakan proxy. (S: Sunday, M: Monday, T: Tuesday, W: Wednesday, H: Thursday, F: Friday, A: Saturday)
    - Penulisan jam menggunakan format: h1:m1-h2:m2. Dengan syarat h1<h2 dan m1<m2
  ```
  acl AVAILABLE_WORKING1 time TW 13:00-18:00
  ```
  ![](/img/8-1.jpg)
  
  - Edit file `/etc/squid/squid.conf` dan tambahkan
  ```
  http_port 8080
  visible_hostname mojokerto
  
  include /etc/squid/acl.conf
  http_access allow AVAILABLE_WORKING1
  ```
  - Jalankan `service squid restart` untuk menjalankan squid

### Soal 9
Setting agar Proxy server **(MOJOKERTO)** hanya dapat diakses pada hari Selasa-Kamis pukul 21.00-09.00 keesokan harinya.
  - Edit file `/etc/squid/acl.conf` dan tambahkan ketentuan baru 
  ```
  acl AVAILABLE_WORKING2_1 time TWH 21:00-24:00
  acl AVAILABLE_WORKING2_2 time WHF 00:00-09:00
  ```
  ![](/img/9-1.jpg)
  
  - Edit file `/etc/squid/squid.conf` dan tambahkan
  ```
  http_port 8080
  visible_hostname mojokerto
  
  include /etc/squid/acl.conf
  http_access allow AVAILABLE_WORKING1
  http_access allow AVAILABLE_WORKING2_1
  http_access allow AVAILABLE_WORKING2_2
  ```
  - Jalankan `service squid restart` untuk menjalankan squid

### Soal 10
Setting Proxy server **(MOJOKERTO)** agar ketika mengakses google.com, maka akan diredirect menuju monta.if.its.ac.id.
  - Edit file `/etc/squid/squid.conf` dan tambahkan
  ```
  http_port 8080
  visible_hostname mojokerto

  acl BLACKLIST dstdomain google.com               //akan mem-block situs google.com
  deny_info http://monta.if.its.ac.id/ BLACKLIST   //situs yang di-block akan di direct ke situs monta.if
  http_reply_access deny BLACKLIST
  ```
  ![](/img/10-1.jpg)
  
  - Jalankan `service squid restart` untuk menjalankan squid
  - Buka private browser dengan proxy yang sudah dijalankan, lalu tes dengan membuka google.com  
  ![](/img/10-2.jpg)
  
  - Situs google langsung otomatis berpindah ke situs monta.if.its.ac.id
  ![](/img/10-3.jpg)
  
### Soal 11
Setting Proxy server **(MOJOKERTO)** agar mengubah error page default squid menjadi halaman yang disediakan. File error page bisa diunduh dengan cara wget 10.151.36.202/error403.tar.gz
Extract : tar -xvf error403.tar.gz
  - File yang sudah di extract, dipindah ke file error yang berada di dalam squid dengan cara
  ```
  cp -r ERR_ACCESS_DENIED /usr/share/squid/errors/English
  ```
  - Edit file `/etc/squid/squid.conf` dengan menambahkan `http_access deny all`
  
  ![](/img/11-1.jpg)
  
  - Jalan kan proxy dan buka web monta.its.ac.id yang sebelumnya lalu refresh
  
  ![](/img/error.jpg)

### Soal 12
Setting DNS server **(MALANG)** agar mengakses Proxy server **(MOJOKERTO)** dapat melalui domain janganlupa-ta.t03.pw dengan port 8080.
  - Install ` apt-get install bind9 -y` pada uml MALANG untuk setting DNS
  - Edit file `/etc/bind/named.conf.local` dan tambahkan zone
  
  ![](/img/12-1.jpg)
  
  - Lakukan command `mkdir /etc/bind/jarkom` dan `cp /etc/bind/jarkom/db.local /etc/bind/jarkom/janganlupa-ta.t13.pw`
  - Edit file `/etc/bind/jarkom/janganlupa-ta.t13.pw` seperti berikut
  
  ![](/img/12-2.jpg)
  
  - Jalankan `service bind9 restart`
  - Aktifkan proxy dengan menggunakan alamt `janganlupa-ta.t13.pw` dan jalankan proxy
  
  ![](/img/proxy.jpg)
  
