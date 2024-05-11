
# PROJECT UTS PENGOLAHAN CITRA A
## Dibuat oleh : Viana Salsabila Fairuz Syahla / 202231027


### Tahapan-tahapan project serta teori pendukung : 
### NO 1
#### 1. Import library untuk cv2, matplotlib dan numpy buat pustaka fungsonalitas untuk pengolahan citra, array dan pembuatan plot.

teori: OpenCV (Open Source Computer Vision Library) adalah pustaka perangkat lunak yang kuat dan populer untuk berbagai tugas pengolahan citra dan visi komputer. Di Python, OpenCV menyediakan fungsionalitas tingkat rendah yang ekstensif untuk manipulasi gambar, analisis, dan pembelajaran mesin. Kemampuannya meliputi:
1. Membaca dan Menulis Gambar: Memuat gambar dari berbagai format, termasuk PNG, JPEG, dan TIFF, dan menyimpan hasil pengolahan dalam format yang diinginkan.
2. Manipulasi Dasar Gambar: Melakukan operasi dasar seperti pemotongan, perubahan ukuran, rotasi, dan konversi ruang warna.
3. Peningkatan Gambar: Meningkatkan kualitas gambar dengan teknik seperti penghilangan noise, penajaman, dan koreksi warna.
4. Analisis Fitur: Mengekstrak fitur penting dari gambar, seperti tepi, sudut, dan bentuk.
5. Klasifikasi dan Pengenalan Objek: Mengklasifikasikan dan mengenali objek dalam gambar menggunakan algoritma pembelajaran mesin.

NumPy (Numerical Python) adalah pustaka komputasi numerik fundamental untuk Python. Kemampuannya dalam menangani array multidimensi dan operasi matematika menjadikannya alat yang ideal untuk pengolahan citra:
1. Penyimpanan Gambar sebagai Array: Menyimpan gambar sebagai array NumPy, memungkinkan manipulasi data gambar yang efisien.
2. Perhitungan Vektor dan Matriks: Melakukan operasi matematika kompleks pada gambar, seperti konvolusi dan transformasi Fourier.
3. Analisis Statistik: Menganalisis distribusi piksel gambar, menghitung statistik seperti mean dan median.
4. Pembuatan Visualisasi: Membuat visualisasi data gambar, seperti histogram dan plot.

Matplotlib adalah pustaka visualisasi Python yang populer untuk membuat grafik dan plot. Dalam pengolahan citra, Matplotlib digunakan untuk:
1. Menampilkan Gambar: Menampilkan gambar asli dan hasil pengolahan dengan kontrol kualitas gambar yang baik.
2. Membuat Visualisasi Data: Memvisualisasikan data yang diekstrak dari gambar, seperti histogram distribusi piksel dan kurva ciri.
3. Membuat Plot Kinerja: Menampilkan metrik kinerja algoritma pengolahan citra, seperti akurasi klasifikasi.

	`import cv2`
    `import matplotlib.pyplot as plt`
    `import numpy as np`

#### 2. Membaca gambar dan mengubah BGR ke RGB
variabel citra untuk membaca via2.jpg, gunain imread untuk bacanya, ubah juga pake cvtColor biar keubah dr BGR ke RGB.
`citra = cv2.imread("via2.jpg")`
`citra = cv2.cvtColor(citra, cv2.COLOR_BGR2RGB)`

`cv2.imread()` digunakan untuk membaca gambar dari file ke dalam variabel dalam bentuk array NumPy.
`cv2.cvtColor()` digunakan untuk mengubah ruang warna atau mode warna suatu gambar. Dalam contoh ini, setelah gambar dibaca, kita menggunakan cv2.COLOR_BGR2RGB untuk mengonversi gambar dari ruang warna BGR (Blue-Green-Red) yang digunakan oleh OpenCV ke ruang warna RGB (Red-Green-Blue) yang lebih umum digunakan.

#### 3. Membuat variabel kernel untuk membuat filter 2d
`kernel = np.array([[0,-1,0], 
                [-1,6,-1], 
                [0,-1,0]])`

dalam konteks filter 2D, kernel ini adalah kernel sharpening yang digunakan untuk mempertajam gambar.

`merah = citraOutput[:,:,0] # Merah`
`hijau = citraOutput[:,:,1] # Hijau`
`biru = citraOutput[:,:,2] # Biru`

##### Histogram
`fig, axs = plt.subplots(3,2, figsize=(12,10))`

buat histogram dengan 3 baris dan 2 kolom, baris 1 untuk histogram warna merah, 2 untuk hijau dan 3 untuk biru

##### Histogram merah
`hist_merah = cv2.calcHist([merah], [0], None, [256], [0,256])
axs[0,0].imshow(merah, cmap='gray')
axs[0,0].set_title('Red color')
axs[0,1].plot(hist_merah, color='r')`

##### Histogram hijau
`hist_hijau = cv2.calcHist([hijau, merah], [0], None, [256], [0,256])
axs[1,0].imshow(hijau, cmap='gray')
axs[1,0].set_title('Green color')
axs[1,1].plot(hist_hijau, color='g')`

##### Histogram biru
`hist_biru = cv2.calcHist([biru], [0], None, [256], [0,256])
axs[2,0].imshow(biru, cmap='gray')
axs[2,0].set_title('Blue color')
axs[2,1].plot(hist_biru, color='b')
`
`plt.show()`

pada histogram warna : calchist itu dr library matplotib, gunanya untuk menghitung histogram dari citra, variabel warna 'merah'/'hijau'/'biru' adalah gambar yang akan dihitung histogramnya. Histogram akan dihitung dari saluran warna pertama (indeks 0) dari gambar ini karena argumen kedua adalah [0].[0] indeks saluran warna yang akan dihitung histogramnya.None digunakan untuk mengindikasikan bahwa tidak ada maska yang digunakan untuk menghitung histogram. [256] adalah jumlah bin untuk histogram.[0,256] adalah rentang nilai piksel yang akan digunakan untuk menghitung histogram.

### NO 2

#### 1. Mengubah citraOutput dari RGB ke GRAY
`gray = cv2.cvtColor(citraOutput, cv2.COLOR_RGB2GRAY)`
#### 2. Mencari Ambang batas dengan threshold pada cv2
`fig, axs = plt.subplots(2,2, figsize=(10,7))`

`(thresh, binary) = cv2.threshold (gray, 3, 255, cv2.THRESH_BINARY)
(thresh, binary_2) = cv2.threshold (merah, 50, 255, cv2.THRESH_BINARY)
(thresh, binary_3) = cv2.threshold (hijau, 100, 255, cv2.THRESH_BINARY)
(thresh, binary_4) = cv2.threshold (biru, 201, 255, cv2.THRESH_BINARY)`

`axs[0,0].imshow(binary, cmap='binary')
axs[0,0].set_title('none')
axs[0,1].imshow(binary_2, cmap='binary')
axs[0,1].set_title('blue')
axs[1,0].imshow(binary_3, cmap='binary')
axs[1,0].set_title('red-blue')
axs[1,1].imshow(binary_4, cmap='binary')
axs[1,1].set_title('red-green-blue')`

menggunakan threshold pada library cv2 untuk menimbang nilai ambang batas yang none, gray adalah citra grayscale yang ingin di-threshold. 3 adalah nilai ambang yang digunakan untuk thresholding. Piksel dengan intensitas di atas nilai ini akan diubah menjadi 255, sedangkan piksel dengan intensitas di bawah nilai ini akan diubah menjadi 0. cv2.THRESH_BINARY adalah jenis thresholding yang digunakan.THRESH_BINARY ngubah piksel menjadi nilai maksimum jika melewati ambang, dan 0 jika tidak.

yang blue, merah adalah citra grayscale yang ingin di-threshold. 50 adalah nilai ambang yang digunakan untuk thresholding. Piksel dengan intensitas di atas nilai ini akan diubah menjadi 255, sedangkan piksel dengan intensitas di bawah nilai ini akan diubah menjadi 0. cv2.THRESH_BINARY adalah jenis thresholding yang digunakan.THRESH_BINARY ngubah piksel menjadi nilai maksimum jika melewati ambang, dan 0 jika tidak.

yang red-blue, hijau adalah citra grayscale yang ingin di-threshold. 100 adalah nilai ambang yang digunakan untuk thresholding. Piksel dengan intensitas di atas nilai ini akan diubah menjadi 255, sedangkan piksel dengan intensitas di bawah nilai ini akan diubah menjadi 0. cv2.THRESH_BINARY adalah jenis thresholding yang digunakan.THRESH_BINARY ngubah piksel menjadi nilai maksimum jika melewati ambang, dan 0 jika tidak.

yang red-green-blue, blue adalah citra grayscale yang ingin di-threshold. 201 adalah nilai ambang yang digunakan untuk thresholding. Piksel dengan intensitas di atas nilai ini akan diubah menjadi 255, sedangkan piksel dengan intensitas di bawah nilai ini akan diubah menjadi 0. cv2.THRESH_BINARY adalah jenis thresholding yang digunakan.THRESH_BINARY ngubah piksel menjadi nilai maksimum jika melewati ambang, dan 0 jika tidak.