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
  
### 1. Membuat topologi jaringan

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

### 2. SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client.

-- SURABAYA

  - ```nano /etc/sysctl.conf``` lalu hilangkan tanda pagar (#) pada bagian ```net.ipv4.ip_forward=1```
  
  - Install dchp relay : ```apt-get install isc-dhcp-relay -y```

-- TUBAN

  - Install dhcp server : ```apt-get install isc-dhcp-server -y```

### 3. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.
  
  setting subnet dhcp file dhcpd.conf di UML Tuban, lalu ubah isinya sesuai dengan range di soal.
  
### 4. Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.

### 5. Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP

### 6. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

### 7. User autentikasi milik Anri memiliki format:
   User : userta_yyy
   Password : inipassw0rdta_yyy
   Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01
    
   Pertama, akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA.
   

### 8. Anri sudah menjadwal pengerjaan TA-nya setiap hari Selasa-Rabu pukul 13.00-18.00.

### 9. Jadwal bimbingan dengan Bu Meguri adalah setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00).

### 10. Agar Anri bisa fokus mengerjakan TA, setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA🙂.

### 11. Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri, Bu Meguri meminta Anri untuk mengubah error page default squid

### 12. Karena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080.

Buat domain di Malang tapi ngarahin ke Tuban
