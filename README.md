# flask-minimal

Proyek awal Flask minimalis yang dirancang untuk membantu Anda menyiapkan aplikasi web yang bersih, sederhana, dan efisien dengan cepat. Proyek ini terstruktur agar tetap ringan dan berfokus pada produktivitas, dengan semua kode Anda tersimpan dalam satu berkas (`app.py`), beserta templat dasar dan aset statis.


## Features
- Aplikasi Flask satu berkas (`app.py`) untuk memaksimalkan produktivitas dan kesederhanaan.
- Struktur templat HTML dasar dengan gaya dan JavaScript minimal.
- Pengaturan proyek yang sederhana dan intuitif tanpa kerumitan yang tidak perlu.
- Mudah disesuaikan untuk pengembangan aplikasi web yang cepat.

## Preparation
```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip -y
```

## Installation

1. Clone repository:
   ```bash
   git clone https://github.com/yourusername/flask-minimal.git
   cd flask-minimal
   ```

2. Buat virtual environment (recommended):
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. Install depensi yng dibutuhkan:
   ```bash
   pip install -r requirements.txt
   ```

4. jalankan aplikasi:
   ```bash
   python app.py
   ```

Aplikasi Flask akan dimulai, dan Anda dapat melihatnya dengan menavigasi ke http://localhost:5000 di peramban Anda.

## Problem
Setelah anda berhasil melakukan cara diatas anda tidak dapat mengreboot mesin karena jika anda mengreboot maka anda harus mengulangi cara dari awal dan itu sangat memakan waktu. Maka dari itu terdapat beberapa cara agar Flask otomatis berjalan kembali setelah reboot EC2. Ini membuat aplikasi Flask berjalan sebagai layanan (daemon) yang otomatis start saat booting.

## Systemd

1. Buat File Service Systemd:
   ```bash
   sudo nano /etc/systemd/system/flask-minimal.service
   ```

## 2. Isikan ini (sesuaikan dengan path-mu!):

```ini
[Unit]
Description=Minimal Flask App
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/flask-minimal
ExecStart=/home/ubuntu/flask-minimal/venv/bin/python3 /home/ubuntu/flask-minimal/app.py
Restart=always

[Install]
WantedBy=multi-user.target
   ```
- Ganti ubuntu dan path sesuai dengan username & lokasi kamu.

3. Reload systemd dan Aktifkan Servicenya:
   ```bash
   sudo systemctl daemon-reexec
   sudo systemctl daemon-reload
   sudo systemctl enable flask-minimal.service
   ```

4. Lalu jalankan:
   ```bash
   sudo systemctl start flask-minimal.service
   ```

5. Cek status apakah berjalan:
   ```bash
   sudo systemctl status flask-minimal.service
   ```  

## Supervisor

1. Install Supervisor:
   ```bash
   sudo apt update
   sudo apt install supervisor -y
   ```

2. Buat Konfigurasi Supervisor
   ```bash
   sudo nano /etc/supervisor/conf.d/flask-minimal.conf
   ```

## 3. Isi file tersebut seperti ini:
```ini
[program:flask-minimal]
directory=/home/ubuntu/flask-minimal
command=/home/ubuntu/flask-minimal/venv/bin/python3 app.py
autostart=true
autorestart=true
stderr_logfile=/var/log/flask-minimal.err.log
stdout_logfile=/var/log/flask-minimal.out.log
user=ubuntu
environment=PATH="/home/ubuntu/flask-minimal/venv/bin"
   ```
- Pastikan:
user=ubuntu sesuai user EC2 kamu
path ke Python dan project sesuai

4. Reload dan Start Supervisor:
   ```bash
   sudo supervisorctl reread
   sudo supervisorctl update
   sudo supervisorctl start flask-minimal
   ```

5. Cek apakah sudah jalan:
   ```bash
  sudo supervisorctl status flask-minimal
   ```  

- Harusnya muncul seperti:
   ```bash
  flask-minimal                RUNNING    pid 1234, uptime 0:00:10
   ```  
- Kedua cara tersebut memang sama saja kegunaannya yaitu untuk membuat aplikasi Flask berjalan sebagai layanan (daemon) yang otomatis start saat booting. Tinggal menyesuaikan saja menurut kita cara mana yang lebih gampang.

## Usage

Proyek awal ini siap digunakan sebagai fondasi untuk membangun aplikasi web. Berkas app.py berisi semua rute dan logika Flask, sehingga mudah diperluas dan disesuaikan. Anda dapat menambahkan lebih banyak templat, rute, atau berkas statis sesuai kebutuhan.

## Customization
Anda dapat dengan mudah mengubah:

- Struktur HTML di `templates/index.html`
- Gaya di `static/style.css`
- Interaktivitas di `static/script.js`

Silakan perbarui berkas app.py untuk menambahkan rute atau logika tambahan sesuai kebutuhan Anda.

## License
Proyek ini dilisensikan di bawah Lisensi MIT.

## Contributing
Jangan ragu untuk melakukan forking repositori ini dan membuat permintaan tarik jika Anda memiliki peningkatan atau perbaikan bug. Jika Anda memiliki saran, sampaikan masalah Anda, dan kami akan membahasnya!

Proyek ini dibangun dengan mengutamakan kesederhanaan dan efisiensi, sempurna untuk memulai aplikasi web kecil atau prototipe dengan cepat dan dengan biaya operasional minimal.

