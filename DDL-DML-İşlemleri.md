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
CREATE TABLE ProductFeatures(
  Id int IDENTITY(1,1) PRIMARY KEY,
  Name nvarchar(100)
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
ALTER ADD anahtar sözcüğü ile Products-ProductFeatures ve Products-Categories tabloları arasındaki 1-1 ve 1-n ilişkileri Foreign Key yardımıyla kuralım.
```
ALTER TABLE Products 
ADD CategoryId int FOREIGN KEY REFERENCES Categories(Id)

ALTER TABLE Products 
ADD FeatureId int FOREIGN KEY REFERENCES ProductFeatures(Id)
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
INSERT INTO ProductFeatures(Name) VALUES('No-Frost')
INSERT INTO ProductFeatures(Name) VALUES('4 Programlı')
INSERT INTO ProductFeatures(Name) VALUES('5 Raflı')
INSERT INTO ProductFeatures(Name) VALUES('Seramik')
INSERT INTO ProductFeatures(Name) VALUES('A Enerji Sınıfı')
INSERT INTO ProductFeatures(Name) VALUES('Suya Dayanıklı')
INSERT INTO ProductFeatures(Name) VALUES('Özel Toz Hazneli')
```
```
INSERT INTO Products(CategoryId,FeatureId,Name,Price,Stock) 
VALUES(1,5,'Çamasir Makinesi',6589,7)

INSERT INTO Products(CategoryId,FeatureId,Name,Price,Stock) 
VALUES(2,4,'Halka Vazo',69.90,25)

INSERT INTO Products(CategoryId,FeatureId,Name,Price,Stock) 
VALUES(1,1,'Buzdolabi',7800,3)

INSERT INTO Products(CategoryId,FeatureId,Name,Price,Stock) 
VALUES(3,6,'Akilli Saat',1.040,8)

INSERT INTO Products(CategoryId,FeatureId,Name,Price,Stock) 
VALUES(2,3,'Kitaplik',650,20)

INSERT INTO Products(CategoryId,FeatureId,Name,Price,Stock) 
VALUES(3,7,'Robot Süpürge',2.400,38)

INSERT INTO Products(CategoryId,FeatureId,Name,Price,Stock) 
VALUES(1,2,'Bulasık Makinesi',6.889,13)
```
SELECT anahtar sözcüğü ile oluşan tabloların son görüntüleri şu şekilde olacaktır.
```
SELECT * FROM Categories
```
![dbo.categories](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/categories.png)

```
SELECT * FROM ProductFeatures
```
![dbo.productFeatures](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/productFeatures.png)

```
SELECT * FROM Products
```
![dbo.products](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/products.png)

Son olarak Products tablosundaki üç adet stoğu bulunan buzdolabı ürününün stoğunun bittiğini varsayarsak bu senaryoda DELETE keyword'ü ile bu ürünü silmemiz gerekir.
Benzer bir şekilde adı 'Akilli Saat' olan üründen üç adet satıldığını düşünürsek stok bilgisinin 8'den 5'e düşmesi gerekmektedir. 
DML ifadelerinden UPDATE anahtar sözcüğü ile de 2.senaryoyu gerçekleştirebiliriz.

```
DELETE FROM Products WHERE Id=9
```
![deletedProduct](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/deletedProduct.png)

```
UPDATE Products SET Stock=5 WHERE Id=4
```
![updatedProduct](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/updatedProduct.png)

Ve Products tablosunun son hali görseldeki gibi olacaktır.

![updatedProductsTable](https://github.com/197-Link-Bilgisayar-Net-Bootcamp/week-3-assignment-senanuryesilyurt/blob/master/photos/updatedProductsTable.png)

