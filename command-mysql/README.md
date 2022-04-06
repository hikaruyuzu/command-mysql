# command-mysql

### mysql or mariadb
- Commandline SQL client disini bisa digunakan untuk MySQL atau MariaDB
- Untuk melihat lebih lengkapnya kamu bsisa menuju slide [Berikut](https://docs.google.com/presentation/d/1v4HllRI-BNj4EdJFLh4_jISq_dcosHoAVo5-LME-ghY/edit#slide=id.p). , dari Programmer Zaman Now
- Untuk perintah-perintah dibawah merupakan ringkasan lengkap dari semua perintah yang dapat digunakan

### basic command
- Membuat database ```create database name_database; ```
- Menghapus database ```drop database name_database;```
- Menggunakan database ```use database name_database;```
- Melihat table dalam database ```show tables;```
- Contoh pembuat table `
 ```ssh
 CREATE TABLE barang (
    id INT NOT NULL ,
    nama VARCHAR(100) NOT NULL DEFAULT 'DEFAULT',
    harga INT NOT NULL DEFAULT 0,
    jumlah INT NOT NULL DEFAULT 0
)ENGINE = InnoDB;
 
```
- Melihat struktur table ```DESCRIBE nama_table```
 melihat process pembuatan table ```SHOW CREATE TABLE nama_table```
 
 -  Modifikasi table yang sudah terlanjur dibuat
 ```ssh
 ALTER TABLE barang
ADD COLUMN deskripsi TEXT;

ALTER TABLE barang
ADD COLUMN salah TEXT;

ALTER TABLE barang
DROP salah;

ALTER TABLE barang
MODIFY nama VARCHAR(200) AFTER deskripsi;

ALTER TABLE barang
MODIFY nama VARCHAR(200) FIRST;

ALTER TABLE barang
MODIFY nama VARCHAR(200) AFTER id;
```

- NOT NULL digunakan untuk memberitahu bahwa kolom dari table tersebut tidak boleh kosong
```ssh
ALTER TABLE barang
MODIFY id INT NOT NULL ;

ALTER TABLE barang
MODIFY nama VARCHAR(200) NOT NULL ;

```

- DEFAULT, nilai yang akan secara otomatis menjadi default value jika user tidak memasukkan datanya
```ssh
ALTER TABLE barang
MODIFY harga INT NOT NULL DEFAULT 0;

ALTER TABLE barang
MODIFY jumlah INT NOT NULL DEFAULT 0;

ALTER TABLE barang
ADD COLUMN waktu_dibuat TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP;
```
- Menghapus semua data yang ada di table ```TRUNCATE nama_table```
-  Permanen menghapus table ```DROP nama_table```

### Insert Data kedalam Table

- Contoh penggunakan INSERT, dimana disini tidak harus berurutan atau juga bisa tidak masalah jika tidak di isi
```ssh
INSERT INTO products(id, name, price, quantity)
VALUES('P0001', 'Sandal jepit', 10000, 100);

INSERT INTO products(id, name, description, price, quantity)
VALUES('P0002', 'Sandal jepit Pro', 'Improve sandal jepit max pro', 20000, 100);

INSERT INTO products(id, name, price, quantity)
VALUES ('P0003', 'Sandal Kayu', 15000, 120),
       ('P0004', 'Minyak Makan', 15000, 200),
       ('P0005', 'Minyak Goreng Udang', 25000, 100);
```

### Select Data dari Table

SELECT Digunakan untuk mengambil data dari Table 
- Mengambil semua data dari table ```SELECT * FROM nama_table```

- Mengambil sebagian data dari table, boleh acak
```ssh
SELECT id, name, price, quantity FROM products;
SELECT name, price, id FROM products;
```
DISTINCT digunakan untuk mengambil data duplicate menjadi satu, misal kita punya data Category nah diakan banyak, kita mau ambil categoryny saja 1 per 1 tanpa adanya duplikasi maka kita bisa menggunakan `DISTINCT`

- DISTINCT Contoh penggunaan
```ssh
SELECT DISTINCT category FROM products;
```
### Primary key
PRIMARY KEY ini digunakan biasanya untuk data yang tidak boleh duplicate misalnya untuk id, di dalam table mysql kita bisa membuat lebih dari satu primary key, akan tetapi ini sangat jarang dilakukan, kecuali ada kondisi tertentu seperti MANY TO MANY RELATIONSHIP 

- Membuat PRIMARY KEY
```ssh
CREATE TABLE products (
    id VARCHAR(10) NOT NULL,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price INT UNSIGNED NOT NULL,
    quantity INT UNSIGNED NOT NULL DEFAULT 0,
    craeted_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (id)
) ENGINE = InnoDB;

```
### Where Clause
Digunakan untuk mencari spesifik data misal berdasarkan id, name, quantity etc.

- Penggunaan WHERE clause
```ssh
SELECT * FROM products WHERE quantity = 100;

SELECT * FROM products WHERE name = 'sandal jepit pro';

SELECT id, name, quantity, price FROM products WHERE quantity = 200;
```
Adapun penggunaan WHERE yang lebih lengkap dengan menggunakan operator perbandingan seperti > , < , >= , <=, !=, bahkan AND dan OR
```ssh
SELECT * FROM products WHERE quantity > 100;

SELECT name, price, quantity, id FROM products WHERE price < 100000 AND quantity > 100;

SELECT * FROM products WHERE (category = 'Barang' OR price < 100000) AND quantity > 100 ;
```

LIKE, LIKE digunakan untuk mencari sebagian atau seluruh kata dalam string, akan tetapi ini tidak di recomendasikan jika record dalam table sudah banyak, dikarenakan akan lambat, LIKE operator tidak case sensitive yang artinya dia tidak peduli dengan besar atau kecil huruf yang di masukan

- Contoh penggunaan LIKE 
```ssh
SELECT * FROM products WHERE name LIKE '%sabun%';

SELECT * FROM products WHERE name LIKE '%fig%';

SELECT * FROM products WHERE name NOT LIKE '%minyak%';
```

IS NULL dan IS NOT NULL digunakan untuk mengecek dan menampilkan data yang null ataupun tidak null 

- Berikut adalah contoh penggunaanya
```ssh
SELECT * FROM products WHERE description IS NOT NULL;

SELECT * FROM products WHERE description IS NULL ;
```

BETWEEN dan NOT BETWEEN pada dasarnya sperti kombinasi dua operator AND misal kita ingin mencari harga > 1000 and harga < 10000 maka dengan between kita bisa menggunakan 

- Seperti contoh dibawah ini
``` ssh
SELECT * FROM products WHERE price BETWEEN 100000 AND 3000000;

SELECT * FROM products WHERE price NOT BETWEEN 1000000 AND 300000;
```

IN dan NOT IN digunakan untuk  menampilkan data dengan category tertentu misal jika `IN category('makanan', 'minuman')` dia akan menampilkan semua data yang memiliki kategori makanan atau minuman, jadi ini seperti OR operator

- Contoh penggunaanya adalah sebagai berikut
```ssh
SELECT * FROM products WHERE category IN('Barang');

SELECT * FROM products WHERE category NOT IN ('Barang', 'Jasa') ;
```

### Update data dalam Table

Kita bisa melakukan update data dalam table dengan kombinasi perintah UPDATE, SET dan juga WHEERE CLAUSE

- Contoh penggunaan
```ssh
UPDATE products
SET category = 'Barang'
WHERE id = 'P0002';

UPDATE products
SET category = 'Lain-lain',
    description = 'Minyak makan untuk goreng apa saja'
WHERE id = 'P0004';

UPDATE products
SET price = price + 5000
WHERE id = 'P0004';
```

### Menghapus data dari table 
Kamu bisa menggunakan DELETE yang bisa dikombinasikan dengan WHERE CLAUSE untuk menghapus data tertentu dalam table
- Contoh penggunaan DELETE
```ssh
INSERT INTO products(id, name, category, description, price, quantity)
VALUES('P0009', 'Minyak curah', 'Lain-lain', 'Wakaranai', 5000, 1000);

DELETE FROM products
WHERE id = 'P0009';
```

### Alias
Alias akan sangat berguna saat kita akan melakukan JOIN, alias sendiri bisa digunakan untuk kolom dalam table atau nama table itu sendiri benefitnya adalah kita bisa membuat nama lain yang lebih pendek atau nama lain dari nama kolom atau nama table, di dalam alias jika kamu buat lebih dari satu kata kamu bisa memakai `'coba aja'` seperti itu
- Contoh penggunaan Alias untuk kolom
```ssh
SELECT id AS Kode,
       name AS Nama,
       category AS Kategori,
       description AS Deskripsi,
       price AS Harga ,
       quantity AS Stock
FROM products;
```
- Contoh Alias untuk table
```ssh

SELECT p.id AS Kode,
       p.name  As Nama,
       p.category  AS Kategori,
       p.description AS Deskripsi,
       p.price AS Harga,
       p.quantity AS Stock
FROM products AS p;
```

### ORDER BY CLAUSE
ORDER BY CLAUSE digunakan untuk mengambil data berdasarkan urutan baik secara ASC ataupun DESC, dan dalam satu query bisa menggunakan lebih dari satu kondisi, secara default jika tidak di beri tahu akan di urutkan secara ASC atau DESC dia akan di urutkan secara ASC

- Contoh penggunaan
```ssh
SELECT id, name, category FROM products ORDER BY category ASC;

SELECT id, name, category, price FROM products ORDER BY category ASC, price DESC ;

SELECT * FROM  products WHERE description IS NULL ORDER BY price DESC ;

```

### LIMIT CLAUSE
LIMIT biasanya digunakan untuk melakukan paging di aplikasi kita, misal kita mempunyai data record yang sudah jutaan, bukan suatu pilihan yang bijak mengambil semua datanya tanpa melakukan pagging, LIMIT digunakan untuk melakukan pagging.

- Contoh penggunaan `LIMIT` 
```ssh
SELECT * FROM products ORDER BY id LIMIT 5;

SELECT * FROM products ORDER BY id LIMIT 3;
```

Jika kita ingin melakukan limit dan melanjutkan limit dari data akhir yang kita limit kamu bisa menggunakan SKIP dengan format LIMIT start, count_skip;

- Contoh penggunaan SKIP
```ssh
SELECT * FROM products ORDER BY id LIMIT 0, 5;

SELECT * FROM products ORDER BY id LIMIT 5, 5;

SELECT * FROM products ORDER BY id LIMIT 10, 5;
```

### Operator Matematika
Kamu bisa gunakan ini untuk memanipulasi data di dalam table, operator-operator yang ada umunya sama saja seperti %, + , - * etc.

- Conoh pengunaan
```ssh
SELECT 10 + 10 AS 'Hasil';

SELECT id, name, price DIV 1000 AS 'Price in K' FROM products;
```

### AUTO INCREMENT
Auto INcrement digunakan untuk menaikkan angka secara otomatis, biasanya digunakan untuk id, id harus di set ke PRIMARY KEY

- Contoh penggunaan
```ssh
CREATE TABLE admin (
    id INT NOT NULL AUTO_INCREMENT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    PRIMARY KEY (id)
)ENGINE = InnoDB;
```

- Untuk melihat last insert id dari AUTO INCREMENT gunakan

```ssh
SELECT LAST_INSERT_ID();
```

### STRING FUNCTION
Function ini digunakan untuk memanipulasi data string untuk lebih lengkapnya ;ihat dokumentasinya [Di sini](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html).

- Contoh penggunaan
```ssh
SELECT id,
       LOWER(name) AS 'Name Lower',
       UPPER(name) AS 'Name Upper',
       LENGTH(name) AS 'Name Length'
FROM products;

```

### DATE TIME FUNCTION
FUnction ini digunakan untuk memanipulasi data waktu untuk lebih lengkapnya kamu bisa lihat [Di Sini](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html).

- Contoh penggunaan
```ssh
SELECT id, name,
       EXTRACT(YEAR FROM craeted_at) AS Year,
       EXTRACT(MONTH FROM craeted_at) AS Month,
       EXTRACT(DAY FROM craeted_at) AS Day
FROM products;

SELECT id, name, YEAR(craeted_at), MONTH(craeted_at) FROM products;
```

### AGGREGATE FUNCTION
Agreegate function ini biasanya digunakan misal untuk melihat harga tertinggi suatu product atau melihat ada berapa record di tabel product, ataupun menjumlahkan semua isi dari price/ pun untuk mencari rata-ratanya kamu bisa melihat dokumetasi resminya [Di sini](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html).
```ssh
SELECT COUNT(id) AS 'Jumlah Record' FROM products;

SELECT AVG(price) AS 'Rata-rata harga' FROM products;

SELECT SUM(quantity) AS 'Stock' FROM products;

SELECT MAX(price) AS 'Maximum harga' FROM products;
```
### GROUP BY CLAUSE
GROUP BY ini berkaitan erat dengan AGREGATE func dimana jika ingin menggunakan GROUP BY harus menyertakan AGREGATE func, GROUP BY ini digunakan untuk misal kita ingin melihat jumlah quantity product/ categorynya atau jumlah product/ category kamu bisa menggunakan AGREGATE 

```ssh
SELECT COUNT(id) AS 'Jumlah Record/ Catgory' , category FROM products GROUP BY category;

SELECT MAX(price) AS 'Max Price/ Catgory', category FROM products GROUP BY category;

SELECT AVG(price) AS 'Rata-rata/ Category', category FROM products GROUP BY category;
```

### HAVING CLAUSE
HAVING CLAUSE ini digunakan untuk memfilter data misal kita ingin mencari data yang quantitynya > 5, akan tetapi ini haya bisa digunkan di AGREGATE func saja, kenapa harus menggunakan having clause ?, karena di AGREGATE func tidak bisa menggunakan WHERE CLAUSE

```ssh
SELECT COUNT(id) AS 'Jumlah Barang', category
FROM products
HAVING `Jumlah Barang`> 6;
```

### CONSTRAINT
CONSTRAINT digunakakn sebagai pembatas misal, kita tidak ingin user memasukan alamat email yang sama kedalam table maka kita bisa memanfaatkan fitur dari constraint ini

#### UNIQUE KEY
- Agar tidak terjadi duplikasi data

- Contoh penggunaan
```ssh
CREATE TABLE costumers (
    id INT NOT NULL AUTO_INCREMENT,
    email VARCHAR(100) NOT NULL ,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100),
    PRIMARY KEY(id),
    UNIQUE KEY email_unique (email)
)ENGINE = InnoDB;


ALTER TABLE costumers
DROP CONSTRAINT email_unique;

ALTER TABLE costumers
ADD CONSTRAINT UNIQUE email_unique(email);

INSERT INTO costumers(email, first_name, last_name)
VALUES ('kaguyachan@gmail.com', 'kaguya', 'shinomiya');

SELECT * FROM costumers;

INSERT INTO costumers(email, first_name, last_name)
VALUES('katomegumi@gmail.com', 'Kato', 'Megumi');
```

#### CHECK
Digunakakn untuk memastikan data input sesuai dengan ketentuan yang sudah kita tetapkan, jika tidak maka datanya akan ditolak

- Contoh penggunakan
```ssh

INSERT INTO products(id, name, category, price, quantity)
VALUES ('P00016', 'Manga Kaguya Sama', 'Lain-lain', 500, 100);

ALTER TABLE products
ADD CONSTRAINT check_price CHECK ( price >= 500 );

UPDATE products
SET price = 1000
WHERE id = 'P0016';
```

### SEARCH DENGAN INDEX
- Jika kita menggunakan index untuk melakukan pencarian, maka pencarian akan lebih cepat dilakukan daripada tidak menggunakan index, akan tetapi kita harus bijak ketika menggunakan index dikarenakan jika kita menggunakan index maka akan berpengaruh ke proses insert update dan delate data, dikarenakan data table yang diberi index akan menyusun kembali datanya dengan konsep balance tree, ini akan membuatnya menjadi lambat, jadi kita harus bijak dalam penggunaan nya, dikarenakan ya iu tadi akan lambat dalam proses CRUD akan tetapi cepat dalam sisi search data

- Jika kita sudah menetapkan kolom ssbagai PRIMARY KEY atau UNIQUE KEY maka index akan secara otomatis di ambahkan jadi kita tidak perlu menambahkan indc lagi secara manual

- Contoh penggunaan index
```ssh
CREATE TABLE sellers (
    id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL ,
    name2 VARCHAR(100) NOT NULL ,
    name3 VARCHAR(100) NOT NULL ,
    email VARCHAR(100) NOT NULL ,
    PRIMARY KEY (id),
    UNIQUE KEY email_unique(email),
    INDEX name_index (name),
    INDEX name_index2(name2),
    INDEX name_index3(name3),
    INDEX name_name2_name3(name, name2, name3)

)ENGINE = InnoDB;
```
- Di atas kita melakukan penambahan index , penambahan bisa dilakukan > 1 index akan tetapi untuk proses qury misal dengan where kita hanya bisa query dengan urut misal menggunakank name saja, name dan name 2 atau name name1 dan nam3, jika meggunakan name2 dan name 3 maka tidak akan terkena impactnya.

### FULL TEXT SEARCH

- Dapat digunakan untuk mencari kata, akan tetapi dia lebih cepat karena dia include INDEX, akan tetai jangan terlalu berharap karena fiturnya snagat terbatas tidak seperti di database khusus search seperi Elastic Search, full text search ini lebih unggul daripada like

- Contoh penggunaan

```ssh
ALTER TABLE products
ADD FULLTEXT full_search(name, description);

ALTER TABLE products
DROP INDEX full_search;
```
#### Cara pencarian di FULL TEXT SEARCH
- IN NATURAL LANGUAGE MODE , digunakan untuk mencari berdasarkan potongan kata dan akan menampilkan data yang ada dalam kata tersebut
```ssh
SELECT * FROM products WHERE
match(name, description) AGAINST ('sandal' IN NATURAL LANGUAGE MODE);
```
- IN BOOLEAN MODE, + artinya kata yang boleh ada - artinya kata yang tidak boleh ada
```ssh
SELECT * FROM products WHERE
MATCH(name, description) AGAINST ('+SANDAL -JEPIT' IN BOOLEAN MODE);
```

-  WITH QUERY EXPANSION, akan mencari berdasarkan pendekatan
```ssh
SELECT * FROM products WHERE
MATCH(name, description) AGAINST ('udang' WITH QUERY EXPANSION );
```

### TABLE RETAIONSHIP
Kenyataanya kita akan banyak menggunakan TABLE RELATIONSHIP ini saat membuat aplikasi, contoh penggunaan table relatonship ini adalah misal kita memiliki table untuk laporan penjulann dimana table laporan ini memiliki data product sellerdan juga costumer, disini berarti dia akan memiliki relationship dengan table seller dan juga table product, referensi untuk table-table ini dapat dihubungkan dengan id, dimana id di dalam table laporan penjualan akan memiliki relasi dengan table product seller dan juga costumer, dimana id di dalam tavle penjualan harus sama dari tipe datanya.

- Contoh simple dari table wishlist melakukan relasi ke table product
```ssh
CREATE TABLE wishlist (
    id INT NOT NULL AUTO_INCREMENT,
    id_product VARCHAR(10) NOT NULL ,
    desciption TEXT,
    PRIMARY KEY (id),
    CONSTRAINT fk_wishlist_product FOREIGN KEY (id_product) REFERENCES products(id)

)ENGINE = InnoDB;

DESCRIBE wishlist;

ALTER TABLE wishlist
DROP CONSTRAINT fk_wishlist_product;

SHOW CREATE TABLE wishlist;

ALTER TABLE wishlist
ADD CONSTRAINT fk_wishlist_product FOREIGN KEY (id_product) REFERENCES products(id);

```
#### Behavior FOREIGN KEY di TABLE RELATIONSHIP
- RESTRICT, ini adalah defaultnya, secara otomatis jika kita update atau delete datanya akan di tolak

- CASECATE , jka data di table product di delete maka data yang reference ke dia juga akan di delete, untuk update juga datanya akan ikut berubah

- NO ACTION, datanya di update dan delete tidak akan berubah

- SET NULL , datanya akan berubah menjadi NULL

```ssh

ALTER TABLE wishlist
ADD CONSTRAINT fk_wishlist_product FOREIGN KEY (id_product) REFERENCES products(id)
ON DELETE CASCADE ON UPDATE CASCADE ;

SELECT * FROM wishlist;

INSERT INTO wishlist(id_product, desciption)
VALUES ('PXXXX', 'Barang kesukaanku');


INSERT INTO wishlist(id_product, desciption)
VALUES ('P0002', 'Barang keperluan rumah tangga');

DELETE FROM products
WHERE id = 'PXXXX';

DELETE FROM wishlist
WHERE id = 8;

```
