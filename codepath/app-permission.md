# Memahami App Permission di Android

Android secara *default* tidak memiliki *permission*. Saat aplikasi perlu menggunakan fitur-fitur khusus yang ada di perangkat (misalnya mengakses kamera, mengirim SMS, membaca kontak, dll.) mereka perlu diberikan *permission* dari user agar tujuannya tercapai. 

Sebelum Marshmallow, *permission* diurus pada saat aplikasi pertama dipasang dankita tentukan di dalam `AndroidManifest.xml`. Semua jenis permission dapat pembaca lihat [di sini](http://developer.android.com/reference/android/Manifest.permission.html). Setelah Marshmallow, *permission* sekarang harus diminta **saat** **aplikasi dijalankan**. Ada [banyak pustaka](https://gist.github.com/dlew/2a21b06ee8715e0f7338) yang mempermudah proses permintaan *permission* agar lebih mudah. 

## Permission Pra Marshmallow

Permission jauh lebih sederhana sebelum kemuncul Marshmallow (API 23). Sebelumnya, semua *permission* dikelola saat pertama kali aplikasi di pasang. Saat pengguna ingin memasang sebuah aplikasi dari Google Play Store, mereka akan diberitahu daftar *permission* yang dibutuhkan oleh aplikasi yang bersangkutan. Pengguna dapat menyetujui semua *permission* dan melanjutkan proses pemasangan atau batal memasangnya sama sekali. Tidak ada cara untuk hanya memberikan *permission* tertentu bagi aplikasi dan tidak ada cara juga untuk menarik *permission* yang telah diberikan. 

Berikut contoh permintaan *permission* pra Marshmallow yang diminta oleh Dropbox:

![](http://i.imgur.com/XeRdzOT.png)\

Bagi developer, membuat *permission* sangat mudah dimasa ini. Untuk meminta salah satu *permission* kita cukup menuliskannya ke `AndroidManifest.xml`. Berikut ini contoh aplikasi yang meminta *permission* untuk membaca kontak yang ditentukan didalam `AdnroidManifest.xml`:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.android.app.myapp" >
    
    <uses-permission android:name="android.permission.READ_CONTACTS" />
    ...
</manifest>
```

Meski cara ini sangat mudah untuk dipakai bagi developer namun cukup merepotkan bagi pengguna karena tidak bisa memberikan  *permission* tertentu atau menarik yang sudah diberikan. 

## Permission di Masa Marshmallow

Marshmallow membawa banyak perubahan cara implementasi *permission*. API ini mengenalkan konsep *runtime permission*. Teknik yang baru ini meminta *permission* bukan pada saat pemasangan namun saat aplikasi berjalan. Tiap *permission* ini lalu dapat diijinkan atau tidak sama sekali oleh user. Untuk *permission* yang sudah diberikan dapat dicabut apabila perlu dikemudian hari. 

Kemajuan ini juga berarti ada beberapa langkah yang mesti dilakukan saat bekerja dengan permission untuk aplikasi **Marshmallow** atau yang lebih baru. Perlu dicatat bahwa `targetSdkVersion` harus `>= 23` dan perangkat/emulator menjalankan Android Marshmallow sebelum jendela permintaan *permission*-nya muncul. 

### Normal Permission

Di Marshmallow Google membuat beberapa *permission* seperti ACCES_NETWORK_STATE, INTERNET, dll. sebagai "Normal Permission". Artinya aplikasi Android yang kita buat secara otomatis sudah memiliki *permission*-*permission* ini dan tidak akan menampilkan jendela permintaan karena dianggap tidak membahayakan aplikasi. 

Perlu untuk diingat, meskipun diberikan secara otomatis namun kita masih harus tetap menambahkannya di `AndroidManifest.xml`.

### Runtime Permission

Jika *permission* yang dibutuhkan bukan merupakan salah satu *normal permission*, maka kita perlu melakukan "Runtime Permission". *Runtime permission* adalah permintaan akan *permission* yang dilakukan saat aplikasi sedang berjalan dan mereka membutuhkan *permission* tersebut. Aplikasi nantinya akan menampilkan sebuah jendela dengan tampilan seperti pada gambar di bawah:

![](http://i.imgur.com/er8yE9J.png)

Langkah pertama yang perlu dilakukan adalah menambahkannya ke `AndroidManifest.xml`:

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.codepath.androidpermissionsdemo" >

    <uses-permission android:name="android.permission.READ_CONTACTS" />
    ...
</manifest>
```

Selanjutnya kita perlu melakukan *permission request* dan menangani hasil *request* tadi. Kode berikut ini akan menunjukkan cara melakukan *request* dan menangani hasilnya dari dalam sebuah `Activity` (dapat pula dilakukan di dalam sebuah `Fragment`).

```
// MainActivity.java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        // Dalam aplikasi aslinya, kita perlu meminta permission saat pengguna melakukan 
        // aksi yang memerlukan permission tersebut.
        getPermissionToReadUserContacts();
    }

    // Pengenal bagi permission request
    private static final int READ_CONTACTS_PERMISSIONS_REQUEST = 1;

    // Fungsi di bawah ini akan dipanggil saat pengguna melakukan aksi yang akan membaca 
    // kontak sehingga nantinya ia membutuhkan sebuah permission.
    public void getPermissionToReadUserContacts() {
        // 1) Pastikan ContextCompat.checkSelfPermission(...) menggunakan versi support ]
        // library karena Context.checkSelfPermission(...) hanya tersedia di Marshmallow.
        // 2) Selalu lakukan pemeriksaan meskipun permission sudah pernah diberikan
        // karena pengguna dapat mencabut permission kapan saja lewat Settings
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS)
                != PackageManager.PERMISSION_GRANTED) {

            // Bila masuk ke blok ini artinya permission belum diberikan oleh user.
            // Periksa apakah pengguna pernah diminta permission ini dan menolaknya.
            // Jika pernah, kita akan memberikan penjelasan mengapa permission ini dibutuhkan.
            if (shouldShowRequestPermissionRationale(
                    Manifest.permission.READ_CONTACTS)) {
                // Tampilkan penjelasan mengapa aplikasi ini perlu membaca kontak disini 
                // sebelum akhirnya me-request permission dan menampilkan hasilnya
            }

            // Lakukan request untuk meminta permission (menampilkan jendelanya)
            requestPermissions(new String[]{Manifest.permission.READ_CONTACTS},
                    READ_CONTACTS_PERMISSIONS_REQUEST);
        }
    }

    // Callback yang membawa hasil dari pemanggilan requestPermissions(...)
    @Override
    public void onRequestPermissionsResult(int requestCode,
                                           @NonNull String permissions[],
                                           @NonNull int[] grantResults) {
        // Pengecekan ini akan memastikan bahwa hasil yang diberikan berasal dari request 
        // yang kita lakukan berdasarkan kode yang ditulis di atas
        if (requestCode == READ_CONTACTS_PERMISSIONS_REQUEST) {
            if (grantResults.length == 1 &&
                    grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                Toast.makeText(this, "Read Contacts permission granted", Toast.LENGTH_SHORT).show();
            } else {
                // showRationale = false jika pengguna memilih Never Ask Again, jika tidak bernilai true
                boolean showRationale = shouldShowRequestPermissionRationale( this, Manifest.permission.READ_CONTACTS);

                if (showRationale) {
                   // lakukan sesuatu disini
                } else {
                   Toast.makeText(this, "Read Contacts permission denied", Toast.LENGTH_SHORT).show();
                }
            }
        } else {
            super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        }
    }
}
```

## Permission Groups

[Permission Groups](http://developer.android.com/preview/features/runtime-permissions.html#perm-groups) menghindari user dari mendapatkan *spam* banyak *permission*. *Permission* yang saling berkaitan dikelompokkan dalam [satu grup](http://developer.android.com/reference/android/Manifest.permission_group.html). Saat sebuah aplikasi meminta *permission* yang merupakan anggota sebuah grup (misal [READ_CONTACTS](http://developer.android.com/reference/android/Manifest.permission.html#READ_CONTACTS)), Android akan meminta pengguna untuk mengijinkan grup dari permission tersebut (yaitu [CONTACTS](http://developer.android.com/reference/android/Manifest.permission_group.html#CONTACTS)). Dengan demikian saat pengguna akan membutuhkan *permission* WRITE_CONTACTS, Android akan memberikan *permission* secara otomatis tanpa perlu meminta lagi. 

## Backward Compatibility

Ada dua skenario utama yang perlu diketahui untuk *backward compatibility*:

1. Jika aplikasi menargetkan API kurang dari Marshmallow (`targetSDKVersion` < `23`), namun emulator/perangkatnya adalah Marshmallow:
   - Aplikasi akan tetap menggunakan cara lama
   - Semua permission di `AndroidManifest` akan diminta saat pemasangan
   - Pengguna dapat mencabut *permission* saat aplikasi telah selesai dipasang. Skenario in perlu diuji sebab hasil dari beberapa aksi mungkin tidak dapat ditebak. 
2. Jika emulator/perangkat menjalankan perangkat kurang dari Marshmallow, tetapi aplikasi menargetkan Marshmallow (`targetSDKVersion` >= `23`):
   - Aplikasi akan tetap menggunakan cara lama
   - Semua permission di `AndroidManifest` akan diminta saat pemasangan.

## Teknik Meminta Permission

Google merekomendasikan empat pola permintaan aplikasi seperti yang dijelaskan oleh [video](https://youtu.be/5xVh-7ywKpE?t=9m15s) berikut:

![](http://imgur.com/nZ2WX8W.png)



Setiap pola akan melakukan cara yang berbeda alam meminta permission. Sebagai contoh, saat meminta sebuah permission yang *critical* namun belum jelas untuk apa, berikan layar selamat datang untuk memberikan pemahaman bagi pengguna mengapa permission tersebut diminta. Untuk permission yang *critical* dan langsung melakukan aksinya seperti aplikasi camera yang membutuhkan permission camera, minta saat cameranya dimulai. 

## Mengelola Permission dengan ADB

*Permission* juga dapat dikelola dari *command-line* menggunakan `adb` dengan perintah-perintah berkut:

Menampilkan semua permission Android:

```
$ adb shell pm list permission -d -g
```

Memberi dan menarik *runtime permission*:

```
$adb shell pm grant com.PackageName.enterprise some.permission.NAME
$adb shell pm revoke com.PackageName.enterprise android.permission.READ_CONTACTS
```

Memasang aplikasi dengan memberikan semua *permission*:

```
$adb install -g myAPP.apk
```

