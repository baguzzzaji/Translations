*Pseudo class selector* adalah *selector* CSS yang diikuti oleh titik dua. Pembaca mungkin sudah sangat familiar dengan beebrapa perintahnya seperti hover:

```css
a:hover {
  /* Yep, hover adalah sebuah pseudo class */
}
```

 Opsi ini sangat berguna di beberapa situasi. Beberapa merupakan CSS3, ada juga yang CSS2. Mayoritas browser selain IE (terutama sebelum IE9) sudah sangat mendukung *pseudo class*. Mari kita bahas satu persatu. Tenang, tidak banyak kok.

## Pseudo Class Selector Untuk Link (Tautan)

:link - Mungkin *pseudo selector* paling membingungkan untuk *link*. Bukankah semua `<a>` adalah *link*? Tidak juga jika mereka tidak memiliki atribut `href`. Selector ini hanya akan memilih *link* yang ada `href`-nya sehingga memiliki cara kerja yang mirip dengan `a[href]`. 

`:visited` - Memilih *link* yang sudah pernah dikunjungi di *browser* yang sedang dipakai. 

`:hover` - Saat kursor berada di atas sebuah *link*, *link* tersebut berada dalam status *hover* dan selector ini yang memilihnya.

`:active` - Memilih *link* yang sedang aktif (sedang diklik). Misalnya, untuk menentukan sebuah *link* berada dalam status "pressed" (sedang ditekan) atau membuat semua link menjadi seperti button. 

## Pseudo Class Selector untuk Input 

`:focus` - Menentukan *style* menggunakan `hover` hanya akan berguna saat menggunakan mouse. Bila pengguna menggunakan keyboard untuk navigasi maka kita perlu memakai `:focus` untuk memilih *link* yang sedang terpilih. Opsi ini tidak hanya untuk *link* tapi dapat pula digunakan untuk `input` dan `textarea`. 

![](https://css-tricks.com/wp-content/csstricks-uploads/formwithfocus.png)

`:target` - Pseudo class `target` dipakai bersama dengan ID dan bekerja saat *hash tag* di URL saat ini berteeu dengan nama ID. Jadi jika kita mengunjungi alamat www.yoursite.com/#home maka selector #home:target akan bertemu. Opsi ini sangat berguna bila kita membuat sebuah area menggunakan tab dimana setiap tab akan terhubung dengan *hash tag* dan mengaktifkan area panel berdasarkan selector yang sesuai dengan `:target`.

`:enabled` - Memilih input yang berada dalam status *enabled* dan siap dipakai. 

`:disabled` - Memilih input yang berada dalam status *disabled*. Banyak *browser* menampilkan input dengan status *disabled* dengan warna keabu-abuan. 

![](https://css-tricks.com/wp-content/csstricks-uploads/disabledelements.png)

`:checked` - Memilih *checkbox* yang sedang terpilih. 

`:indeterminate` - Memilih *radio button* yang ada dalam status tidak ada yang terpilih (kondisi bawaan saat suatu halaman baru termuat). 

![](https://css-tricks.com/wp-content/csstricks-uploads/radiopurgatory.png)

## Pseudo Class Selector Untuk Menentukan Posisi

`root` - Memilih elemen yang menjadi *root* sebuah dokumen. Hampir pasti akan memilih elemen `<html>`, kecuali kita bekerja dalam lingkungan yang juga bisa menggunakan CSS, mungkin XML. 

`:first-child` - Memilih elemen pertama dalam sebuah *parent*.

`:last-child` - Memilih elemen terakhir dalam sebuah *parent*.

`:nth-child()` - Memilih elemen berdasarkan ekspresi aljabar sederhana (contoh "2n", atau "4n-h1")

`:nth-of-type()` - Mirip seperti :nth-child tetapi dipakai saat elemen yang ada di dalam level yang sama menggunakan tipe yang berbeda. Misalnya didalam sebuah div kita ada beberapa paragraf dan gambar. Kita ingin memilih semua gambar yang ada di posisi ganjil. Opsi :nth-child tidak akan bekerja di sini, kita akan memerlukan `div img:nth-of-type(odd)`. Berguna sekali saat dipakai dalam sebuah *list* yang memiliki elemen `<dt>` dan `<dd>`.

`:first-of-type` - Memilih elemen pertama di dalam *parent* mana pun. Jadi jika kita memiliki dua div yang didalamnya ada sebuah paragraf, gambar, paragraf, dan gambar. Maka `div img:first-of-type` akan memilih gambar (tag <img>) pertama di dalam kedua div tersebut. 

`:last-of-type` - Kebalikan dari opsi di atas.

`:nth-last-of-type()` - Bekerja seperti :nth-of-type, tapi dimulai dari bawah.

`:nth-last-child()` - Bekerja seperti :nth-child, tapi dimulai dari bawah.

`:only-of-type` - Memilih suatu elemen apabila di dalam *parent*-nya tidak ada elemen lain yang sepertinya. 

![](https://css-tricks.com/wp-content/csstricks-uploads/relationalpseudos2.png)

## Pseudo Class Selector Relasi

`:not()` - Mengeluarkan elemen yang terpilih berdasarkan selector yang ada di dalam parameter :not(). Contoh, kita ingin memilih semua div tanpa div yang berkelas "music" = `div:not(.music)`. Spesifikasi CSS menyebutkan bahwa :not tidak dapat disarangkan (*nested*), tetapi bisa di sambung (*chained*). Beberapa *browser* (Firefox) dan mendukung parameter yang dipisahkan dengan koma.

`:empty` - Memilih elemen yang tidak mengandung teks apapun dan tidak ada elemen anak. Contoh: `<p></p>`

## Pseudo Class Selector Untuk Teks

`::first-letter` - Memilih huruf pertama dalam elemen. Biasa dipakai untuk *dropcap* (Huruf pertama yang membesar). 

`::first-line` - Memilih baris pertama dari sebuah teks di dalam elemen. 

`:lang` - Akan memilih elemen yang merupakan anak dari sebuah elemen dengan atribut `lang` tertentu. Misal, `:lang(fr)` akan memilih semua paragraf yang ada di dalam suatu body apabila body tersebut memilih atribut `lang="fr"`.

### Catatan

Kita bisa menghubungkan *pseudo selector* seperti kita menghubungkan kelas dan id. Misalnya tidak tidak mau memilih huruf pertama disemua paragraf tapi hanya yang ada di baris pertama, maka gunakan `p:first-child:first-letter{}`

![](https://css-tricks.com/wp-content/csstricks-uploads/dropcap.png)

## Pseudo Class Selector Untuk Konten

`::before` - Dipakai untuk menambah konten tertentu sebelum suatu elemen. Misal, menambahkan tanda petik sebelum sebuah `blockquote`.

`::after` - Dipakai untuk menambah konten tertentu setelah suatu elemen. Misal, menambahkan tanda petik setelah sebuah `blockquote`. 

### Pseudo Elemen vs Pseudo Selector

Kita sudah beberapa kali menemukan tanda `::`. Tanda itu bukanlah *typo* seperti yang pembaca pikirkan. Selector yang menggunakan `::` disebut *pseudo element* bukan *pseudo selector* karena mereka tidak benar-benar memilih sebuah elemen utuh. 

## Kualifikasi Tag

Pada contoh kode di bawah ini, kita hanya akan memilih suatu elemen apabila sebuah anak elemen pertama dari suatu elemen adalah `<p>`. Jika `<p>` tadi bukan anak elemen pertama (sebelum `<p>` adalah `<img>` misalnya) maka kode di bawah tidak dapat memilih `<p>` tersebut.

```css
p:first-child {
  color: red;
}
```



Sumber: [Pseudo Class Selectors](https://css-tricks.com/pseudo-class-selectors/)