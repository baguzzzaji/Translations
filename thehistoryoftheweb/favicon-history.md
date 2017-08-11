# Asal Mula Favicon

Pada tahun 1999 dua *browser* terbesar saat itu, Netscape Navigator dan Microsoft Internet Explorer saling sikut-sikutan untuk menguasai pasar. Rilis baru Internet Explorer 5 pada bulan Maret 1999 memberikan pengguna sebuah *browser* yang terintegrasi dengan sistem operasi Windows serta memberikan serangan terakhir untuk pangsa pasar Netscape Navigator. 

Persaingan antar *browser* membuat pengembang harus bereksperimen dengan fitur darn ide baru demi mendapat keuntungan lebih dari kompetitor. Komunikasi antar dua perusahaan sangat jarang didengar dan pada saat itu proses berbagi kode masih belum populer. 

Eksperimen-eksperimen ini lah yang membawa developer bernama Bharat Shyam di Microsoft untuk membuat sebuah fitur "favorite" sederhana saat bekerja untuk Internet Explorer. Di Internet Explorer 4, pengguna dapat menambahkan sebuah website ke daftar favorit mereka, fitur yang meniru "bookmarks"-nya Netscape. Tapi kedua *browser* masih belum memiliki cara mudah untuk bernavigasi didaftar website yang panjang. 

Untuk membuat sebuah website mudah dikenali, Shyam menambahkan sebuah icon kecil disebelah nama website didaftar favorit; sebuah gambar sebesar 16 x 16px yang dapat membantu pengguna membedakan satu website dengan website lain. Dia menamai fitur ini sebagai *favicon*, sebuah kombinasi antara "favorite" dan "icon". Hari ini kata favicon sudah menjadi istilah sehari-hari, namun banyak yang tidak tahu sejarah aslinya. 

Agar sebuah website memiliki *icon*, Shyam menyederhanakan prosesnya dengan membuat developer cukup menaruh file bernama *favicon.ico* di direktori root web server mereka. Internet Explorer dengan sendirinya akan tahu dimana mencari *icon* yang diperlukan. Shyam menggunakan .ico karena format ini sudah menjadi standar format yang digunakan oleh Windows.

Akhirnya pada suatu malam, Shyam memanggil seorang *junior project manager* bernama Ray Sun untuk melihat fitur yang Ia kerjakan dan meminta ijin untuk memasukkan kode ini ke versi Internet Explorer versi selanjutnya. Karena terkesima dengan fitur tersebut, Sun tanpa pikir panjang memberi ijin. Dengan ijin itu favicon masuk ke Internet Explorer 5. 

Esoknya Sun ditegur oleh manager lain karena mengijinkan fitur baru masuk terlalu cepat. Ternyata alasan Shyam menunggu sampai malam karena tahu Program Manager yang lebih berpengalaman mungkin tidak akan memberikan ijin untuk membawa fitur ini. Namun pada hari itu, kode yang ditulis telah terlanjur di-*merge*. 

Apapun yang terjadi, favicon adalah sebuah fitur yang spektakuler. Developer banyak yang menyukai ide ini, bahkan pemain besar pada waktu itu Yahoo, langsung mencobanya. 

Ternyata selain mempermudah pengguna untuk membedakan antar website di daftar favorit, favicon juga bisa dipakai untuk menghitung ada berapa orang yang melakukan mengunjungi web dengan membaca jumlah *request* file favicon.ico. 

Tak lama kemudian *web standard* menentukan sebuah pendekatan untuk menambahkan sebuah favicon bagi suatu website. Karena menaruh langsung favicon ke root directory web server mungkin sulit untuk beberapa kasus, sebuah proposal diberikan kepada W3C untuk menggunakan elemen `link` dengan atribut baru. Atribut ini dapat memberitahu dimana gambar favicon yang akan diberikan. Proposal ini juga membuat favicon dapat menggunakan format lain selain .ico seperti berikut:

```
<link rel=”icon” href=”/path/to/icon.png”>
```

Evolusi tak berhenti sampai disini. Selanjutnya Microsoft mulai menampilkan favicon tak hanya untuk *bookmark* tapi juga di konteks lain, seperti disebelah URL *address bar* (diikuti juga oleh *browser* lain nantinya). 

Demikian lah kisah bagaimana favicon muncul di Internet Explorer 5. Berkat fitur sederhan ini, dunia web menjadi lebih berwarna seperti yang kita rasakan saat ini.

Sumber: [How We Got the Favicon](http://thehistoryoftheweb.com/how-we-got-the-favicon/)