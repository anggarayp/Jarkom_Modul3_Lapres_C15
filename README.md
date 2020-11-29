# Jarkom_Modul3_Lapres_C15
Lapres Modul 3 Jarkom

### Nama Anggota Kelompok :
### 1. Anggara Yuda Pratama (05111840000008)
### 2. Patrick (05111840000098)

![image](https://user-images.githubusercontent.com/61231385/100473189-5af25200-3110-11eb-8893-6b7712f26d48.png)

Anri adalah seorang mahasiswa tingkat akhir yang sedang mengerjakan TA mengenai DHCP dan Proxy. Bu Meguri sebagai dosen pembimbing Anri memberikan tugas pertamanya,

Keterangan :

- Jangan lupa membuat file bye.sh dengan cara : ```nano bye.sh```, yang isinya :
 
  ```
  uml_mconsole SURABAYA halt
  uml_mconsole MALANG halt
  uml_mconsole MOJOKERTO halt
  uml_mconsole TUBAN halt
  uml_mconsole SIDOARJO halt
  uml_mconsole GRESIK halt
  uml_mconsole BANYUWANGI halt
  uml_mconsole MADIUN halt
  ```
Anri sudah pernah mempelajari teknik Jaringan Komputer sehingga Anri dapat membuat topologi tersebut dengan mudah. Bu Meguri memerintahkan Anri untuk menjadikan **SURABAYA** sebagai router, **MALANG** sebagai DNS Server, **TUBAN** sebagai DHCP server, serta **MOJOKERTO** sebagai Proxy server, dan **UML lainnya** sebagai client. Bu Meguri berpesan pada Anri untuk menyusun topologi secara hati-hati dan memperhatikan gambar topologi yang diberikan Bu Meguri.

- Setting file interfaces dengan cara ```nano /etc/network/interfaces.conf``` masing-masing UML.

- **SURABAYA** sebagai router
  
  ```
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.76.66 //IP_eth0_SURABAYA_tiap_kelompok
  netmask 255.255.255.252
  gateway 10.151.76.65 //IP_tuntap_tiap_kelompok

  auto eth1
  iface eth1 inet static
  address 192.168.0.1 //IP_eth1_SURABAYA_tiap_kelompok
  netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
  address 192.168.1.1
  netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
  address 10.151.77.129 //NID_DMZ_tiap_kelompok + 1
  netmask 255.255.255.0
  ```
  
- **MALANG** sebagai DNS Server

  ```
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.77.130 //IP_MALANG_tiap_kelompok
  netmask 255.255.255.248
  gateway 10.151.77.129 //eth3 == eth1 lama
  ```

- **MOJOKERTO** sebagai Proxy Server

  ```
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.77.131 //IP_MOJOKERTO_tiap_kelompok
  netmask 255.255.255.248
  gateway 10.151.77.129 //IP_eth1_SURABAYA_tiap_kelompok
  ```

- **TUBAN** sebagai DHCP Server

  ```
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 10.151.77.132 //IP_TUBAN_tiap_kelompok
  netmask 255.255.255.248
  gateway 10.151.77.129 //IP_eth1_SURABAYA_tiap_kelompok
  ```

- **SIDOARJO**

  ```
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.0.2
  netmask 255.255.255.0
  gateway 192.168.0.1
  ```
  
- **GRESIK**

  ```
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.0.3
  netmask 255.255.255.0
  gateway 192.168.0.1
  ```
  
- **BANYUWANGI**

  ```
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.1.3 //eth2+2
  netmask 255.255.255.0
  gateway 192.168.1.1 //eth2_sesuai_soal
  ```
  
- **MADIUN**

  ```
  auto lo
  iface lo inet loopback

  auto eth0
  iface eth0 inet static
  address 192.168.1.2 //eth2+1
  netmask 255.255.255.0
  gateway 192.168.1.1 //eth2
  ```
  
### SOAL NO 1

- Mengisi file topologi.sh dengan cara : ```nano topologi.sh```

- Lalu isi topologi.sh dengan :
  ```
  # Switch
  uml_switch -unix switch1 > /dev/null < /dev/null &
  uml_switch -unix switch2 > /dev/null < /dev/null &
  uml_switch -unix switch3 > /dev/null < /dev/null &

  # Router
  xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.76.65 eth1=daemon,,,switch1 eth2=daemon,,,switch3 eth3=daemon,,,switch3 mem=256M &

  # Server switch 2
  xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=168M &
  xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
  xterm -T TUBAN -e linux ubd0=TUBAN,jarkom umid=TUBAN eth0=daemon,,,switch2 mem=128M &

  # Klien switch 1
  xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=64M &
  xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=64M &
  
  #Klien switch 3
  xterm -T BANYUWANGI -e linux ubd0=BANYUWANGI,jarkom umid=BANYUWANGI eth0=daemon,,,switch3 mem=64M &
  xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch3 mem=64M &
  ```
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/topologi.sh.png)

### SOAL NO 2

-- SURABAYA

  - ```nano /etc/sysctl.conf``` lalu hilangkan tanda pagar (#) pada bagian ```net.ipv4.ip_forward=1```
  
  - Install dchp relay : ```apt-get install isc-dhcp-relay -y```
  
  - Edit agar SURABAYA bisa melakukan relay dari server TUBAN ```nano /etc/default/isc-dhcp-relay``` 
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/2.PNG)

-- TUBAN

  - Install dhcp server : ```apt-get install isc-dhcp-server -y```
  
  - Edit file server :```nano /etc/default/isc-dhcp-server``` 
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/2a.PNG)
  
  - Restart dhcp server : ```service isc-dhcp-server restart```

### SOAL NO 3 4 5 6
  
  Setting subnet dhcp file **dhcp.conf** dengan cara ```nano /etc/dhcp/dhcpd.conf``` di UML Tuban, lalu ubah isinya sesuai dengan range di soal.
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/3.png)
    
  ```
  subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.10 192.168.0.100;
    range 192.168.0.110 192.168.0.200;
    option routers 10.151.77.129;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.77.130, 202.46.129.2;
    default-lease-time 300;
    max-lease-time 600;
 }

 subnet 192.168.1.0 netmask 255.255.255.0 {
     range 192.168.1.50 192.168.1.70;
     option routers 10.151.77.129;
     option broadcast-address 192.168.1.255;
     option domain-name-servers 10.151.77.130, 202.46.129.2;
     default-lease-time 600;
     max-lease-time 1200;
 }
 ```
 
 ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/5a.png)
 
 Lalu ```cat /etc/resolv.conf``` untuk mengecek apakah sudah sesuai konfigurasi di DHCP.
 
 ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/5b.png)
 
### SOAL NO 7
 Akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. User autentikasi milik Anri memiliki format:
 User : userta_yyy
 Password : inipassw0rdta_yyy
 Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01

-- MOJOKERTO

  - Install squid : ```apt-get install squid -y```
  
  - Install apache2-utils : ```apt-get install apache2-utils```
  
  - Setting Proxy pada setting Windows/ browser (Firefox/Chrome)
  
  Dengan memasukkan ip Mojokerto : ```10.151.77.131```
  
  - Buat user dan password : ``` htpasswd -c /etc/squid/passwd userta_c15```
  
  - Ketik password (inipassw0rdta_c15)
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/7b.png)
  
  - Backup file konfigurasi squid :```mv /etc/squid/squid.conf /etc/squid/squid.conf.bak```
  
  - Buat file konfigurasi squid baru : ```nano /etc/squid/squid.conf```
  
  - Masukkan konfigurasi berikut ke squid.conf 
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/7.png)
  
  - Restart squid : ```service squid restart```
  
  - Hasilnya akan seperti ini ketika proxy menggunakan MOJOKERTO sebagai host
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/7c.png)
  
  
### SOAL NO 8
-- MOJOKERTO
  
  - Buat file konfigurasi : ```nano /etc/squid/acl.conf```
  
  - Edit file konfigurasi tersebut
  
  ```
  acl AVAILABLE_WORKING time TW 13:00-19:00
  ```
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/8%209.png)
  
  - Edit file konfigurasi squid : ```nano /etc/squid/squid.conf```
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/8a.PNG)
  
  - Restart squid : ```service squid restart```
  
  - Hasilnya akan seperti ini ketika mencoba akses internet diluar Selasa jam 13.00 - 18.00
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/8.png)
  
### SOAL NO 9
-- MOJOKERTO
  
  - Buat file konfigurasi : ```nano /etc/squid/acl.conf```
  
  - Edit file konfigurasi tersebut
  
  ```
  acl BIMBINGAN_NIGHT_TIME time TWH 21:00-23:59
  acl BIMBINGAN_MORNING_TIME time WHF 00:00-09:00
  ```
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/8%209.png)
  
  - Edit file konfigurasi squid : ```nano /etc/squid/squid.conf```
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/8a.PNG)
  
  - Restart squid : ```service squid restart```
  
### SOAL NO 10
-- MOJOKERTO
  
  - Edit file konfigurasi squid : ```nano /etc/squid/squid.conf```
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/10.PNG)
  
  - Restart squid : ```service squid restart```
  
  - Tiap mencoba mengakses google akan diredirect ke monta.if.its.ac.id
  
### SOAL NO 11
-- MOJOKERTO
  
  - Pindah ke directory errors : ```cd /usr/share/squid/errors/en```
  
  - Download file error page : ```wget 10.151.36.202/ERR_ACCESS_DENIED```
  
  - Remove file error yang lama : ```rm ERR_ACCESS_DENIED```
  
  - Rename file error yang baru : ```mv ERR_ACCESSS_DENIED.1 ERR_ACCESSS_DENIED```
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/11a.PNG)
  
  - Restart squid : ```service squid restart```
  
  - Error page akan menjadi seperti berikut :
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/11.PNG)
  
### SOAL NO 12
-- MALANG
  
  - Install bind : ```apt-get install bind9 -y```
  
  - Edit file konfigurasi : ```nano /etc/bind/named.conf.local```
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/12b.PNG)
  
  - Buat folder dalam /etc/bind : ```mkdir /etc/bind/ta```
  
  - Copy file db.local ke /etc/bind/ta : ```cp /etc/bind/db.local /etc/bind/ta/janganlupa-ta.c15.pw```
  
  - Edit file janganlupa-ta.c15.pw : ```nano /etc/bind/ta/janganlupa-ta.c15.pw
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/12a.PNG)
  
  - Restart bind : ```service bind9 restart```
  
  - Ganti settingan proxy pada Windows atau pada browser (Firefox) dengan mengganti IP mojokerto dengan ```janganlupa-ta.c15.pw```
  
  - Ketika proxy menggunakan janganlupa-ta.c15.pw, hasilnya akan seperti ini :
  
  ![image](https://github.com/anggarayp/Jarkom_Modul3_Lapres_C15/blob/main/Screenshots/12.PNG)
  
