# Memahami App Resources di Android

Di Android, hampir semuanya adalah sebuah **resource**. [Mendefinisikan resource](http://developer.android.com/guide/topics/resources/providing-resources.html) yang nantinya dapat [diakses](http://developer.android.com/guide/topics/resources/accessing-resources.html) di aplikasi merupakan bagian mendasar dari proses pengembangan aplikasi Android.

*Resource* dapat dipakai untuk mendefinisikan *color* (warna), *image* (gambar), *layout* (tata letak), *menu*, dan nilai *string*. Nilai-nilai di dalam *resource* ini mencegah kita dari menuliskannya secara langsung (*hardcoded*). Semua yang didefinisikan dapat dipanggil dari manapun didalam *project* yang dibuat. Contoh yang paling umum dan paling sederhana adalah penggunaan **string resource** untuk lokalisasi teks yang fleksibel.

## Jenis-jenis Resource

Berikut ini jenis *resource* yang paling umum dipakai di aplikasi Android:

| **Nama**             | **Folder** | **Deskripsi**                            |
| -------------------- | ---------- | ---------------------------------------- |
| *Property Animation* | animator   | berkas XML yang mendefinisikan properti animasi |
| *Tween Animation*    | anim       | berkas XML yang mendefinisikan *tween* animasi |
| *Drawable*           | drawable   | berkas gambar berbentuk Bitmap atau XML  |
| Layout               | layout     | berkas XML yang mendefinisikan antarmuka aplikasi |
| Menu                 | menu       | berkas XML yang mendefinisikan menu atau item action bar |
| Values               | values     | berkas XML yang menyimpan nilai seperti string, integer, dan color. |

Di bawah ini merupakan berkas-berkas yang tersimpan di folder `values`:

| Nama       | File                     | Deskripsi                                |
| ---------- | ------------------------ | ---------------------------------------- |
| Color      | `res/values/colors.xml`  | Mendefinisikan [warna](http://developer.android.com/guide/topics/resources/more-resources.html#Color) |
| Dimensions | `res/values/dimens.xml`  | Mendefinisikan [ukuran](http://developer.android.com/guide/topics/resources/more-resources.html#Dimension) seperti padding, margin, font, dll. |
| Strings    | `res/values/strings.xml` | Mendefinisikan [teks](http://developer.android.com/guide/topics/resources/string-resource.html) misalnya untuk *placeholder* atau nama aplikasi |
| Styles     | `res/values/styles.xml`  | Mendefinisikan *[style](http://developer.android.com/guide/topics/resources/style-resource.html)* misalnya warna untuk AppBar |

Untuk daftar jenis *resource* yang lebih lengkap, silahkan kunjungi halaman [berikut](http://developer.android.com/guide/topics/resources/providing-resources.html).

## Membuat App Resource

###Membuat Sebuah String Resource

Untuk setiap teks yang ingin ditampilkan di aplikasi (misalnya label sebuah Button dan teks didalam TextView), kita pertama harus mendefinisikan dulu teks tersebut di dalam berkas `/res/avlues/strings.xml`. Setiap nilai yang ditambahkan di dalam berkas tersebut terdiri atas key (sebagai id untuk teks) dan nilai dari teks itu sendiri. Contoh, apabila kita ingin membuat sebuah Button dengan teks "Submit", berikut ini nilai yang mesti ditambahkan ke `strings.xml`:

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="hello">Hello!</string>
    <string name="submit_label">Submit</string>
</resources>
```

Sekarang kapanpun kita memanggil id `submit_label`, kita akan mendapatkan teks "Submit". Kita dapat menampilkan data yang lebih kompleks (misalnya teks yang mengandung kode html atau karakter khusus) menggunakan CDATA sebagai berikut:

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="feedback_label">
  <![CDATA[
    Please <a href="http://highlight.com">let us know</a> if you have feedback on this or if 
    you would like to log in with another identity service. Thanks! This is a longer string!  
  ]]>
  </string>
</resources>
```

### Mereferensikan App Resource

Sekarang setelah kita mendefinisikan string resource yang dibutuhkan, kita dapat memanggilnya dari Java atau XML lain. Untuk mengaksesnya dari file XML lain, cukup panggil menggunakan simbol `@`:

```
<Button
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="@string/submit_label" />
```

Untuk mengakses resource ini dari Java, maka kita perlu memanggil metode `getResource.getString` atau `getString` dengan memanggil id-nya:

```
String submitText = getResources().getString(R.string.submit_label)
```

Pola yang sama dapat kita pakai untuk memanggil semua jenis *resource*.

### Mendefinisikan Color Resource

Selain string *resource* seperti contoh di atas, kita juga dapat membuat *color resource* sebagai berikut:

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
   <color name="white">#FFFFFF</color>
   <color name="yellow">#FFFF00</color>
   <color name="fuchsia">#FF00FF</color>
</resources>
```

Mengaksesnya di kode Java dengan cara sebagai berikut:

```
// cara pemanggilan getResources().getColor() sudah ditinggalkan
// Resources res = getResources();
// int color = res.getColor(R.color.yellow); 

// Gunakan ContextCompatResources untuk menggantikan getColor()
int color = ContextCompat.getColor(context, R.color.yellow);
```

Dan memanggilkan *color resource* dari berkas XML lain dengan cara:

```
<TextView
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:textColor="@color/fuchsia"
    android:text="Hello"/>
```

### Mendefinisikan Dimension Resource

Saat menentukan ukuran kita menggunakan beberapa unit misalnya 16dp untuk ukuran margin dan 12sp untuk ukuran font. Dimensi ini mesti kita definisikan di berkas `dimens.xml` dengan cara:

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <dimen name="textview_height">25dp</dimen>
    <dimen name="textview_width">150dp</dimen>
    <dimen name="ball_radius">30dp</dimen>
    <dimen name="font_size">16sp</dimen>
</resources>
```

Pemanggilannya di Java:

```
Resources res = getResources();
float fontSize = res.getDimension(R.dimen.font_size);
```

Sedangkan di XML:

```
<TextView
    android:layout_height="@dimen/textview_height"
    android:layout_width="@dimen/textview_width"
    android:textSize="@dimen/font_size"/>
```

###Pemanggilan Resource Secara Dinamis

Dalam beberapa kasus kita ingin mengambil *resource* secara dinamis tanpa menuliskan manual `id`-nya. Contoh, kita ingin mengambil teks dari "submit_label" berdasarkan namanya. Nilai yang diinginkan bisa kita dapatkan menggunakan metode `getIdentifier` di dalam sebuah Activity:

```
public String getStringValue(String key) {
    // Mengambil resource id
    String packageName = getBaseContext().getPackageName();
    Resources resources = getBaseContext().getResources();
    int stringId = resources.getIdentifier(key, "string", packageName);
    if (stringId == 0) { return null; }
    // Mengembalikan nilai string berdasarkan resource id
    return resources.getString(stringId);
}
```

Sekarang kita bisa mereferensikan string resource secara dinamis dengan cara:

```
public String myKey = "submit_label"; // Maksudnya R.string.submit_label
public String myStringValue = getStringValue(myKey); // Mengambil teks berdasarkan id
```

Teknik di atas dapat diaplikasikan ke jenis *resource* yang lain. Berikut ini contoh untuk mengambil view secara dinamis berdasarkan `id`-nya:

```
// getViewById("tvTest");
public View getViewById(String id) {
    // Retrieve the resource id
    String packageName = getBaseContext().getPackageName();
    Resources resources = getBaseContext().getResources();
    int viewId = resources.getIdentifier(id, "id", packageName);
    if (viewId == 0) { return null; }
    return findViewById(viewId);
}
```

## Membuat Resource Alternatif

### Responsive Design

Untuk membuat antarmuka yang menarik, penting bagi developer membuat aplikasi yang ia kembangkan dapat berjalan dengan baik di berbagai peragnkat. Untuk mencapai tujuan ini, kita pertama harus membagi jenis-jenis perangkat Android kedalam kategori berdasarkan besar layar sebagai berikut:

![](http://i.imgur.com/bLOO9e3.png)

Aplikasi harus didesain untuk berkerja di berbagai jenis lebar layar dan kerapatannya. 