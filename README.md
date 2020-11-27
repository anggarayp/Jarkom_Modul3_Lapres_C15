# Jarkom_Modul3_Lapres_C15
Lapres Modul 3 Jarkom

### Nama Anggota Kelompok :
### 1. Anggara Yuda Pratama (05111840000008)
### 2. Patrick (05111840000098)

![image](https://user-images.githubusercontent.com/61231385/100473189-5af25200-3110-11eb-8893-6b7712f26d48.png)

Anri adalah seorang mahasiswa tingkat akhir yang sedang mengerjakan TA mengenai DHCP dan Proxy. Bu Meguri sebagai dosen pembimbing Anri memberikan tugas pertamanya,

Keterangan :
- Terlebih dahulu mengisi file topologi.sh dengan cara : ```nano topologi.sh```

- Lalu isi topologi.sh dengan :
  ```
  # Switch
  uml_switch -unix switch1 > /dev/null < /dev/null &
  uml_switch -unix switch2 > /dev/null < /dev/null &

  # Router
  xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,'ip_tuntap_tiap_kelompok' eth1=daemon,,,switch2 eth2=daemon,,,switch1 mem=96M &

  # Server
  xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=128M &
  xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=160M &
  xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch2 mem=160M &

  # Klien
  xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=96M &
  xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=96M &
  ```

- Lalu masing-masing uml di-export proxy-nya agar bisa apt-get update. Kita membuat file dengan nama export.sh yang isinya :
  ```
  export http_proxy=”http://DPTSI-564318-23148:7b3c2@proxy.its.ac.id:8080”
  export https_proxy=”http://DPTSI-564318-23148:7b3c2@proxy.its.ac.id:8080”
  export ftp_proxy=”http://DPTSI-564318-23148:7b3c2@proxy.its.ac.id:8080”
  ```
 
 - Jangan lupa membuat file bye.sh dengan cara : ```nano bye.sh```, yang isinya :
 
   ```
   uml_mconsole SURABAYA halt
   uml_mconsole MALANG halt
   uml_mconsole MOJOKERTO halt
   uml_mconsole SIDOARJO halt
   uml_mconsole GRESIK halt
   uml_mconsole PROBOLINGGO halt
   ```

### 1. Membuat topologi jaringan


Anri sudah pernah mempelajari teknik Jaringan Komputer sehingga Anri dapat membuat topologi tersebut dengan mudah. Bu Meguri memerintahkan Anri untuk menjadikan **SURABAYA** sebagai router, **MALANG** sebagai DNS Server, **TUBAN** sebagai DHCP server, serta **MOJOKERTO** sebagai Proxy server, dan **UML lainnya** sebagai client. Bu Meguri berpesan pada Anri untuk menyusun topologi secara hati-hati dan memperhatikan gambar topologi yang diberikan Bu Meguri.


### 2. SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client.

### 3. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.

### 4. Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.

### 5. Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP

### 6. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

### 7. User autentikasi milik Anri memiliki format:
    ● User : userta_yyy
    ● Password : inipassw0rdta_yyy
    Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01
    
   Pertama, akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA.
   

### 8. Anri sudah menjadwal pengerjaan TA-nya setiap hari Selasa-Rabu pukul 13.00-18.00.

### 9. Jadwal bimbingan dengan Bu Meguri adalah setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00).

### 10. Agar Anri bisa fokus mengerjakan TA, setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA🙂.

### 11. Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri, Bu Meguri meminta Anri untuk mengubah error page default squid

### 12. Karena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080.

