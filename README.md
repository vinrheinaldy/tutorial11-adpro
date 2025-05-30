# tutorial11-adpro

## Tutorial: Hello Minikube - Refleksi

### 1. Perbandingan Log Aplikasi Sebelum dan Sesudah Diekspos sebagai Service

Sebelum aplikasi diekspos sebagai Service, log hanya menampilkan pesan awal saat container mulai berjalan. Setelah diekspos sebagai Service dan mengakses aplikasi beberapa kali melalui proxy, log bertambah setiap kali ada permintaan baru, mencatat aktivitas HTTP. Ini membuktikan bahwa Service berhasil mengarahkan lalu lintas eksternal ke Pod

### 2. Tujuan Opsi `-n` pada Perintah `kubectl get`

Opsi `-n` digunakan untuk menentukan namespace di Kubernetes. Secara default, sumber daya dibuat di namespace `default`. Namespace `kube-system` berisi sumber daya tingkat sistem seperti `metrics-server` dan `kube-dns`. Dengan opsi `-n kube-system`, output hanya menampilkan sumber daya di namespace tersebut, sehingga Pod dan Service yang telah dibuat buat (di namespace `default`) tidak muncul.


## Tutorial: Rolling Update & Kubernetes Manifest File - Refleksi

### 1. Perbedaan antara Strategi Deployment Rolling Update dan Recreate

Perbedaan Strategi Rolling Update dan Recreate

-Rolling Update: Mengganti Pod lama dengan Pod baru secara bertahap, memastikan tidak ada downtime. Cocok untuk lingkungan produksi yang membutuhkan ketersediaan terus-menerus.

-Recreate: Menghentikan semua Pod lama sebelum membuat Pod baru, menyebabkan downtime selama proses. Lebih sederhana tetapi kurang andal.

### 2. Percobaan Deploy Spring Petclinic REST Menggunakan Strategi Recreate

Saya mencoba strategi Recreate dengan memodifikasi file `deployment.yaml` untuk menyertakan:

```strategy:
  type: Recreate```

Setelah menerapkannya, aplikasi mengalami downtime saat pembaruan, berbeda dengan strategi Rolling Update yang berjalan lancar tanpa gangguan.

### 3. Penyiapan File Manifest Berbeda untuk Strategi Recreate

Contoh deployment.yaml

```apiVersion: apps/v1
kind: Deployment
metadata:
  name: spring-petclinic-rest
spec:
  replicas: 3
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: spring-petclinic
  template:
    metadata:
      labels:
        app: spring-petclinic
    spec:
      containers:
      - name: spring-petclinic-rest
        image: docker.io/springcommunity/spring-petclinic-rest:3.0.2
        ports:
        - containerPort: 9966
        resources:
          limits:
            cpu: "500m"
            memory: "512Mi"```

### 4. Manfaat Penggunaan File Manifest Kubernetes

File manifest menyediakan cara deklaratif untuk mengelola sumber daya, memungkinkan version control, reproduksi, dan kolaborasi yang lebih mudah. Dibandingkan perintah `kubectl` manual, file manifest lebih konsisten dan mengurangi kesalahan manusia. Misalnya, menerapkan kembali `deployment.yaml` dan `service.yaml` setelah mereset Minikube lebih cepat dan andal daripada menjalankan perintah manual ulang.