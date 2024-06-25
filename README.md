# inventory_mobile

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

# TUGAS 7

1. 
a. Stateless Widgets :
	* Tidak memiliki state atau "keadaaan", tidak dapat berubah sendirinya dari sebuah aksi internal atau behavior lain.
	* Berubah karena event atau aksi dari entity luar dari parent widget dalam widget tree.
	* Memberikan kembali bagaimana mereka dibangun kepada beberapa parent widget dalam struktur.
	* Child widget akan menerima deskripsi dari parent widget dan tidak berubah oleh sendirinya.
	* Hanya memiliki property final terdefinisi dalam construction, dan hanya membutuhkan tersebut yang dibangun di layar device.

b. Stateful widget :
	* Mengubah deskripsinya dalam jangka waktu hidupnya/lifespannya.
	* Immutable, namun memiliki company State class yang merepresentasikan keadaan/state widget sekarang.

<br>
<br>

2. Widgets used :

MaterialApp: Biasanya digunakan untuk membungkus aplikasi dan menetapkan properti global seperti judul, tema, dan navigasi.

Scaffold: Menyediakan struktur dasar untuk sebuah halaman, termasuk AppBar dan body.

AppBar: Bagian atas aplikasi yang menampilkan judul atau teks tertentu.

SingleChildScrollView: Memungkinkan konten di dalamnya untuk discroll jika melebihi ukuran layar yang tersedia.

Padding: Menambahkan padding ke dalam widget.

Column: Menyusun widget secara vertikal.

GridView: Menampilkan widget dalam grid.

InkWell: Memberikan efek sentuhan responsif saat di-tap.

Container: Mengelompokkan widget, memungkinkan konfigurasi seperti padding, warna, dan lainnya.

Icon: Menampilkan ikon sesuai dengan nilai IconData yang diberikan.

Text: Menampilkan teks dengan berbagai konfigurasi seperti style, ukuran, warna, dll.

SnackBar: Pop-up pesan yang muncul untuk memberikan umpan balik singkat setelah suatu tindakan.

<br>
<br>

3. Step by step checklist implementation :

- Setup flutter dengan menginstallnya dan memenuhi dependensinya (Android SDK/Android Studio, Android licenses, dan Visual Studio) -> Setup path dalam device

- Create new folder untuk menampung proyek flutter baru :
`flutter assignments` -> Di dalamnya, buka cmd folder dan run command berikut :
	* `flutter create inventory_mobile`` 
ini akan membuat file - file proyek baru secara otomatis 

```
android/
ios/
lib/main.dart
test/
.gitignore
.metadata
.packages
pubspec.yaml
pubspec.lock
README.md
analysis_options.yaml
LICENSE
CNAME
```
- Pada `lib/main.dart` akan disediakan suatu template yang dapat digunakan untuk membangun aplikasi lebih lanjut (main.dart menampilkan sebuah template app dengan tombol dan counter dimana tombol akan increment counter pada layar)

- Pada `lib/` create new file dengan open notepad -> `ctrl + s` -> all files, save dengan nama `menu.dart`.
menu.dart akan digunakan sebagai tempat dimana interface menu diletakkan, main akan hanya memanggil function yang kita butuhkan saat dibutuhkan, atau abstraksi/pemisahan fitur.

- Ubah main.dart menjadi

```dart
// Import 

import 'package:flutter/material.dart';
import 'package:inventory_mobile/menu.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) { // Tambahkan 
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(

        colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo), //Default color scheme, will be overridden by menu.dart
        useMaterial3: true,
      ),
      home: MyHomePage(),
    );
  }
}
```

dan menu.dart menjadi :

```dart
// Tambahkan import

import 'package:flutter/material.dart';

class MyHomePage extends StatelessWidget {
  MyHomePage({Key? key}) : super(key: key);

  final List<ShopItem> items = [

    // Panggil tiap constructor yang sudah kita define di bawah
      ShopItem.lihatItem, 
      ShopItem.tambahItem,
      ShopItem.logout,
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'Yharnam.OS',
        ),
      ),
      body: SingleChildScrollView(
        child: Padding(
          padding: const EdgeInsets.all(10.0), 
          child: Column(
            children: <Widget>[
              const Padding(
                padding: EdgeInsets.only(top: 10.0, bottom: 10.0),
                child: Text(
                  '✦ Inventory ✦', 
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    fontSize: 30,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              // Grid layout
              GridView.count(
                primary: true,
                padding: const EdgeInsets.all(20),
                crossAxisSpacing: 10,
                mainAxisSpacing: 10,
                crossAxisCount: 3,
                shrinkWrap: true,
                children: items.map((ShopItem item) {
                  return ShopCard(item);
                }).toList(),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class ShopItem {
  
  // Define attribute untuk tiap tombol yang akan di buat
  final String name;
  final IconData icon;
  final Color color;

  // Constructor Tombol
  ShopItem(this.name, this.icon, this.color);

  // Define setiap constructor dengan benar sesuai format. Bedakan tiap warna dan attribute lainnya untuk setiap tombol
  static final ShopItem lihatItem = ShopItem("Lihat Item", Icons.checklist, Colors.cyan[400]!);
  static final ShopItem tambahItem = ShopItem("Tambah Item", Icons.add_box, Colors.cyan[600]!);
  static final ShopItem logout = ShopItem("Logout", Icons.run_circle_outlined, Colors.cyan[800]!);
}

class ShopCard extends StatelessWidget {
  final ShopItem item;

  const ShopCard(this.item, {super.key}); // Constructor Snackbar

  @override
  Widget build(BuildContext context) {
    return Material(
      color: item.color,
      child: InkWell(
        // Area responsive terhadap sentuhan
        onTap: () {
          // Memunculkan SnackBar ketika diklik
          ScaffoldMessenger.of(context)
            ..hideCurrentSnackBar()
            ..showSnackBar(SnackBar(
                content: Text("Kamu telah menekan tombol ${item.name}!"))); // Snack bar memunculkan text sesuai nama item/ tombol yang di tekan
        },
        child: Container(
          // Container untuk menyimpan Icon dan Text
          padding: const EdgeInsets.all(8),
          child: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(
                  item.icon,
                  color: Colors.white,
                  size: 30.0,
                ),
                const Padding(padding: EdgeInsets.all(3)),
                Text(
                  item.name,
                  textAlign: TextAlign.center,
                  style: const TextStyle(color: Colors.white),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

- App Flutter dapat dijalankan dengan :
Membuka folder app (`inventory_mobile`) -> run cmd -> jalankan perintah `flutter run` -> saat diberi pilihan, pilihlah sesuai kebutuhan, misal untuk ini chrome, tekan `2`, program akan memakai waktu sedikit lama untuk compile code dan connect ke browser pilihan

- Tampilan akan terlihat seperti berikut :

![Alt text](image.png)

- Jika sudah selesai operasi app, matikan dengan perintah `q` pada cmd (sesuai pada cmd)

- create new repo, git init, git add ., git commit, git push

<br>
<br>
<br>

# TUGAS 8

<br>

1. Jelaskan perbedaan antara Navigator.push() dan Navigator.pushReplacement(), disertai dengan contoh mengenai penggunaan kedua metode tersebut yang tepat!

- `.push()` : Menambahkan layar baru diatas stack/di atas layar saat ini. Terdapat tombol back pada layar baru (atas kiri), akan digantikan dengan tombol drawer jika ada implementasi drawer pada aplikasi

```dart
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => SecondScreen()),
);
```

- `.pushReplacement()` : Menggantikan layar yang sedang aktif dengan layar baru dalam fungsi `.pushReplacement()`, layar baru akan menjadi "current screen" 

```dart
Navigator.pushReplacement(
  context,
  MaterialPageRoute(builder: (context) => ThirdScreen()),
);
```

<br>

2. Jelaskan masing-masing layout widget pada Flutter dan konteks penggunaannya masing-masing!

- Container :
  * Sebuah box yang dapat diisi dengan widget lain
  * Mmeliki properti lain untuk styling seperti margin dan decoration
- Row and Column : 
  * "flex widget" yang dapat mengurutkan isinya/childnya secara vertikal atau horizontal (row/column)
- ListView
  * Menampilkan child sebagai widget yang dapat discroll
  * Dibuat secara dinamik menggunakan function builder, mengurangi memory consumption 
- GridView
  * Seperti Listview
  * Scrollable grid of widgets
  * Childrennya dapat diarrange seperti 2 dimensional array
- Stack
  * Children ditumpuk di atas satu sama lain
  * Dapat diposisikan relatif edges stack atau satu sama lain

<br>

3. Sebutkan apa saja elemen input pada form yang kamu pakai pada tugas kali ini dan jelaskan mengapa kamu menggunakan elemen input tersebut!

- TextFormField
  * Widget yang menyediakan kolom input teks.
  * Biasanya digunakan untuk menerima input berupa text.
  * Dapat didefinisikan kontrol (controllers) untuk mengelola status input dan menggunakan callback untuk menangani perubahan atau pengiriman formulir.

- ElevatedButton
  * Tombol/button yang tampak memiliki bayangan (elevated)
  * `child` dapat berisi widget lagi, dapat berupa text atau semacamnya
  * `onPressed` dapat diisi dengan fungsi ketika tombol dipencet

<br>

4. Bagaimana penerapan clean architecture pada aplikasi Flutter?

Clean architecture pada flutter, secara singkat, merupakan metode penerapan prinsip dari Uncle Bob tentang pembagian tugas dalam suatu aplikasi atau "Seperation of Concern" seperti MVP, MVT, atau MVCC, yang diterapkan kepada development aplikasi flutter.

Pembagian :

- Domain : Berisi business logic aplikasi, ditulis full dalam dart tanpa ada elemen flutter, digunakan agar lebih mudah melakukan migrasi antar platform.
  * Entities
  * Usecases
  * Repositories

- App : Perantara yang menyambungkan layer lain dengan Domain.
  * View
  * Controller
  * Presenter
  * Extra

- Data : Data retrival dari database.
  * Repositories
  * Models
  * Mappers
  * Extra

- Device : Berkomunikasi langsung dengan platform Android dan IOS, bertanggungjawab atas fungsional native seperti GPS.
  * Devices
  * Extra


5. Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step! (bukan hanya sekadar mengikuti tutorial)

- Tambahkan layar dan tampilan form baru untuk mengisi data item :
`lib` -> create new folder `screens` -> create new file `item_form.dart` :

```dart
import 'package:flutter/material.dart';
import 'package:inventory_mobile/widgets/left_drawer.dart';
// TODO: Impor drawer yang sudah dibuat sebelumnya

class ShopFormPage extends StatefulWidget {
    const ShopFormPage({super.key});

    @override
    State<ShopFormPage> createState() => _ShopFormPageState();
}

class _ShopFormPageState extends State<ShopFormPage> {
  final _formKey = GlobalKey<FormState>();
  String _name = "";
  int _amount = 0;
  String _description = "";
  List<Map<String, dynamic>> itemList = [];
  
    @override
    Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: const Center(
              child: Text(
                'Form Tambah Produk',
              ),
            ),
            backgroundColor: Colors.amber[200]!,
            foregroundColor: Colors.black,
          ),
          // TODO: Tambahkan drawer yang sudah dibuat di sini
          drawer: const LeftDrawer(),
          body: Form(
            key: _formKey,
            child: SingleChildScrollView(
               child: Column(      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Padding(
          padding: const EdgeInsets.all(8.0),
          child: TextFormField(
            decoration: InputDecoration(
              hintText: "Nama Item",
              labelText: "Nama Item",
              border: OutlineInputBorder(
                borderRadius: BorderRadius.circular(5.0),
              ),
            ),
            onChanged: (String? value) {
              setState(() {
                _name = value!;
              });
            },
            validator: (String? value) {
              if (value == null || value.isEmpty) {
                return "Nama tidak boleh kosong!";
              }
              return null;
            },
          ),
        ),
        Padding(
  padding: const EdgeInsets.all(8.0),
  child: TextFormField(
    decoration: InputDecoration(
      hintText: "Amount",
      labelText: "Amount",
      border: OutlineInputBorder(
        borderRadius: BorderRadius.circular(5.0),
      ),
    ),
    // TODO: Tambahkan variabel yang sesuai
    onChanged: (String? value) {
      setState(() {
        _amount = int.parse(value!);
      });
    },
    validator: (String? value) {
      if (value == null || value.isEmpty) {
        return "Quantity cannot be empty!";
      }
      if (int.tryParse(value) == null) {
        return "Quantity is a number...";
      }
      return null;
    },
  ),
),
Padding(
  padding: const EdgeInsets.all(8.0),
  child: TextFormField(
    decoration: InputDecoration(
      hintText: "Deskripsi",
      labelText: "Deskripsi",
      border: OutlineInputBorder(
        borderRadius: BorderRadius.circular(5.0),
      ),
    ),
    onChanged: (String? value) {
      setState(() {
        // TODO: Tambahkan variabel yang sesuai
        _description = value!;
      });
    },
    validator: (String? value) {
      if (value == null || value.isEmpty) {
        return "Deskripsi tidak boleh kosong!";
      }
      return null;
    },
  ),
),
    Align(
  alignment: Alignment.bottomCenter,
  child: Padding(
    padding: const EdgeInsets.all(8.0),
    child: ElevatedButton(
      style: ButtonStyle(
        backgroundColor:
            MaterialStateProperty.all(Colors.amber[400]!),
      ),
          onPressed: () {
            if (_formKey.currentState!.validate()) {
              setState(() {
                itemList.add({
                  'Nama': _name,
                  'Amount': _amount,
                  'Deskripsi': _description,
                });
              });
              showDialog(
                context: context,
                builder: (context) {
                  return AlertDialog(
                    title: const Text('Produk berhasil tersimpan'),
                    content: SingleChildScrollView(
                      child: Column(
                        crossAxisAlignment:
                            CrossAxisAlignment.start,
                        children: [
                          Text('Nama: $_name'),
                          Text('Harga: $_amount'),
                          Text('Deskripsi: $_description'),
                          // TODO: Munculkan value-value lainnya
                        ],
                      ),
                    ),
                    actions: [
                      TextButton(
                        child: const Text('OK'),
                        onPressed: () {
                          Navigator.pop(context);
                        },
                      ),
                    ],
                  );
                },
              );
            }
            // _formKey.currentState!.reset();
          },
          child: const Text(
        "Save",
        style: TextStyle(color: Colors.black),
      ),
    ),
  ),
),
          ]
              ),
            ),
          ),
        );
    }
}
```

- Pindahkan `menu.dart` ke dalam folder `lib` -> `screens` :

```dart
import 'package:flutter/material.dart';
// import 'package:shopping_list/shoplist_form.dart';
import 'package:inventory_mobile/widgets/item_card.dart';
import 'package:inventory_mobile/widgets/left_drawer.dart';

class MyHomePage extends StatelessWidget {
  MyHomePage({Key? key}) : super(key: key);

  final List<ShopItem> items = [
      ShopItem.lihatItem, // Use the predefined instances with different colors
      ShopItem.tambahItem,
      ShopItem.logout,
  ];

  // This widget is the home page of your application. It is stateful, meaning
  // that it has a State object (defined below) that contains fields that affect
  // how it looks.

  // This class is the configuration for the state. It holds the values (in this
  // case the title) provided by the parent (in this case the App widget) and
  // used by the build method of the State. Fields in a Widget subclass are
  // always marked "final".

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.amber[200]!,
        title: const Text(
          'Yharnam.OS',
        ),
      ),
       drawer: const LeftDrawer(),
      body: SingleChildScrollView(
        // Widget wrapper yang dapat discroll
        child: Padding(
          padding: const EdgeInsets.all(10.0), // Set padding dari halaman
          child: Column(
            // Widget untuk menampilkan children secara vertikal
            children: <Widget>[
              const Padding(
                padding: EdgeInsets.only(top: 10.0, bottom: 10.0),
                // Widget Text untuk menampilkan tulisan dengan alignment center dan style yang sesuai
                child: Text(
                  '✦ Inventory ✦', // Text yang menandakan toko
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    fontSize: 30,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              // Grid layout
              GridView.count(
                // Container pada card kita.
                primary: true,
                padding: const EdgeInsets.all(20),
                crossAxisSpacing: 10,
                mainAxisSpacing: 10,
                crossAxisCount: 3,
                shrinkWrap: true,
                children: items.map((ShopItem item) {
                  // Iterasi untuk setiap item
                  return ShopCard(item);
                }).toList(),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

```

- Pisahkan bagian `ShopItem` dan `ShopCard` yang tadinya pada `menu.dart` ke file baru :

`lib` -> create new folder `widgets` -> create new file to store the said stuff `item_card.dart` :

```dart
// Isi item_card.dart

import 'package:flutter/material.dart';
import 'package:inventory_mobile/screens/item_form.dart';

class ShopItem {
  final String name;
  final IconData icon;
  final Color color;

  ShopItem(this.name, this.icon, this.color);

  static final ShopItem lihatItem = ShopItem("Lihat Item", Icons.checklist, Colors.cyan[400]!);
  static final ShopItem tambahItem = ShopItem("Tambah Item", Icons.add_box, Colors.cyan[600]!);
  static final ShopItem logout = ShopItem("Logout", Icons.run_circle_outlined, Colors.cyan[800]!);
}

class ShopCard extends StatelessWidget {
  final ShopItem item;

  const ShopCard(this.item, {super.key}); // Constructor

  @override
  Widget build(BuildContext context) {
    return Material(
      color: item.color,
      child: InkWell(
        // Area responsive terhadap sentuhan
        onTap: () {
          // Memunculkan SnackBar ketika diklik
          ScaffoldMessenger.of(context)
            ..hideCurrentSnackBar()
            ..showSnackBar(SnackBar(
                content: Text("Kamu telah menekan tombol ${item.name}!")));

          // Navigate ke route yang sesuai (tergantung jenis tombol)
            if (item.name == "Tambah Item") {
              // TODO: Gunakan Navigator.push untuk melakukan navigasi ke MaterialPageRoute yang mencakup ShopFormPage.
              Navigator.push(
                    context,
                    MaterialPageRoute(
                      builder: (context) => ShopFormPage(),
                    ));
            }

        },
        child: Container(
          // Container untuk menyimpan Icon dan Text
          padding: const EdgeInsets.all(8),
          child: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(
                  item.icon,
                  color: Colors.white,
                  size: 30.0,
                ),
                const Padding(padding: EdgeInsets.all(3)),
                Text(
                  item.name,
                  textAlign: TextAlign.center,
                  style: const TextStyle(color: Colors.black),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

- Tambahkan drawer atau yang tiga diatas biasanya itu untuk pindah - pindah layar :

`drawer` akan menjadi cara user untuk pindah - pindah layar dan fitur menggunakan `navigation.push()` functions :

```dart
import 'package:flutter/material.dart';
import 'package:inventory_mobile/screens/menu.dart';
import 'package:inventory_mobile/screens/item_form.dart';
// TODO: Impor halaman ShopFormPage jika sudah dibuat

class LeftDrawer extends StatelessWidget {
  const LeftDrawer({super.key});

  @override
  Widget build(BuildContext context) {
    return Drawer(
      child: ListView(
        children: [
          const DrawerHeader(
            // TODO: Bagian drawer header
              decoration: BoxDecoration(
              color: Colors.amber,
            ),
      child: Column(
        children: [
            Text(
                  'Shopping List',
                  textAlign: TextAlign.center,
                  style: TextStyle(
                  fontSize: 30,
                  fontWeight: FontWeight.bold,
                  color: Colors.black,
                  ),
                ),
      Padding(padding: EdgeInsets.all(10)),
      Text("Catat seluruh keperluan belanjamu di sini!",
                  textAlign: TextAlign.center,
                  style: TextStyle(
                  fontSize: 15,
                  fontWeight: FontWeight.normal,
                  color: Colors.black,
                  ),
            ),
    ],
            ),
          ),
          // TODO: Bagian routing
          ListTile(
              leading: const Icon(Icons.home_outlined),
              title: const Text('Halaman Utama'),
              // Bagian redirection ke MyHomePage
              onTap: () {
                Navigator.pushReplacement(
                    context,
                    MaterialPageRoute(
                      builder: (context) => MyHomePage(),
                    ));
              },
              ),
              ListTile(
              leading: const Icon(Icons.add_shopping_cart),
              title: const Text('Tambah Produk'),
              // Bagian redirection ke ShopFormPage
              onTap: () {
                /*
                TODO: Buatlah routing ke ShopFormPage di sini,
                setelah halaman ShopFormPage sudah dibuat.
                */
                Navigator.push(
                    context,
                    MaterialPageRoute(
                      builder: (context) => const ShopFormPage(),
                    ));
              },
              ),
        ],
      ),
    );
  }
}
```

<br>

Sekarang drawer dapat diimplementasi kedalam tiap layar untuk memberikan "isi access" untuk user melakukan pindah - pindah layar/fitur.

# TUGAS 9

<br>

1. Apakah bisa kita melakukan pengambilan data JSON tanpa membuat model terlebih dahulu? Jika iya, apakah hal tersebut lebih baik daripada membuat model sebelum melakukan pengambilan data JSON?

ans : 
Ya, dengan beberapa syarat berikut :
- Jika hanya mengambil data dari sebuah file JSON atau API dan tidak perlu melakukan manipulasi atau analisis yang kompleks, kita tidak perlu membuat model khusus.
- Jika hanya butuh untuk mengambil dan menganalisis data JSON secara sederhana, maka tidak perlu membuat model terlebih dahulu.
- Jika ingin melakukan tugas seperti klasifikasi atau prediksi berdasarkan data JSON, maka kita mungkin perlu membuat model terlebih dahulu. 
- Jika struktur kompleks maka di sarankan untuk membangun model untuk membantu manajemen kompleksitas struktur data.

<br>

2. Jelaskan fungsi dari CookieRequest dan jelaskan mengapa instance CookieRequest perlu untuk dibagikan ke semua komponen di aplikasi Flutter.

ans: 
CookieRequest digunakan untuk mengatur berbagai request HTTP dan cookie dalam Flutter. Menggunakan instance CookieRequest melalui Provider, informasi autentikasi dan sesi user dapat diakses di seluruh aplikasi.
Provider juga dapat dengan mudah manajemen pengaksesan data sesi atau user.

<br>

3. Jelaskan mekanisme pengambilan data dari JSON hingga dapat ditampilkan pada Flutter.

ans : 
- Buat model item dan mengambil endpointnya melalui `/json/id/`. Lalu masukan data ke dalam quicktype dan set untuk `dart` dan `json`. Copy code yang disediakan oleh quicktype ke file baru `item.dart` pada folder `models`.
- add new dependencies `flutter pub add http` 
- add data fetch requirement to `android/app/src/main/AndoridManifest.xml` :
```xml
...
  <application>
  ...
  </application>
  <!-- Required to fetch data from the Internet. -->
  <uses-permission android:name="android.permission.INTERNET" />
...
```
- Create new file `lib` -> `screens` -> `item_show.dart`, dan isi dengan kode yang tertera pada StepByStep.
(Penjelasan lebih lanjut pada StepByStep)

<br>

4. Jelaskan mekanisme autentikasi dari input data akun pada Flutter ke Django hingga selesainya proses autentikasi oleh Django dan tampilnya menu pada Flutter.

ans :
  * Meminta input mengenai username dan password di Flutter.
  * Menggunakan metode http.post untuk mengirim informasi data login user ke server Django
  * Setelah django mendapatkan data informasi login, django akan memverifikasi informasi login. Jika valid, maka django akan mengembalikan token autentikasi.
  * Jika autentikasi berhasil, tampilan menu atau akses ke fitur tertentu yang memerlukan autentikasi akan di tampilkan di aplikasi flutter.

<br>

5. Sebutkan seluruh widget yang kamu pakai pada tugas ini dan jelaskan fungsinya masing-masing.

ans :
  * `http`: Ditambahkan sebagai dependensi untuk membuat permintaan HTTP
  * `FutureBuilder`: Digunakan untuk membangun widget secara asinkron berdasarkan snapshot terbaru dari data.
  * `Provider` : Digunakan untuk mengelola dan menyediakan state dari kelas CookieRequest ke semua widget anak.
  * `Elevated Button`: Untuk membuat button
  * `Text`: Menampilkan teks
  * `SizedBox`: Memberikan space diantara widget
  * `ListView`: Digunakan untuk loop item

## Step by Step

1. Django deployment success
- In process, if deployment still has problems, resort to localhost 🙏, apologies in advanced.

2. Login, Register, Logout page for Flutter
- Create new django-app in `inventory_project` (prev work)
- Create `authentication` in `inventory_project` -> `settings.py` -> `INSTALLED_APPS`
- Run `pip install django-cors-headers` in `inventory_mobile` to get cors headers packages
- add  :
  * `settings.py` -> `INSTALLED_APPS` add-> `corsheaders`
  * `settings.py` -> `MIDDLEWARE` add-> `corsheaders.middleware.CorsMiddleware`
  * `settings.py` -> :

```python
CORS_ALLOW_ALL_ORIGINS = True
CORS_ALLOW_CREDENTIALS = True
CSRF_COOKIE_SECURE = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SAMESITE = 'None'
SESSION_COOKIE_SAMESITE = 'None'
```

- add functions for Login, Logout, & Register ->
  * authentication/views.py :
  
```python
# IMPORTS
from django.shortcuts import render
from django.contrib.auth import authenticate, login as auth_login
from django.http import HttpResponse, JsonResponse
from django.views.decorators.csrf import csrf_exempt
from django.contrib.auth import logout as auth_logout
from django.contrib.auth.models import User
from django.core import serializers
from main.models import Item

user = None # Set user to None

# REGISTER 
@csrf_exempt
def register(request):
    
    username = request.POST.get('username')
    password = request.POST.get('password')

    if User.objects.filter(username=username).exists():
        return JsonResponse({
            "status": False,
            "message": "Username sudah digunakan. Pilih username lain."
        }, status=400)

    # Buat user baru tanpa menggunakan email
    user = User.objects.create_user(username=username, password=password)

    return JsonResponse({
        "username": user.username,
        "status": True,
        "message": "Registrasi berhasil!"
    }, status=201)


# LOGIN
@csrf_exempt
def login(request):
    username = request.POST['username']
    password = request.POST['password']
    user = authenticate(username=username, password=password)
    if user is not None:
        if user.is_active:
            auth_login(request, user)
            # Status login sukses.
            return JsonResponse({
                "username": user.username,
                "status": True,
                "message": "Login sukses!"
                # Tambahkan data lainnya jika ingin mengirim data ke Flutter.
            }, status=200)
        else:
            return JsonResponse({
                "status": False,
                "message": "Login gagal, akun dinonaktifkan."
            }, status=401)

    else:
        return JsonResponse({
            "status": False,
            "message": "Login gagal, periksa kembali email atau kata sandi."
        }, status=401)
    
# LOGOUT
from django.contrib.auth import logout as auth_logout
...
@csrf_exempt
def logout(request):
    username = request.user.username

    try:
        auth_logout(request)
        return JsonResponse({
            "username": username,
            "status": True,
            "message": "Logout berhasil!"
        }, status=200)
    except:
        return JsonResponse({
        "status": False,
        "message": "Logout gagal."
        }, status=401)
```

- add `views.py` urls to `urls.py` :

```python
from django.urls import path
from authentication.views import login,logout, register

app_name = 'authentication'

urlpatterns = [
    path('login/', login, name='login'),
    path('logout/', logout, name='logout'),
    path('register/', register, name='register'),
]
```

- Install more packages for flutter (install these in `inventory_mobile`) :
```bash
flutter pub add provider
flutter pub add pbp_django_auth
```

- add new stuff to flutter app, `inventory_mobile` -> `main.dart`
  * Cookie request :
  ```dart
      class MyApp extends StatelessWidget {
      const MyApp({Key? key}) : super(key: key);

      @override
      Widget build(BuildContext context) {
          return Provider(
              create: (_) {
                  CookieRequest request = CookieRequest(); // NEW
                  return request;
              },
              child: MaterialApp(
                  title: 'Flutter App',
                  theme: ThemeData(
                      colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
                      useMaterial3: true,
                  ),
                  home: MyHomePage()
              ),
          );
      }
  }
  ```

  * Create a new file, `screens` Create-> `login.dart` :
  ```dart
  import 'package:inventory_mobile/screens/menu.dart';
  import 'package:flutter/material.dart';
  import 'package:pbp_django_auth/pbp_django_auth.dart';
  import 'package:provider/provider.dart';

  void main() {
      runApp(const LoginApp());
  }

  class LoginApp extends StatelessWidget {
  const LoginApp({super.key});

  @override
  Widget build(BuildContext context) {
      return MaterialApp(
          title: 'Login',
          theme: ThemeData(
              primarySwatch: Colors.blue,
      ),
      home: const LoginPage(),
      );
      }
  }

  class LoginPage extends StatefulWidget {
      const LoginPage({super.key});

      @override
      _LoginPageState createState() => _LoginPageState();
  }

  class _LoginPageState extends State<LoginPage> {
      final TextEditingController _usernameController = TextEditingController();
      final TextEditingController _passwordController = TextEditingController();

      @override
      Widget build(BuildContext context) {
          final request = context.watch<CookieRequest>();
          return Scaffold(
              appBar: AppBar(
                  title: const Text('Login'),
              ),
              body: Container(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                          TextField(
                              controller: _usernameController,
                              decoration: const InputDecoration(
                                  labelText: 'Username',
                              ),
                          ),
                          const SizedBox(height: 12.0),
                          TextField(
                              controller: _passwordController,
                              decoration: const InputDecoration(
                                  labelText: 'Password',
                              ),
                              obscureText: true,
                          ),
                          const SizedBox(height: 24.0),
                          ElevatedButton(
                              onPressed: () async {
                                  String username = _usernameController.text;
                                  String password = _passwordController.text;
                                  
                                  final response = await request.login("http://127.0.0.1:8000/auth/login/", {
                                  'username': username,
                                  'password': password,
                                  });
                      
                                  if (request.loggedIn) {
                                      String message = response['message'];
                                      String uname = response['username'];
                                      Navigator.pushReplacement(
                                          context,
                                          MaterialPageRoute(builder: (context) => MyHomePage()),
                                      );
                                      ScaffoldMessenger.of(context)
                                          ..hideCurrentSnackBar()
                                          ..showSnackBar(
                                              SnackBar(content: Text("$message Selamat datang, $uname.")));
                                      } else {
                                      showDialog(
                                          context: context,
                                          builder: (context) => AlertDialog(
                                              title: const Text('Login Gagal'),
                                              content:
                                                  Text(response['message']),
                                              actions: [
                                                  TextButton(
                                                      child: const Text('OK'),
                                                      onPressed: () {
                                                          Navigator.pop(context);
                                                      },
                                                  ),
                                              ],
                                          ),
                                      );
                                  }
                              },
                              child: const Text('Login'),
                          ),
                      ],
                  ),
              ),
          );
      }
  }
  ```

  * `main.dart` change-> from `home: MyHomePage()` to `home: LoginPage()`

3. Integrate Django project models to Flutter
- Open JSON endpoint on `inventory_project`, runserver -> add-> /json/4 to link, retrieve the json output
add json paste to quicktype, alter the settings to JSON and dart compatible

- Create `inventory_mobile` -> `lib` -> `models` -> `item.dart`, copy the quicktype code to `item.dart` with no alterations

- Create new page to display items in possession:
  * `screens` Create-> `item_show.dart` :
  ```dart
  import 'package:flutter/material.dart';
  import 'package:http/http.dart' as http;
  import 'dart:convert';
  import 'package:inventory_mobile/models/item.dart';
  import 'package:inventory_mobile/screens/item_detail.dart';
  import 'package:inventory_mobile/widgets/left_drawer.dart';

  class ItemListPage extends StatefulWidget {
      const ItemListPage({Key? key}) : super(key: key);

      @override
      _itemPageState createState() => _itemPageState();
  }

  class _itemPageState extends State<ItemListPage> {
  Future<List<Item>> fetchitem() async {
      // TODO: Ganti URL dan jangan lupa tambahkan trailing slash (/) di akhir URL!
      var url = Uri.parse(
          'http://127.0.0.1:8000/json/');
      var response = await http.get(
          url,
          headers: {"Content-Type": "application/json"},
      );

      // melakukan decode response menjadi bentuk json
      var data = jsonDecode(utf8.decode(response.bodyBytes));

      // melakukan konversi data json menjadi object item
      List<Item> list_item = [];
      for (var d in data) {
          if (d != null) {
              list_item.add(Item.fromJson(d));
          }
      }
      return list_item;
  }

  @override
  Widget build(BuildContext context) {
      return Scaffold(
          appBar: AppBar(
          title: const Text('Item'),
          ),
          drawer: const LeftDrawer(),
          body: FutureBuilder(
              future: fetchitem(),
              builder: (context, AsyncSnapshot snapshot) {
                  if (snapshot.data == null) {
                      return const Center(child: CircularProgressIndicator());
                  } else {
                      if (!snapshot.hasData) {
                      return const Column(
                          children: [
                          Text(
                              "Tidak ada data item.",
                              style:
                                  TextStyle(color: Color(0xff59A5D8), fontSize: 20),
                          ),
                          SizedBox(height: 8),
                          ],
                      );
                  } else {
                      return ListView.builder(
                          itemCount: snapshot.data!.length,
                          itemBuilder: (_, index) => Container(
                                  margin: const EdgeInsets.symmetric(
                                      horizontal: 16, vertical: 12),
                                  padding: const EdgeInsets.all(20.0),
                                  child: Column(
                                  mainAxisAlignment: MainAxisAlignment.start,
                                  crossAxisAlignment: CrossAxisAlignment.start,
                                  children: [
                                      Text(
                                      "${snapshot.data![index].fields.name}",
                                      style: const TextStyle(
                                          fontSize: 18.0,
                                          fontWeight: FontWeight.bold,
                                      ),
                                      ),
                                      const SizedBox(height: 10),
                                      Text("${snapshot.data![index].fields.omen}"),
                                      const SizedBox(height: 10),
                                      Text("${snapshot.data![index].fields.space}"),
                                      const SizedBox(height: 10),
                                      Text(
                                          "${snapshot.data![index].fields.description}"),
                                      const SizedBox(height: 10),
                                      Text("${snapshot.data![index].fields.amount}"),
                                      ElevatedButton(
                                          onPressed: () {
                                          Navigator.push(
                                            context,
                                            MaterialPageRoute(
                                              builder: (context) => DetailItemPage(item: snapshot.data![index]),
                                            ),
                                          );
                                        },
                                        child: const Text('Item Details'),
                                      ),
                                  ],
                                  ),
                              ));
                      }
                  }
              }));
      }
  }
  ``` 

  * Display individual item data :
  add to `item_show.dart`
  ```dart
  ElevatedButton(
      onPressed: () {
      Navigator.push(
        context,
        MaterialPageRoute(
          builder: (context) => DetailItemPage(item: snapshot.data![index]),
        ),
      );
    },
    child: const Text('Detail Item'),
  ),
  ```
  Create new file `screens` -> `item_detail.dart` :
  ```dart
  import 'package:flutter/material.dart';
  import 'package:inventory_mobile/models/item.dart';
  import 'package:inventory_mobile/widgets/left_drawer.dart';

  class DetailItemPage extends StatefulWidget {
      final Item item;

      const DetailItemPage({super.key, required this.item});

      @override
      State<DetailItemPage> createState() => _DetailItemPageState();
  }

  class _DetailItemPageState extends State<DetailItemPage> {
    @override
    Widget build(BuildContext context) {
      return Scaffold(
            appBar: AppBar(
              title: const Center(
                child: Text(
                  'Detail Item',
                ),
              ),
              backgroundColor: Colors.indigo,
              foregroundColor: Colors.white,
            ),
        drawer: const LeftDrawer(),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: [
              const Text(
                'Item Name:',
                style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
              ),
              Text(
                widget.item.fields.name,
                style: const TextStyle(fontSize: 36, fontWeight: FontWeight.bold),
              ),
              const SizedBox(height: 20),
              Text(
                'Omen: ${widget.item.fields.omen}',
                style: const TextStyle(fontSize: 18),
              ),
                          Text(
                'Space: ${widget.item.fields.space}',
                style: const TextStyle(fontSize: 18),
              ),
              Text(
                'Description: ${widget.item.fields.description}',
                style: const TextStyle(fontSize: 18),
              ),
              Text(
                'Amount: ${widget.item.fields.amount}',
                style: const TextStyle(fontSize: 18),
              ),
              ElevatedButton(
                onPressed: () {
                  // Navigasi kembali ketika tombol ditekan
                  Navigator.pop(context);
                },
                child: const Text('Kembali'),
              ),            
            ],
          ),
        ),
      );
    }
  }
  ```




