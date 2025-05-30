# tutorial11-adpro

## Tutorial: Hello Minikube - Refleksi

### 1. Perbandingan Log Aplikasi Sebelum dan Sesudah Diekspos sebagai Service

Sebelum aplikasi diekspos sebagai Service, log hanya menampilkan pesan awal saat container mulai berjalan. Setelah diekspos sebagai Service dan mengakses aplikasi beberapa kali melalui proxy, log bertambah setiap kali ada permintaan baru, mencatat aktivitas HTTP. Ini membuktikan bahwa Service berhasil mengarahkan lalu lintas eksternal ke Pod

### 2. Tujuan Opsi `-n` pada Perintah `kubectl get`

Opsi `-n` digunakan untuk menentukan namespace di Kubernetes. Secara default, sumber daya dibuat di namespace `default`. Namespace `kube-system` berisi sumber daya tingkat sistem seperti `metrics-server` dan `kube-dns`. Dengan opsi `-n kube-system`, output hanya menampilkan sumber daya di namespace tersebut, sehingga Pod dan Service yang telah dibuat buat (di namespace `default`) tidak muncul.
