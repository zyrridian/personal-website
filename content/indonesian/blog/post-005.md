---
title: "Membangun Aplikasi Resep Flutter: Bagian 1 - Layar Utama"
meta_title: "Membangun Aplikasi Resep Flutter"
description: "Pelajari cara membangun aplikasi resep yang indah dan fungsional menggunakan Flutter. Tutorial langkah demi langkah ini mencakup desain UI, daftar resep, dan fitur interaktif."
date: 2025-01-31T00:00:00Z
image: "images/blog/blog-005/img.png"
categories: ["Tutorial", "Flutter", "Mobile Development"]
author: "zyrridian"
tags: ["Flutter", "Dart", "Recipe App", "Mobile Development"]
draft: false
---

Dalam seri tutorial ini, kita akan membangun aplikasi resep modern yang bernama "Flavor Fusion" menggunakan Flutter. Aplikasi ini akan menampilkan desain yang bersih dengan layar utama yang menampilkan kategori resep dan resep populer, halaman detail resep, dan layar profil pengguna.

Di akhir artikel pertama ini, kita akan membuat layar utama yang fungsional dengan app bar kustom, kartu kategori, dan daftar resep. Mari kita mulai!

## Menyiapkan Proyek

Pertama, mari kita buat proyek Flutter baru dan menyiapkan struktur aplikasi utama. Kita akan mulai dengan mengkonfigurasi tema aplikasi, membuat model data, dan menyiapkan navigasi.

### Konfigurasi Aplikasi Utama

Mari kita mulai dengan file main.dart untuk menyiapkan tema aplikasi dan layar utama awal:

```dart
import 'package:flutter/material.dart';
import 'package:recipe_app/presentation/home/home_screen.dart';

void main() {
  runApp(const MainApp());
}

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Flavor Fusion',
      theme: ThemeData(
        primaryColor: Color(0xFF5C9F42),
        colorScheme: ColorScheme.fromSeed(
          seedColor: Color(0xFF5C9F42),
          brightness: Brightness.light,
        ),
        fontFamily: 'Poppins',
        useMaterial3: true,
      ),
      home: HomeScreen(),
    );
  }
}
```

Di sini kita mengkonfigurasi aplikasi dengan:
- Warna primer hijau (#5C9F42)
- Poppins sebagai font keluarga
- Sistem desain Material 3
- Tanpa banner debug
- HomeScreen sebagai titik awal

{{< image src="images/blog/blog-005/img1.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

### Membuat Layar Sementara

Sebelum kita membuat HomeScreen, kita perlu membuat layar sementara untuk daftar resep dan layar profil untuk menghindari error kompilasi. Mari buat file-file ini:

Pertama, buat `lib/presentation/list/recipe_list_screen.dart`:

```dart
import 'package:flutter/material.dart';

class RecipeListScreen extends StatelessWidget {
  const RecipeListScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text('Layar Daftar Resep - Segera Hadir'),
      ),
    );
  }
}
```

Kemudian, buat `lib/presentation/profile/profile_screen.dart`:

```dart
import 'package:flutter/material.dart';

class ProfileScreen extends StatelessWidget {
  const ProfileScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text('Layar Profil - Segera Hadir'),
      ),
    );
  }
}
```

Ini adalah layar sementara sederhana yang akan kita kembangkan secara detail di Bagian 2 dan 3 dari tutorial ini. Untuk saat ini, mereka hanya menampilkan pesan teks di tengah.

### Membuat Layar Utama

Sekarang setelah kita memiliki layar sementara, mari buat HomeScreen yang akan menangani navigasi antara layar daftar resep dan profil:

```dart
import 'package:flutter/material.dart';
import 'package:recipe_app/presentation/profile/profile_screen.dart';
import 'package:recipe_app/presentation/list/recipe_list_screen.dart';

class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  int _selectedIndex = 0;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: _selectedIndex == 0 ? RecipeListScreen() : ProfileScreen(),
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _selectedIndex,
        onTap: (index) {
          setState(() {
            _selectedIndex = index;
          });
        },
        selectedItemColor: Theme.of(context).primaryColor,
        unselectedItemColor: Colors.grey,
        items: [
          BottomNavigationBarItem(
            icon: Icon(Icons.restaurant_menu),
            label: 'Resep',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.person),
            label: 'Profil',
          ),
        ],
      ),
    );
  }
}
```

Ini membuat:
- Widget stateful untuk melacak tab mana yang dipilih
- Bottom navigation bar dengan dua opsi (Resep dan Profil)
- Logika untuk beralih antara layar berdasarkan tab yang dipilih

{{< image src="images/blog/blog-005/img2.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

### Menyiapkan Model Data

Sebelum kita masuk ke layar daftar resep, mari buat model Recipe dan beberapa data dummy:

```dart
// lib/data/recipe.dart
class Recipe {
  final String name;
  final String chef;
  final String imageUrl;
  final String cookTime;
  final String difficulty;
  final double rating;
  final List<String> ingredients;
  final List<String> steps;

  Recipe({
    required this.name,
    required this.chef,
    required this.imageUrl,
    required this.cookTime,
    required this.difficulty,
    required this.rating,
    required this.ingredients,
    required this.steps,
  });
}
```

Sekarang, mari isi beberapa data dummy untuk resep kita:

```dart
// lib/data/dummy.dart
import 'package:recipe_app/data/recipe.dart';

final List<Recipe> recipes = [
  Recipe(
    name: "Avocado Toast dengan Telur Rebus",
    chef: "Emma Wilson",
    imageUrl: "assets/images/avocado_toast.jpg",
    cookTime: "15 menit",
    difficulty: "Mudah",
    rating: 4.8,
    ingredients: [
      "2 potong roti gandum",
      "1 alpukat matang",
      "2 telur",
      "Tomat ceri",
      "Bubuk cabai merah",
      "Garam dan merica secukupnya",
      "Jus jeruk nipis segar",
    ],
    steps: [
      "Panggang roti hingga keemasan dan renyah.",
      "Hancurkan alpukat dan oleskan di atas roti.",
      "Rebus telur dalam air mendidih selama 3 menit.",
      "Tambahkan telur, irisan tomat, dan bumbu di atas roti.",
    ],
  ),
  // Resep lainnya...
];
```

Dengan model data kita di tempat, kita sekarang dapat fokus pada membangun layar daftar resep.

## Membangun Layar Daftar Resep

Sekarang, mari kita bangun layar daftar resep utama. Kita akan membuatnya langkah demi langkah:

### Membuat SliverAppBar

Kita akan mulai dengan membuat app bar kustom menggunakan SliverAppBar untuk pengalaman scrolling yang lebih dinamis:

```dart
import 'package:flutter/material.dart';
import 'package:recipe_app/data/dummy.dart';

class RecipeListScreen extends StatelessWidget {
  const RecipeListScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Color(0xFFF9F9F9),
      body: CustomScrollView(
        slivers: [
          SliverAppBar(
            expandedHeight: 120,
            floating: true,
            pinned: true,
            backgroundColor: Colors.white,
            elevation: 0,
            flexibleSpace: FlexibleSpaceBar(
              title: Text(
                'Flavor Fusion',
                style: TextStyle(
                  color: Colors.black87,
                  fontWeight: FontWeight.bold,
                ),
              ),
              centerTitle: false,
              titlePadding: EdgeInsets.only(left: 16, bottom: 16),
            ),
            actions: [
              IconButton(
                icon: Icon(Icons.search, color: Colors.black87),
                onPressed: () {},
              ),
              IconButton(
                icon: Icon(Icons.favorite_border, color: Colors.black87),
                onPressed: () {},
              ),
            ],
          ),
          // Kita akan menambahkan lebih banyak sliver di sini nanti
        ],
      ),
    );
  }
}
```

SliverAppBar memberi kita:
- Header yang dapat diperluas dengan nama aplikasi
- Ikon pencarian dan favorit
- Perilaku scrolling yang halus di mana header menciut saat Anda scroll ke bawah

{{< image src="images/blog/blog-005/img3.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

### Menambahkan Bagian Kategori

Selanjutnya, mari tambahkan bagian kategori dengan scrolling horizontal:

```dart
// Tambahkan sliver ini setelah SliverAppBar
SliverToBoxAdapter(
  child: Padding(
    padding: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
    child: Text(
      'Kategori',
      style: TextStyle(
        fontSize: 20,
        fontWeight: FontWeight.bold,
      ),
    ),
  ),
),
SliverToBoxAdapter(
  child: SizedBox(
    height: 120,
    child: ListView(
      padding: EdgeInsets.symmetric(horizontal: 8),
      scrollDirection: Axis.horizontal,
      children: [
        _buildCategoryCard('Sarapan', 'assets/images/breakfast.jpg', context),
        _buildCategoryCard('Makan Siang', 'assets/images/lunch.jpg', context),
        _buildCategoryCard('Makan Malam', 'assets/images/dinner.jpg', context),
        _buildCategoryCard('Makanan Penutup', 'assets/images/desserts.jpg', context),
        _buildCategoryCard('Smoothie', 'assets/images/smoothies.jpg', context),
      ],
    ),
  ),
),
```

Dan implementasikan metode _buildCategoryCard di kelas yang sama setelah blok Widget build():

```dart
Widget _buildCategoryCard(
  String title,
  String imageUrl,
  BuildContext context,
) {
  return Padding(
    padding: EdgeInsets.symmetric(horizontal: 8, vertical: 2),
    child: GestureDetector(
      onTap: () {},
      child: Container(
        width: 100,
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(15),
          color: Colors.white,
          boxShadow: [
            BoxShadow(
              color: Colors.black.withOpacity(0.05),
              blurRadius: 5,
              offset: Offset(0, 3),
            ),
          ],
        ),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Container(
              height: 60,
              width: 60,
              decoration: BoxDecoration(
                color: Theme.of(context).primaryColor.withOpacity(0.2),
                shape: BoxShape.circle,
              ),
              child: Icon(
                _getCategoryIcon(title),
                color: Theme.of(context).primaryColor,
                size: 30,
              ),
            ),
            SizedBox(height: 8),
            Text(
              title,
              style: TextStyle(
                fontWeight: FontWeight.w500,
                fontSize: 14,
              ),
            ),
          ],
        ),
      ),
    ),
  );
}

IconData _getCategoryIcon(String category) {
  switch (category) {
    case 'Sarapan':
      return Icons.free_breakfast;
    case 'Makan Siang':
      return Icons.lunch_dining;
    case 'Makan Malam':
      return Icons.dinner_dining;
    case 'Makanan Penutup':
      return Icons.cake;
    case 'Smoothie':
      return Icons.local_drink;
    default:
      return Icons.restaurant;
  }
}
```

Ini membuat:
- Daftar kartu kategori yang dapat di-scroll secara horizontal
- Setiap kartu memiliki ikon lingkaran dan label
- Ikon yang sesuai untuk setiap kategori makanan

{{< image src="images/blog/blog-005/img4.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

### Membuat Bagian Resep Populer

Sekarang mari tambahkan header untuk bagian resep populer:

```dart
// Tambahkan setelah bagian kategori
SliverToBoxAdapter(
  child: Padding(
    padding: EdgeInsets.only(left: 16, right: 16, top: 16, bottom: 0),
    child: Row(
      mainAxisAlignment: MainAxisAlignment.spaceBetween,
      children: [
        Text(
          'Resep Populer',
          style: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.bold,
          ),
        ),
        TextButton(
          onPressed: () {},
          child: Text('Lihat Semua'),
        ),
      ],
    ),
  ),
),
```

{{< image src="images/blog/blog-005/img5.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

### Membuat Widget Kartu Resep

Sebelum kita dapat menampilkan resep dalam daftar kita, kita perlu membuat widget RecipeCard yang dapat digunakan kembali. Buat file baru di `lib/presentation/list/components/recipe_card.dart`:

```dart
import 'package:flutter/material.dart';
import 'package:recipe_app/data/recipe.dart';

class RecipeCard extends StatelessWidget {
  final Recipe recipe;

  const RecipeCard({super.key, required this.recipe});

  @override
  Widget build(BuildContext context) {
    return Container(
      margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
      decoration: BoxDecoration(
        color: Colors.white,
        borderRadius: BorderRadius.circular(20),
        boxShadow: [
          BoxShadow(
            color: Colors.black.withOpacity(0.05),
            offset: Offset(0, 3),
            blurRadius: 10,
          ),
        ],
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Stack(
            children: [
              ClipRRect(
                borderRadius: BorderRadius.vertical(top: Radius.circular(20)),
                child: Container(
                  height: 180,
                  width: double.infinity,
                  color: Theme.of(context).primaryColor.withOpacity(0.1),
                  child: Icon(
                    Icons.image,
                    size: 80,
                    color: Theme.of(context).primaryColor.withOpacity(0.2),
                  ),
                ),
              ),
              Positioned(
                top: 12,
                right: 12,
                child: Container(
                  padding: EdgeInsets.symmetric(horizontal: 10, vertical: 6),
                  decoration: BoxDecoration(
                    color: Colors.white.withOpacity(0.9),
                    borderRadius: BorderRadius.circular(20),
                  ),
                  child: Row(
                    children: [
                      Icon(Icons.star, color: Colors.amber, size: 16),
                      SizedBox(width: 4),
                      Text(
                        '${recipe.rating}',
                        style: TextStyle(fontWeight: FontWeight.bold),
                      ),
                    ],
                  ),
                ),
              ),
            ],
          ),
          Padding(
            padding: EdgeInsets.all(16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  recipe.name,
                  style: TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                  ),
                  maxLines: 1,
                  overflow: TextOverflow.ellipsis,
                ),
                SizedBox(height: 6),
                Row(
                  children: [
                    CircleAvatar(
                      radius: 12,
                      backgroundColor: Theme.of(context).primaryColor.withOpacity(0.1),
                      child: Text(
                        recipe.chef.substring(0, 1),
                        style: TextStyle(
                          fontSize: 12,
                          color: Theme.of(context).primaryColor,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ),
                    SizedBox(width: 8),
                    Text(
                      recipe.chef,
                      style: TextStyle(color: Colors.grey),
                    ),
                  ],
                ),
                SizedBox(height: 12),
                Row(
                  children: [
                    _buildInfoChip(context, Icons.timer, recipe.cookTime),
                    SizedBox(width: 12),
                    _buildInfoChip(context, Icons.equalizer, recipe.difficulty),
                  ],
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildInfoChip(BuildContext context, IconData icon, String text) {
    return Container(
      padding: EdgeInsets.symmetric(horizontal: 10, vertical: 6),
      decoration: BoxDecoration(
        color: Theme.of(context).primaryColor.withOpacity(0.1),
        borderRadius: BorderRadius.circular(20),
      ),
      child: Row(
        children: [
          Icon(icon, size: 16, color: Theme.of(context).primaryColor),
          SizedBox(width: 4),
          Text(
            text,
            style: TextStyle(
              fontSize: 12,
              fontWeight: FontWeight.w500,
              color: Theme.of(context).primaryColor,
            ),
          ),
        ],
      ),
    );
  }
}
```

Ini membuat tata letak kartu yang indah untuk setiap resep dengan:
- Tempat untuk gambar resep
- Badge rating di pojok kanan atas
- Nama resep dan info chef
- Chip waktu memasak dan tingkat kesulitan

Sekarang kita dapat menggunakan widget RecipeCard ini di RecipeListScreen kita.

### Memperbarui Layar Daftar Resep

Sekarang setelah kita memiliki komponen RecipeCard, mari perbarui RecipeListScreen kita untuk menggunakannya. Perbarui file `lib/presentation/list/recipe_list_screen.dart`:

```dart
import 'package:flutter/material.dart';
import 'package:recipe_app/data/dummy.dart';
import 'package:recipe_app/presentation/list/components/recipe_card.dart';

class RecipeListScreen extends StatelessWidget {
  const RecipeListScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Color(0xFFF9F9F9),
      body: CustomScrollView(
        slivers: [
          SliverAppBar(
            expandedHeight: 120,
            floating: true,
            pinned: true,
            backgroundColor: Colors.white,
            elevation: 0,
            flexibleSpace: FlexibleSpaceBar(
              title: Text(
                'Flavor Fusion',
                style: TextStyle(
                  color: Colors.black87,
                  fontWeight: FontWeight.bold,
                ),
              ),
              centerTitle: false,
              titlePadding: EdgeInsets.only(left: 16, bottom: 16),
            ),
            actions: [
              IconButton(
                icon: Icon(Icons.search, color: Colors.black87),
                onPressed: () {},
              ),
              IconButton(
                icon: Icon(Icons.favorite_border, color: Colors.black87),
                onPressed: () {},
              ),
            ],
          ),
          SliverToBoxAdapter(
            child: Padding(
              padding: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
              child: Text(
                'Kategori',
                style: TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.bold,
                ),
              ),
            ),
          ),
          SliverToBoxAdapter(
            child: SizedBox(
              height: 120,
              child: ListView(
                padding: EdgeInsets.symmetric(horizontal: 8),
                scrollDirection: Axis.horizontal,
                children: [
                  _buildCategoryCard(
                      'Sarapan', 'assets/images/breakfast.jpg', context),
                  _buildCategoryCard(
                      'Makan Siang', 'assets/images/lunch.jpg', context),
                  _buildCategoryCard(
                      'Makan Malam', 'assets/images/dinner.jpg', context),
                  _buildCategoryCard(
                      'Makanan Penutup', 'assets/images/desserts.jpg', context),
                  _buildCategoryCard(
                      'Smoothie', 'assets/images/smoothies.jpg', context),
                ],
              ),
            ),
          ),
          SliverToBoxAdapter(
            child: Padding(
              padding: EdgeInsets.only(left: 16, right: 16, top: 16, bottom: 0),
              child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  Text(
                    'Resep Populer',
                    style: TextStyle(
                      fontSize: 20,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  TextButton(
                    onPressed: () {},
                    child: Text('Lihat Semua'),
                  ),
                ],
              ),
            ),
          ),
          SliverList(
            delegate: SliverChildBuilderDelegate(
              (context, index) {
                return RecipeCard(recipe: recipes[index]);
              },
              childCount: recipes.length,
            ),
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        backgroundColor: Theme.of(context).primaryColor,
        child: Icon(Icons.add, color: Colors.white),
      ),
    );
  }

  Widget _buildCategoryCard(
    String title,
    String imageUrl,
    BuildContext context,
  ) {
    return Padding(
      padding: EdgeInsets.symmetric(horizontal: 8, vertical: 2),
      child: GestureDetector(
        onTap: () {},
        child: Container(
          width: 100,
          decoration: BoxDecoration(
            borderRadius: BorderRadius.circular(15),
            color: Colors.white,
            boxShadow: [
              BoxShadow(
                color: Colors.black.withOpacity(0.05),
                blurRadius: 5,
                offset: Offset(0, 3),
              ),
            ],
          ),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Container(
                height: 60,
                width: 60,
                decoration: BoxDecoration(
                  color: Theme.of(context).primaryColor.withOpacity(0.2),
                  shape: BoxShape.circle,
                ),
                child: Icon(
                  _getCategoryIcon(title),
                  color: Theme.of(context).primaryColor,
                  size: 30,
                ),
              ),
              SizedBox(height: 8),
              Text(
                title,
                style: TextStyle(
                  fontWeight: FontWeight.w500,
                  fontSize: 14,
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  IconData _getCategoryIcon(String category) {
    switch (category) {
      case 'Sarapan':
        return Icons.free_breakfast;
      case 'Makan Siang':
        return Icons.lunch_dining;
      case 'Makan Malam':
        return Icons.dinner_dining;
      case 'Makanan Penutup':
        return Icons.cake;
      case 'Smoothie':
        return Icons.local_drink;
      default:
        return Icons.restaurant;
    }
  }
}
```

## Menjalankan Aplikasi

Dengan semua komponen ini di tempat, kita sekarang dapat menjalankan aplikasi kita dan melihat layar utama yang indah dalam aksi. Anda akan melihat layar dengan:

1. App bar yang dapat menciut dengan nama aplikasi dan ikon aksi
2. Daftar kategori makanan yang dapat di-scroll secara horizontal
3. Bagian "Resep Populer" yang menampilkan kartu resep
4. Tombol floating action untuk menambahkan resep baru
5. Bottom navigation bar untuk beralih antara layar

{{< image src="images/blog/blog-005/img6.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

## Menambahkan Lebih Banyak Resep

Sekarang setelah aplikasi Anda berjalan, Anda dapat menambahkan lebih banyak resep ke data dummy. Buka `lib/data/dummy.dart` dan tambahkan lebih banyak resep ke daftar `recipes`. Berikut contoh cara menambahkan resep lain:

```dart
// Tambahkan ini ke daftar recipes di lib/data/dummy.dart
Recipe(
  name: "Chicken Tikka Masala",
  chef: "Sarah Patel",
  imageUrl: "assets/images/chicken_tikka.jpg",
  cookTime: "45 menit",
  difficulty: "Sedang",
  rating: 4.7,
  ingredients: [
    "2 pon dada ayam",
    "1 cangkir yogurt",
    "2 sdm garam masala",
    "1 bawang bombay, potong dadu",
    "3 siung bawang putih",
    "1 kaleng saus tomat",
    "1 cangkir krim kental",
    "Ketumbar segar",
    "Garam dan merica secukupnya",
  ],
  steps: [
    "Marinasi ayam dalam yogurt dan bumbu selama 2 jam.",
    "Panggang ayam hingga hangus dan matang.",
    "Tumis bawang bombay dan bawang putih hingga lunak.",
    "Tambahkan saus tomat dan didihkan selama 10 menit.",
    "Tambahkan krim dan ayam yang sudah dimasak, didihkan selama 5 menit.",
    "Hias dengan ketumbar segar dan sajikan dengan nasi.",
  ],
),
```

Anda dapat menambahkan sebanyak mungkin resep yang Anda inginkan mengikuti pola ini. Setiap resep harus mencakup:
- Nama yang deskriptif
- Nama chef
- URL gambar (untuk saat ini, kita menggunakan placeholder)
- Waktu memasak
- Tingkat kesulitan
- Rating
- Daftar bahan
- Instruksi langkah demi langkah

Resep akan otomatis muncul di tampilan daftar aplikasi Anda. Coba tambahkan beberapa resep lagi untuk melihat tampilannya!

{{< image src="images/blog/blog-005/img7.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title" webp="false" >}}

## Kesimpulan

Dalam bagian pertama dari seri tutorial kita ini, kita telah membuat fondasi aplikasi resep kita, termasuk:

- Menyiapkan tema dan struktur aplikasi
- Membuat model data kita
- Membangun layar utama dengan navigasi
- Mengimplementasikan layar daftar resep dinamis dengan SliverAppBar
- Membuat kartu kategori dan kartu resep kustom

Di bagian selanjutnya, kita akan fokus pada membangun layar detail resep di mana pengguna dapat melihat resep lengkap dengan bahan dan langkah-langkahnya.

Tunggu bagian 2!

## Apa Selanjutnya

Di Bagian 2, kita akan membuat:
- Tampilan detail resep dengan gambar, deskripsi, dan metadata
- Daftar bahan dengan gaya kustom
- Instruksi memasak langkah demi langkah
- Tombol aksi untuk menyimpan dan berbagi resep

Selamat coding! 