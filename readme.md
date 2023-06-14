# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

yazar tablosu: yazarno, yazarad, yazarsoyad
tur tablosu: turno, turadi
kitap tablosu: kitapno, isbnno, kitapadi, yazarno, turno,sayfasayisi, puan
ogrenci tablosu: ogrno, ograd, ogrsoyad, cinsiyet, dtarih, sinif, puan
islem tablosu: islemno, ogrno, kitapno, atarih, vtarih

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın.

1. Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
   SELECT ograd, ogrsoyad, atarih FROM ogrenci o, islem i
   WHERE o.ogrno = i.ogrno;

2. Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
   SELECT kitapadi, turadi FROM kitap k, tur t
   WHERE k.turno = t.turno AND turadi = 'Fıkra' OR turadi = 'Hikaye';

3. 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

   SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi
   FROM ogrenci o, islem i, kitap k
   WHERE o.ogrno = i.ogrno
   AND i.kitapno = k.kitapno
   AND (o.sinif = '10B' OR o.sinif = '10C');

   #join ile yazın

4. Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
   SELECT o.ograd, o.ogrsoyad, i.atarih
   FROM ogrenci o
   INNER JOIN islem i ON o.ogrno = i.ogrno;

   5. Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
      SELECT k.kitapadi, t.turadi
      FROM kitap k
      INNER JOIN tur t ON k.turno = t.turno
      WHERE t.turadi = 'Fıkra' OR t.turadi = 'Hikaye';

   6. 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
      SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi
      FROM ogrenci o
      INNER JOIN islem i
      ON o.ogrno = i.ogrno
      INNER JOIN kitap k
      ON i.kitapno = k.kitapno
      WHERE o.sinif = '10B' OR o.sinif = '10C'
      ORDER BY o.ograd;

   7. Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
      SELECT o.ograd, o.ogrsoyad, i.atarih
      FROM ogrenci o
      LEFT JOIN islem i ON o.ogrno = i.ogrno;

   8. Kitap almayan öğrencileri listeleyin.
      SELECT o.ograd, o.ogrsoyad, i.atarih
      FROM ogrenci o
      LEFT JOIN islem i ON o.ogrno = i.ogrno
      WHERE i.atarih IS NULL;

   9. Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
      SELECT k.kitapno, k.kitapadi, COUNT(i.kitapno)
      FROM kitap k
      INNER JOIN islem i ON k.kitapno = i.kitapno
      GROUP BY k.kitapno
      ORDER BY k.kitapno;

   10. Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
       SELECT k.kitapno, k.kitapadi, COUNT(i.kitapno)
       FROM kitap k
       LEFT JOIN islem i ON k.kitapno = i.kitapno
       GROUP BY k.kitapno
       ORDER BY k.kitapno;

   11. Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
       SELECT o.ograd, o.ogrsoyad, k.kitapadi
       FROM ogrenci o
       INNER JOIN islem i ON o.ogrno = i.ogrno
       INNER JOIN kitap k ON i.kitapno = k.kitapno;

   12. Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
       SELECT o.ograd, o.ogrsoyad, k.kitapadi, y.yazarad, y.yazarsoyad, t.turadi, i.atarih
       FROM ogrenci o
       LEFT JOIN islem i ON o.ogrno = i.ogrno
       LEFT JOIN kitap k ON i.kitapno = k.kitapno
       LEFT JOIN yazar y ON k.yazarno = y.yazarno
       LEFT JOIN tur t ON k.turno = t.turno;

   13. 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
       SELECT o.ograd, o.ogrsoyad, COUNT(i.kitapno)
       FROM ogrenci o
       INNER JOIN islem i ON o.ogrno = i.ogrno
       WHERE o.sinif = '10A' OR o.sinif = '10B'
       GROUP BY o.ograd, o.ogrsoyad;

   14. Tüm kitapların ortalama sayfa sayısını bulunuz.
       #AVG
       SELECT AVG(k.sayfasayisi)
       FROM kitap k;

   15. Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
       SELECT k.kitapadi, k.sayfasayisi
       FROM kitap k
       WHERE k.sayfasayisi > (SELECT AVG(k.sayfasayisi)
       FROM kitap k);

   16. Öğrenci tablosundaki öğrenci sayısını gösterin
       SELECT COUNT(ogrno)
       FROM ogrenci;

   17. Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
       SELECT COUNT(ogrno) AS toplam
       FROM ogrenci;

   18. Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
       SELECT COUNT(DISTINCT o.ograd)
       FROM ogrenci o;

   19. En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
       SELECT MAX(k.sayfasayisi)
       FROM kitap k;

   20. En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
       SELECT k.kitapadi, k.sayfasayisi
       FROM kitap k
       WHERE k.sayfasayisi = (SELECT MAX(k.sayfasayisi)
       FROM kitap k);

   21. En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
       SELECT MIN(k.sayfasayisi)
       FROM kitap k;

   22. En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
       SELECT k.kitapadi, k.sayfasayisi
       FROM kitap k
       WHERE k.sayfasayisi = (SELECT MIN(k.sayfasayisi)
       FROM kitap k);

   23. Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
       SELECT MAX(k.sayfasayisi)
       FROM kitap k
       INNER JOIN tur t ON k.turno = t.turno
       WHERE t.turadi = 'Dram';

   24. numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
       #1.yöntem - sadece sayfa sayısını getirir
       SELECT SUM(k.sayfasayisi)
       FROM kitap k
       INNER JOIN islem i ON k.kitapno = i.kitapno
       WHERE i.ogrno = 15;

       #2.yöntem - öğrenci id ile birlikte getirir
       SELECT o.ogrno, sum(k.sayfasayisi)
       FROM ogrenci o
       LEFT JOIN islem i ON o.ogrno = i.ogrno
       LEFT JOIN kitap k ON i.kitapno=k.kitapno
       WHERE o.ogrno=15
       GROUP BY o.ogrno;

   25. İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
       SELECT o.ograd, COUNT(o.ograd)
       FROM ogrenci o
       GROUP BY o.ograd;

   26. Her sınıftaki öğrenci sayısını bulunuz.
       SELECT o.sinif, COUNT(o.sinif)
       FROM ogrenci o
       WHERE sinif IS NOT NULL
       GROUP BY o.sinif;

   27. Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
       SELECT o.sinif, o.cinsiyet, COUNT(o.cinsiyet)
       FROM ogrenci o
       WHERE sinif AND cinsiyet IS NOT NULL
       GROUP BY o.sinif, o.cinsiyet;

   28. Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
       SELECT o.ograd, o.ogrsoyad, SUM(k.sayfasayisi)
       FROM ogrenci o
       LEFT JOIN islem i ON o.ogrno = i.ogrno
       LEFT JOIN kitap k ON i.kitapno = k.kitapno
       GROUP BY o.ograd, o.ogrsoyad
       ORDER BY SUM(k.sayfasayisi) DESC;

   29. Her öğrencinin okuduğu kitap sayısını getiriniz.
       SELECT o.ograd, o.ogrsoyad, COUNT(i.kitapno)
       FROM ogrenci o
       LEFT JOIN islem i ON o.ogrno = i.ogrno
       GROUP BY o.ograd, o.ogrsoyad;
