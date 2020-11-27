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
