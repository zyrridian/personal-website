---
title: "Cara Membuat Aplikasi Kalkulator di Kotlin Menggunakan Android Studio"
meta_title: "Membuat Aplikasi Kalkulator di Kotlin | Tutorial Android Studio"
description: "Pelajari cara membuat aplikasi kalkulator sederhana namun fungsional menggunakan Kotlin di Android Studio. Tutorial ini mencakup desain UI, gaya tombol, dan operasi aritmatika dasar."
date: 2025-01-31T00:00:00Z
image: "images/blog/blog-004/img.png"
categories: ["Tutorial", "Android", "Teknologi"]
author: "zyrridian"
tags: ["Kotlin", "Mobile Development"]
draft: false
---

Dalam tutorial ini, kita akan memandu Anda membuat aplikasi kalkulator sederhana namun fungsional menggunakan Kotlin di Android Studio. Proyek ini cocok untuk pemula dan akan membimbing Anda langkah demi langkah, mulai dari menyiapkan proyek hingga membangun kalkulator yang berfungsi penuh. Di akhir tutorial, Anda akan memiliki aplikasi yang mendukung operasi aritmatika dasar seperti penjumlahan, pengurangan, perkalian, dan pembagian.

Aplikasi akhir akan memiliki antarmuka pengguna (UI) yang bersih dan intuitif, dilengkapi dengan keypad numerik dan layar hasil. Pengguna dapat melakukan perhitungan dengan lancar menggunakan tombol-tombol interaktif. Di bawah ini, Anda akan melihat GIF yang memperlihatkan aplikasi dalam aksi, mendemonstrasikan cara pengguna memasukkan angka dan operasi untuk menghitung hasil.

{{< image src="images/blog/blog-004/img01.gif" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

## Mempersiapkan Proyek
Sebelum mulai menulis kode, pastikan Anda telah menginstal versi terbaru Android Studio. Meskipun tidak wajib, menggunakan versi terbaru akan memastikan akses ke fitur terkini dan kompatibilitas yang lebih baik.

- **Kode Final**: https://github.com/zyrridian/calculator-app
- **Demo Aplikasi**: https://github.com/zyrridian/calculator-app/releases/tag/v1.0

## Menambahkan Gaya ke Tema

Untuk membuat desain yang konsisten dan menarik secara visual, kita akan mulai dengan mendefinisikan **style** untuk aplikasi. Ini akan membantu mempertahankan tampilan seragam di seluruh tombol dan tata letak (layout).

- Buka tab **Project** di Android Studio.
- Arahkan ke `res > values > themes.xml`.

Anda akan melihat kode default seperti ini:

{{< image src="images/blog/blog-004/img02.webp" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

Jika Anda menggunakan versi Android Studio yang berbeda dan melihat kode yang sedikit tidak sama, jangan khawatir — Anda tidak perlu mengubah kode default secara besar-besaran. Cukup tambahkan baris berikut untuk mengatur warna latar belakang (background) menjadi putih murni:

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Base.Theme.Calculator" parent="Theme.Material3.DayNight.NoActionBar">
        <!-- Customize your light theme here. -->
        <!-- <item name="colorPrimary">@color/my_dark_primary</item> -->
        <item name="android:colorBackground">@color/white</item>
    </style>

    <style name="Theme.Calculator" parent="Base.Theme.Calculator" />

</resources>
```

## Membuat Gaya Tombol

Berikutnya, kita akan mendefinisikan tiga gaya tombol:

1. `CalculatorButton`: Untuk tombol numerik standar.
2. `CalculatorButtonOperation`: Untuk tombol operasi (misal: +, -, *, /).
3. `CalculatorButtonEqual`: Untuk tombol sama dengan (=).

{{< image src="images/blog/blog-004/img03.png" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

Setiap gaya menentukan ukuran, warna, dan tampilan tombol. Gaya ini memastikan tombol terlihat berbeda secara visual namun konsisten di seluruh aplikasi.

Berikut cara mendefinisikan gaya tersebut di file `themes.xml` Anda. Pastikan menambahkan kode ini sebelum tag penutup `</resources>`:

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme -->
    <style name="Base.Theme.Calculator" parent="Theme.Material3.DayNight.NoActionBar">
        <!-- <item name="colorPrimary">@color/my_dark_primary</item> -->
        <item name="android:colorBackground">@color/white</item>
    </style>

    <style name="Theme.Calculator" parent="Base.Theme.Calculator" />

    <style name="CalculatorButton" parent="Widget.MaterialComponents.Button.TextButton">
        <item name="android:layout_width">0dp</item>
        <item name="android:layout_height">0dp</item>
        <item name="android:layout_rowWeight">1</item>
        <item name="android:layout_columnWeight">1</item>
        <item name="android:backgroundTint">#F8F8F8</item>
        <item name="android:textColor">#000000</item>
        <item name="android:textSize">18sp</item>
        <item name="cornerRadius">16dp</item>
        <item name="rippleColor">#FFE0B2</item>
    </style>

    <style name="CalculatorButtonOperation" parent="Widget.MaterialComponents.Button.TextButton">
        <item name="android:layout_width">0dp</item>
        <item name="android:layout_height">0dp</item>
        <item name="android:layout_rowWeight">1</item>
        <item name="android:layout_columnWeight">1</item>
        <item name="android:backgroundTint">#FF5722</item>
        <item name="android:textColor">#FFFFFF</item>
        <item name="android:textSize">18sp</item>
        <item name="cornerRadius">16dp</item>
        <item name="rippleColor">#FF8A65</item>
    </style>

    <style name="CalculatorButtonEqual" parent="Widget.MaterialComponents.Button.TextButton">
        <item name="android:layout_width">0dp</item>
        <item name="android:layout_height">0dp</item>
        <item name="android:layout_rowWeight">1</item>
        <item name="android:layout_columnWeight">1</item>
        <item name="android:backgroundTint">#FF9800</item>
        <item name="android:textColor">#FFFFFF</item>
        <item name="android:textSize">20sp</item>
        <item name="cornerRadius">16dp</item>
        <item name="rippleColor">#FFC947</item>
    </style>

</resources>
```

{{< notice "note" >}}  
Jika kode terlihat rumit di awal, jangan khawatir! Kunci utama menguasai pemrograman adalah terus melanjutkan. Kami akan menjelaskan detail cara kerja gaya-gaya ini setelah aplikasi berfungsi.  
{{< /notice >}}  

## Membuat Tata Letak  

{{< image src="images/blog/blog-004/img04.png" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}  

Setelah mendefinisikan gaya aplikasi, sekarang saatnya merancang tata letak. Tata letak yang terstruktur dengan baik sangat penting untuk membuat antarmuka yang intuitif dan ramah pengguna. Pada bagian ini, kita akan membangun struktur dasar aplikasi kalkulator, termasuk header dan bagian tampilan (*display*).  

Buka file `res > layout > activity_main.xml` di Android Studio. Beralih ke mode "**Split**" untuk melihat kode XML dan editor visual secara bersamaan. Ini memudahkan Anda melihat bagaimana perubahan memengaruhi tata letak secara *real-time*.  

Berikut kode XML untuk tata letak awal:  

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/white"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <!-- Header Section -->
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginHorizontal="16dp"
        android:layout_marginTop="16dp"
        android:gravity="center"
        android:text="Calculator"
        android:textColor="@color/black"
        android:textSize="24sp"
        android:textStyle="bold" />

    <!-- Display Section -->
    <androidx.cardview.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:elevation="0dp"
        app:cardBackgroundColor="@color/white"
        app:cardCornerRadius="16dp">

        <TextView
            android:id="@+id/tvDisplay"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="end"
            android:padding="24dp"
            android:singleLine="true"
            android:text="0"
            android:textColor="#000000"
            android:textSize="32sp" />

    </androidx.cardview.widget.CardView>

</LinearLayout>
```

### Bagian Header  

Bagian header adalah `TextView` sederhana yang menampilkan judul aplikasi "Calculator". Teks ini diposisikan tengah secara horizontal dan menggunakan ukuran font **24sp** dengan gaya tebal untuk visibilitas yang jelas. Margin yang ditambahkan memastikan judul memiliki jarak yang sesuai dari tepi layar, memberikan tampilan yang rapi dan terorganisir.  

### Bagian Display  

Di bawah header, kami menambahkan `CardView` untuk membuat area tampilan kalkulator yang berbeda secara visual. `CardView` ini memiliki sudut melengkung (*corner radius*) sebesar **16dp** dan warna latar belakang yang sesuai dengan tema putih aplikasi. Di dalam `CardView`, sebuah `TextView` digunakan untuk menampilkan perhitungan atau hasil saat ini.  

`TextView` (`tvDisplay`) dikonfigurasi untuk menampilkan teks yang rata kanan (untuk bahasa left-to-right) dengan ukuran font lebih besar (**32sp**) agar mudah dibaca. Atribut `singleLine="true"` memastikan teks tidak *wrap* ke baris baru, sehingga tampilan tetap rapi.  

## Menambahkan Grid Tombol  

{{< image src="images/blog/blog-004/img05.png" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}  

Setelah header dan bagian display selesai, langkah selanjutnya adalah menambahkan grid tombol untuk angka, operasi, dan tombol sama dengan (=). Grid ini memungkinkan pengguna memasukkan angka dan melakukan perhitungan dengan lancar. Untuk mencapainya, kita akan menggunakan `GridLayout` untuk mengatur tombol secara terstruktur dan responsif.  

Berikut kode XML terbaru untuk file `activity_main.xml`. Tambahkan bagian grid tombol ke kode Anda di posisi yang sesuai. Untuk kejelasan, struktur kode yang sudah ada tetap dipertahankan agar Anda mudah melihat di mana bagian baru ditempatkan:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/white"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <!-- Header Section -->
    <TextView
        android:layout_width="match_parent"
        ... />

    <!-- Display Section -->
    <androidx.cardview.widget.CardView
        android:layout_width="match_parent"
        ... >

        <TextView
            android:id="@+id/tvDisplay"
            ... />

    </androidx.cardview.widget.CardView>

    <!-- Button Grid -->
    <GridLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:columnCount="4"
        android:padding="8dp"
        android:rowCount="5">

        <!-- Row 1 -->
        <com.google.android.material.button.MaterialButton
            android:id="@+id/btnClear"
            style="@style/CalculatorButtonOperation"
            android:layout_marginStart="6dp"
            android:layout_marginEnd="6dp"
            android:text="C" />

        <com.google.android.material.button.MaterialButton
            android:id="@+id/btnBackspace"
            style="@style/CalculatorButtonOperation"
            android:layout_marginStart="6dp"
            android:layout_marginEnd="6dp"
            android:text="⌫" />

        <com.google.android.material.button.MaterialButton
            android:id="@+id/btnPercent"
            style="@style/CalculatorButtonOperation"
            android:layout_marginStart="6dp"
            android:layout_marginEnd="6dp"
            android:text="%" />

        <com.google.android.material.button.MaterialButton
            android:id="@+id/btnDivide"
            style="@style/CalculatorButtonOperation"
            android:layout_marginStart="6dp"
            android:layout_marginEnd="6dp"
            android:text="÷" />

        <!-- Row 2-->

    </GridLayout>

</LinearLayout>
```

### Memahami GridLayout  

`GridLayout` dikonfigurasi untuk memenuhi lebar layar penuh (`match_parent`) dengan tinggi **0dp**. Sekilas, ini mungkin terlihat tidak masuk akal — mengapa tinggi diatur ke **0dp**? Jawabannya terletak pada properti `layout_weight`. Dengan mengatur `layout_weight="1"`, GridLayout akan mengembang secara dinamis untuk mengisi sisa ruang vertikal di dalam `LinearLayout` induk. Ini memastikan grid tombol berukuran tepat dan responsif.  

Kita juga mengatur `columnCount` ke 4 dan `rowCount` ke 5, membuat grid yang dapat menampung 20 tombol (4 kolom × 5 baris). Padding sebesar **8dp** memastikan tombol terpisah secara merata dan enak dipandang.  

## Menambahkan Tombol ke Grid  

Di dalam `GridLayout`, kita menambahkan baris pertama tombol: Clear (C), Backspace (⌫), Persen (%), dan Bagi (÷). Tombol ini menggunakan komponen `MaterialButton` yang menyediakan desain modern dan konsisten. Setiap tombol diberi gaya menggunakan style **`CalculatorButtonOperation`** yang telah kita definisikan sebelumnya.  

- **Margins**: Setiap tombol memiliki margin horizontal (`layout_marginStart` dan `layout_marginEnd`) sebesar **6dp** untuk memberikan jarak antar tombol.  
- **IDs**: ID unik seperti `btnClear` dan `btnBackspace` diberikan ke setiap tombol, yang nantinya akan digunakan untuk menangani klik tombol di kode Kotlin.  

{{< notice "tip" >}}  
Eksperimen dengan Gaya: Coba hapus atribut `style` dari salah satu tombol untuk melihat pengaruhnya pada tampilan. Ini akan membantu Anda memahami pentingnya gaya yang telah kita definisikan sebelumnya.  
{{< /notice >}}  

{{< notice "info" >}}  
Komentar Baris: Perhatikan komentar `<!-- Row 1 -->` di atas set tombol pertama. Karena kita mengatur `columnCount` ke 4, setiap empat tombol akan membentuk baris baru. Menambahkan komentar seperti ini membuat kode lebih mudah dibaca dan di-maintain.  
{{< /notice >}}  

### Langkah Selanjutnya  
Setelah baris pertama tombol selesai, Anda bisa melanjutkan menambahkan tombol angka, operasi, dan tombol sama dengan (=). Ikuti pola yang sama, pastikan setiap baris ditandai dengan komentar (contoh: `<!-- Row 2 -->`).

```xml
<!-- Row 2 -->
<com.google.android.material.button.MaterialButton
    android:id="@+id/btn7"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="7" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btn8"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="8" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btn9"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="9" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btnMultiply"
    style="@style/CalculatorButtonOperation"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="×" />

<!-- Row 3 -->
<com.google.android.material.button.MaterialButton
    android:id="@+id/btn4"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="4" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btn5"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="5" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btn6"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="6" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btnMinus"
    style="@style/CalculatorButtonOperation"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="-" />

<!-- Row 4 -->
<com.google.android.material.button.MaterialButton
    android:id="@+id/btn1"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="1" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btn2"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="2" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btn3"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="3" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btnPlus"
    style="@style/CalculatorButtonOperation"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="+" />

<!-- Row 5 -->
<com.google.android.material.button.MaterialButton
    android:id="@+id/btnComma"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="." />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btn0"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="0" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btnSignChange"
    style="@style/CalculatorButton"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="±" />

<com.google.android.material.button.MaterialButton
    android:id="@+id/btnEqual"
    style="@style/CalculatorButtonEqual"
    android:layout_marginStart="6dp"
    android:layout_marginEnd="6dp"
    android:text="=" />
```

Setelah semua tombol ditambahkan, tata letak akan selesai, dan Anda akan memiliki antarmuka kalkulator yang berfungsi penuh.

{{< notice "info" >}}
Anda dapat menemukan kode final untuk `activity_main.xml` di [**repositori GitHub ini**](https://github.com/zyrridian/calculator-app/blob/master/app/src/main/res/layout/activity_main.xml).
{{< /notice >}}

## Menambahkan Fungsi

Tata letak aplikasi kita sudah selesai, tetapi saat Anda mengklik tombol, tidak ada yang terjadi. Mengapa? Karena sejauh ini kita hanya membangun antarmuka pengguna — kita belum menambahkan fungsionalitas untuk menangani interaksi pengguna. Untuk membuat kalkulator berfungsi, kita perlu mengimplementasikan logika di `MainActivity.kt`. Mari masuk ke kode dan menghidupkan kalkulator kita.

Buka file `MainActivity.kt` Anda. Di sini, kita akan mulai dengan mendeklarasikan variabel untuk menyimpan status kalkulator dan menghubungkan komponen UI dari tata letak XML ke kode Kotlin. Kita juga akan menyiapkan *event listener* klik untuk setiap tombol untuk menentukan apa yang terjadi saat pengguna berinteraksi dengan tombol tersebut.

Berikut tampilan pengaturan awalnya:

```kotlin
package com.example.calculator

import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.google.android.material.button.MaterialButton

class MainActivity : AppCompatActivity() {

    private lateinit var tvDisplay: TextView
    private var currentInput = ""
    private var operator = ""
    private var lastOperator = ""
    private var result = 0.0
    private var isNewInput = false

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }

        tvDisplay = findViewById(R.id.tvDisplay)

        // Number buttons
        val numberButtons = listOf<MaterialButton>(
            findViewById(R.id.btn0),
            findViewById(R.id.btn1),
            findViewById(R.id.btn2),
            findViewById(R.id.btn3),
            findViewById(R.id.btn4),
            findViewById(R.id.btn5),
            findViewById(R.id.btn6),
            findViewById(R.id.btn7),
            findViewById(R.id.btn8),
            findViewById(R.id.btn9)
        )

        numberButtons.forEach { button ->
            button.setOnClickListener { appendToDisplay(button.text.toString()) }
        }

        // Operation buttons
        findViewById<MaterialButton>(R.id.btnPlus).setOnClickListener { setOperator("+") }
        findViewById<MaterialButton>(R.id.btnMinus).setOnClickListener { setOperator("-") }
        findViewById<MaterialButton>(R.id.btnMultiply).setOnClickListener { setOperator("×") }
        findViewById<MaterialButton>(R.id.btnDivide).setOnClickListener { setOperator("÷") }
        findViewById<MaterialButton>(R.id.btnPercent).setOnClickListener { calculatePercentage() }
        findViewById<MaterialButton>(R.id.btnSignChange).setOnClickListener { toggleSign() }

        // Clear and backspace
        findViewById<MaterialButton>(R.id.btnClear).setOnClickListener { clearDisplay() }
        findViewById<MaterialButton>(R.id.btnBackspace).setOnClickListener { backspace() }

        // Decimal and equals
        findViewById<MaterialButton>(R.id.btnComma).setOnClickListener { appendToDisplay(".") }
        findViewById<MaterialButton>(R.id.btnEqual).setOnClickListener { calculateResult() }
    }

}
```

### Variabel:

- `tvDisplay`: `TextView` ini akan menampilkan input atau hasil saat ini.
- `currentInput`: Menyimpan input pengguna sebagai string.
- `operator` dan `lastOperator`: Melacak operasi aritmatika saat ini dan sebelumnya.
- `result`: Menyimpan hasil perhitungan.
- `isNewInput`: Sebuah flag untuk menentukan apakah pengguna memulai input baru setelah operasi.

### Menghubungkan Komponen UI:

- Kita menggunakan `findViewById` untuk menghubungkan setiap tombol dari tata letak XML ke kode Kotlin.
- Untuk tombol angka, kita melakukan loop menggunakan `forEach` dan mengatur *click listener* untuk menambahkan teks tombol ke tampilan.
- Tombol operasi (`+`, `-`, `×`, `÷`), fungsi khusus (`%`, perubahan tanda), dan tombol utilitas (`C`, `⌫`, `.`, `=`) diberikan *click listener* masing-masing.

## Langkah Selanjutnya: Membuat Fungsi untuk Click Listeners

{{< image src="images/blog/blog-004/img06.png" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

Setelah mengatur *click listener*, kita perlu mendefinisikan fungsi yang menangani interaksi ini. Fungsi-fungsi ini akan mengelola input, melakukan perhitungan, dan memperbarui tampilan. Pastikan untuk menempatkan fungsi-fungsi ini di dalam kelas `MainActivity`, setelah metode `onCreate()`, untuk menghindari error.

### Fungsi Penanganan Input

Mari mulai dengan fungsi `appendToDisplay()`, yang menangani input pengguna:

```kotlin
private fun appendToDisplay(value: String) {
    if (isNewInput) {
        currentInput = ""
        isNewInput = false
    }
    if (currentInput == "0" && value != ".") {
        currentInput = value
    } else {
        currentInput += value
    }
    updateDisplay(currentInput)
}
```

Fungsi ini memeriksa apakah pengguna memulai input baru (`isNewInput`). Jika ya, fungsi akan menghapus string `currentInput`. Fungsi ini juga memastikan bahwa angka nol di depan (*leading zeros*) ditangani dengan benar dan menambahkan nilai baru ke input. Terakhir, fungsi ini memanggil `updateDisplay()` untuk memperbarui tampilan dengan input yang telah diupdate.

Fungsi `updateDisplay()` bertanggung jawab untuk menampilkan input atau hasil saat ini di layar. Namun, kita belum mendefinisikannya. Selain ini, kita juga akan membuat fungsi utilitas untuk menghapus tampilan, menghapus karakter terakhir, dan melakukan operasi lainnya.

### Fungsi Utilitas

Setelah mengatur penanganan input dasar, mari implementasikan fungsi utilitas yang akan membantu mengelola status kalkulator dan interaksi pengguna. Fungsi-fungsi ini mencakup menghapus tampilan, menangani backspace, dan memperbarui tampilan dengan nilai baru.

Berikut kode untuk fungsi utilitas:

```kotlin
private fun clearDisplay() {
    currentInput = ""
    operator = ""
    result = 0.0
    updateDisplay("0")
}

private fun backspace() {
    if (currentInput.isNotEmpty()) {
        currentInput = currentInput.dropLast(1)
        updateDisplay(if (currentInput.isEmpty()) "0" else currentInput)
    }
}

private fun updateDisplay(value: String) {
    tvDisplay.text = value
}
```

#### clearDisplay()

Fungsi `clearDisplay()` mengatur ulang kalkulator ke keadaan awalnya. Fungsi ini menghapus string `currentInput`, mereset variabel `operator` dan `result`, serta memperbarui tampilan untuk menampilkan "0". Fungsi ini biasanya dipanggil saat pengguna menekan tombol "C" (Clear), memastikan semua input dan perhitungan sebelumnya dihapus.

#### backspace()

Meskipun fungsi `backspace()` mungkin terlihat mirip dengan `clearDisplay()`, logikanya cukup berbeda. Fungsi ini menangani penghapusan karakter terakhir dalam string `currentInput`.

- **Periksa Input Kosong**: Pertama, fungsi memeriksa apakah `currentInput` kosong. Jika ya, fungsi tidak melakukan apa pun karena tidak ada yang perlu dihapus.
- **Hapus Karakter Terakhir**: Jika `currentInput` tidak kosong, fungsi menggunakan `dropLast()` bawaan Kotlin untuk menghapus karakter terakhir dari string. Ini menghilangkan kebutuhan manipulasi string manual.
- **Perbarui Tampilan**: Setelah memodifikasi `currentInput`, fungsi memanggil `updateDisplay()` untuk menyegarkan layar. Jika `currentInput` menjadi kosong setelah penghapusan, tampilan diperbarui untuk menampilkan "0". Jika tidak, tampilan akan menampilkan `currentInput` yang telah diperbarui.

#### updateDisplay()

Fungsi `updateDisplay()` adalah utilitas sederhana namun penting yang memperbarui tampilan kalkulator dengan nilai yang diberikan.
- **Parameter**: Fungsi ini menerima parameter `value` bertipe `String`, yang mewakili teks yang akan ditampilkan.
- **Fungsi**: Fungsi ini mengatur properti `text` dari `tvDisplay` (`TextView` yang digunakan untuk tampilan kalkulator) ke nilai yang diberikan. Fungsi ini dipanggil setiap kali tampilan perlu disegarkan, seperti setelah menambahkan angka, menghapus tampilan, atau menghapus karakter.

### Fungsi Penanganan Operator

Selanjutnya, kita akan menambahkan dua fungsi untuk menangani operator matematika dan menghitung hasil sementara saat operasi dirantai. Misalnya, ketika pengguna memasukkan `2 + 3 - 1`, kalkulator perlu menghitung `2 + 3` sebelum menangani `- 1`.

Berikut kode untuk fungsi-fungsi ini:

```kotlin
private fun setOperator(op: String) {
    if (currentInput.isNotEmpty()) {
        if (operator.isNotEmpty()) {
            calculateIntermediateResult()
        } else {
            result = currentInput.toDouble()
        }
        operator = op
        lastOperator = operator
        currentInput = ""
        isNewInput = true
        updateDisplay("$result $operator")
    }
}

private fun calculateIntermediateResult() {
    if (operator.isNotEmpty() && currentInput.isNotEmpty()) {
        val secondOperand = currentInput.toDouble()
        result = when (operator) {
            "+" -> result + secondOperand
            "-" -> result - secondOperand
            "×" -> result * secondOperand
            "÷" -> if (secondOperand != 0.0) result / secondOperand else Double.NaN
            else -> result
        }
        operator = ""
    }
}
```

#### Fungsi setOperator()

Fungsi `setOperator()` bertanggung jawab untuk mengatur operator saat ini dan mempersiapkan kalkulator untuk input berikutnya. Berikut cara kerjanya:
- Jika sudah ada operator yang digunakan, fungsi ini memanggil `calculateIntermediateResult()` untuk menghitung hasil operasi sebelumnya.
- Jika tidak ada operator yang diatur, fungsi menyimpan input saat ini sebagai operand pertama dalam variabel `result`.
- Kemudian, fungsi memperbarui variabel `operator` dan `lastOperator`, menghapus `currentInput`, dan mengatur `isNewInput` ke `true` untuk menunjukkan bahwa input baru diharapkan.
- Terakhir, fungsi memperbarui tampilan untuk menampilkan hasil saat ini dan operator baru.

#### Fungsi calculateIntermediateResult()

Fungsi `calculateIntermediateResult()` menangani komputasi aktual saat operasi dirantai. Berikut penjelasannya:
- Fungsi memeriksa apakah operator dan operand kedua (`currentInput`) tersedia.
- Menggunakan ekspresi `when`, fungsi melakukan perhitungan yang sesuai berdasarkan operator (`+`, `-`, `×`, `÷`).
- Untuk pembagian, fungsi memastikan operand kedua tidak nol untuk menghindari error, mengembalikan `Double.NaN` (Not a Number) jika terjadi pembagian dengan nol.
- Setelah menghitung hasil, fungsi menghapus variabel `operator` untuk mempersiapkan operasi berikutnya.

### Fungsi Komputasi

Terakhir, kita akan menambahkan tiga fungsi lagi untuk menangani komputasi akhir: menghitung hasil saat tombol sama dengan (`=`) ditekan, menghitung persentase, dan mengubah tanda (positif/negatif) dari input saat ini.

```kotlin
private fun calculateResult() {
    if (currentInput.isNotEmpty()) {
        calculateIntermediateResult()
        currentInput = result.toString()
        operator = ""
        updateDisplay(currentInput)
    }
}

private fun calculatePercentage() {
    if (currentInput.isNotEmpty()) {
        currentInput = (currentInput.toDouble() / 100).toString()
        updateDisplay(currentInput)
    }
}

private fun toggleSign() {
    if (currentInput.isNotEmpty()) {
        currentInput = if (currentInput.startsWith("-")) {
            currentInput.substring(1)
        } else {
            "-$currentInput"
        }
        updateDisplay(currentInput)
    }
}
```

Fungsi `calculateResult()` menyelesaikan komputasi saat pengguna menekan "=" dengan memanggil `calculateIntermediateResult()`, memperbarui `currentInput` dengan hasilnya, menghapus `operator`, dan menyegarkan tampilan. Begitu juga, fungsi `calculatePercentage()` mengubah `currentInput` menjadi persentase dengan membaginya dengan 100 dan memperbarui tampilan sesuai hasilnya.

Fungsi `toggleSign()` mengubah tanda `currentInput`, mengubah angka positif menjadi negatif dan sebaliknya. Jika input diawali dengan "-", fungsi menghapus tanda tersebut; jika tidak, fungsi menambahkan tanda "-". Tampilan kemudian diperbarui untuk mencerminkan perubahan tersebut.

## Menguji Aplikasi

Itu seharusnya cukup untuk membuat kalkulator berfungsi sepenuhnya! Sebelum menjalankan aplikasi, periksa kembali kode Anda untuk memastikan tidak ada error atau typo. Setelah semuanya terlihat baik, Anda dapat menguji aplikasi di emulator atau perangkat fisik untuk memastikan semua tombol dan operasi berfungsi seperti yang diharapkan.

{{< notice "info" >}}
Sebagai referensi, Anda dapat menemukan kode lengkap **MainActivity.kt** di [**repositori GitHub ini**](https://github.com/zyrridian/calculator-app/blob/master/app/src/main/java/com/example/calculator/MainActivity.kt).
{{< /notice >}}

## Sentuhan Akhir

Saat Anda menjalankan aplikasi, aplikasi akan berfungsi! Selamat! Namun, ada beberapa masalah yang bisa kita perbaiki. Pertama, mari atasi masalah orientasi. Saat Anda mengubah tampilan ke mode landscape, UI akan terlihat **rusak atau tidak sejajar**, yang tentu tidak ideal.

Kita bisa memperbaikinya dengan membuat tata letak responsif untuk ukuran layar tertentu, tetapi itu akan memakan waktu dan membuat codelab ini terlalu rumit!

{{< image src="images/blog/blog-004/img07.gif" caption="" alt="alter-text" height="" width="" position="left" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

Karena itu, mari terapkan trik sederhana. Kita akan mencegah aplikasi kita berotasi ke mode landscape, sehingga aplikasi akan selalu tetap dalam orientasi portrait. Untuk melakukan ini, buka file **`AndroidManifest.xml`** dan modifikasi tag `<activity>` dengan menambahkan atribut `android:screenOrientation="portrait"`.

{{< image src="images/blog/blog-004/img08.png" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

Berikut tampilan `AndroidManifest.xml` yang telah diperbarui:

```kotlin
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Calculator"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:screenOrientation="portrait">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

Kita hampir selesai! Untuk membuat aplikasi terlihat lebih halus dan menarik secara visual, mari ubah logo aplikasi agar tidak terlihat membosankan.

{{< notice "info" >}}
Saya telah menyiapkan logo untuk Anda, yang dapat Anda unduh dari tautan ini: [**Unduh Logo Kalkulator**](https://i.imgur.com/oLsUiwH.png).
{{< /notice >}}

Ikuti langkah-langkah berikut untuk memperbarui logo:

1. Klik kanan pada folder `drawable` di proyek Anda, lalu klik New > **Image Asset**.
2. Di kolom **Path**, klik ikon **folder** dan pilih gambar yang telah Anda unduh sebelumnya.
3. Atur nilai **Resize** ke **50%**.
4. Selanjutnya, kita akan mengubah latar belakang. Klik pada **Background Layer**, ubah **Asset Type** menjadi **Color**, dan atur warnanya ke #FFFFFF (putih).
5. Setelah selesai, klik **Next** dan kemudian **Finish**.

Sekarang, jalankan aplikasi dan **voilà!** Aplikasi Anda akan terlihat jauh lebih baik dengan logo baru dan orientasi yang telah diperbaiki. Kerja bagus!
