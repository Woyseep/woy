nama: CI

pada: [dorong, alur kerja_dispatch]

pekerjaan:
  membangun:

    berjalan-on: windows-terbaru

    Langkah:
    - nama: Unduh
      jalankan: Invoke-WebRequest https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-windows-amd64.zip -OutFile ngrok.zip
    - nama: Ekstrak
      jalankan: Perluas-Arsip ngrok.zip
    - nama: Auth
      jalankan: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN
      lingkungan:
        NGROK_AUTH_TOKEN: ${{ rahasia.NGROK_AUTH_TOKEN }}
    - nama: Aktifkan TS
      jalankan: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-nama "fDenyTSConnections" -Nilai 0
    - jalankan: Aktifkan-NetFirewallRule -DisplayGroup "Desktop Jarak Jauh"
    - jalankan: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -nama "UserAuthentication" -Nilai 1
    - jalankan: Set-LocalUser -Nama "runneradmin" -Password (ConvertTo-SecureString -AsPlainText "P@ssw0rd!" -Force)
    - nama: Buat Terowongan
      jalankan: .\ngrok\ngrok.exe tcp 3389
