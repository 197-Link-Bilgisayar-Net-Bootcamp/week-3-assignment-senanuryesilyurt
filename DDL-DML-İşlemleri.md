# DDL (Data Definition Language)
- Veri tanımlama dili; veritabanı nesneleri oluşturma, silme ve nesneler üzerinde değişiklik yapma işlemleri için kullanılır.
- Bu ifadeler; CREATE, TRUNCATE, ALTER, DROP, COMMENT, RENAME şeklindedir.

İlk olarak Microsoft SQL Server'da yeni bir veritabanı oluşturalım.
```
CREATE DATABASE Week3Assignment
```
Ardından üzerinde çalışacağımız veritabanının yeni oluşturduğumuz database olduğunu belirtelim.
```
use Week3Assignment
```
En sık karşılaşılan CREATE anahtar sözcüğü ile sırasıyla Category, Product Features ve Product tablolarını yaratalım.
```
CREATE TABLE Categories(
  Id int IDENTITY(1,1) PRIMARY KEY,
  Name nvarchar(75)
);
```
```
CREATE TABLE Products(
  Id int IDENTITY(1,1) PRIMARY KEY,
  Name nvarchar(75),
  Price decimal,
  Stock int
);
```
Product ile Product Features tabloları arasında 1-1 bir ilişki olduğundan dolayı Product Feature'ın Id değeri veritabanına eklenen product id'lerinden biri olmalıdır.
Bu nedenle bu tabloyu oluşturuken Primary Key olan Id değeri aynı zamanda Foreign Key olarak tanımlanmalıdır.
```
CREATE TABLE ProductFeatures(
  Id int PRIMARY KEY,
  Name nvarchar(150)
  CONSTRAINT FK_ProductFeatures_Id FOREIGN KEY (Id) REFERENCES Products(Id)
)
```
ALTER ADD anahtar sözcüğü ile Products-Categories tabloları arasındaki 1-n ilişkisini Foreign Key yardımıyla kuralım.
```
ALTER TABLE Products 
ADD CategoryId int FOREIGN KEY REFERENCES Categories(Id)
```
Bu işlemleri gerçekleştirdiğimizde oluşan database diyagramı şu şekilde olacaktır.

![Diyagram](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/Diyagram.png)


# DML (Data Manipulation Language) 
- Veri işleme dili; database üzerine veri ekleme, veri güncelleme ve veri silme gibi işlemler için kullanılır.
- Bu ifadeler; INSERT, UPDATE, MERGE, DELETE,CALL, EXPLAIN PLAN ve LOCK TABLE şeklindedir.

Oluşturduğumuz Categories, Products ve ProductFeatures tablolarına INSERT INTO anahtar sözcüğü ile veri ekleyelim.
```
INSERT INTO Categories(Name) VALUES('Beyaz Eşya')
INSERT INTO Categories(Name) VALUES('Dekorasyon Ürünleri')
INSERT INTO Categories(Name) VALUES('Teknolojik Ürünler')
```
```
INSERT INTO Products(CategoryId,Name,Price,Stock) 
VALUES(1,'Çamasir Makinesi',6589,7)

INSERT INTO Products(CategoryId,Name,Price,Stock) 
VALUES(2,'Halka Vazo',69.90,25)

INSERT INTO Products(CategoryId,Name,Price,Stock) 
VALUES(1,'Buzdolabi',7800,3)

INSERT INTO Products(CategoryId,Name,Price,Stock) 
VALUES(3,'Akilli Saat',1.040,4)

INSERT INTO Products(CategoryId,Name,Price,Stock) 
VALUES(2,'Kitaplik',650,20)

INSERT INTO Products(CategoryId,Name,Price,Stock) 
VALUES(3,'Robot Süpürge',2.400,38)

INSERT INTO Products(CategoryId,Name,Price,Stock) 
VALUES(1,'Bulasık Makinesi',6.889,13)

INSERT INTO Products(CategoryId,Name,Price,Stock) 
VALUES(2,'Cerceve',45,100)
```
```
INSERT INTO ProductFeatures(Id,Name) VALUES(11,'No-Frost')
INSERT INTO ProductFeatures(Id,Name) VALUES(7,'4 Programli')
INSERT INTO ProductFeatures(Id,Name) VALUES(5,'5 Rafli')
INSERT INTO ProductFeatures(Id,Name) VALUES(2,'Seramik')
INSERT INTO ProductFeatures(Id,Name) VALUES(1,'A Enerji Sinifi')
INSERT INTO ProductFeatures(Id,Name) VALUES(4,'Suya Dayanikli')
INSERT INTO ProductFeatures(Id,Name) VALUES(6,'Özel Toz Hazneli')
INSERT INTO ProductFeatures(Id,Name) VALUES(12,'Ahsap')
```
SELECT anahtar sözcüğü ile oluşan tabloların son görüntüleri şu şekilde olacaktır.
```
SELECT * FROM Categories
```
![dbo.categories](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/categories.png)

```
SELECT * FROM Products
```
![dbo.products](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/products.png)

```
SELECT * FROM ProductFeatures
```
![dbo.productFeatures](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/productFeatures.png)


Products tablosundaki beş adet stoğu bulunan akıllı saat ürününün stoğunun bittiğini varsayarsak bu senaryoda DELETE keyword'ü ile bu ürünü silmemiz gerekir. Fakat bu bu ürüne ait olan özelliği ProductFeatures tablosundan silmeden bu ürünü silmeye çalışırsak hata alırız. Bu nedenle ilk olarak ürüne ait özelliği ardından ürünü silerek bu işlemi gerçekleştirmeliyiz.
```
DELETE FROM ProductsFeatures WHERE Id=4
DELETE FROM Products WHERE Id=4
```
![deletedProduct](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/deletedProduct.png)

İkinci bir senaryo olarak adı 'Kitaplik' olan üründen sekiz adet satıldığını düşünürsek stok bilgisinin 20'den 12'ye düşmesi gerekmektedir. DML ifadelerinden UPDATE anahtar sözcüğü ile de bu senaryoyu gerçekleyebiliriz. 
```
UPDATE Products SET Stock=12 WHERE Id=5
```
![updatedProduct](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/updatedProduct.png)

