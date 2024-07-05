-- Veritabanı oluşturma
CREATE DATABASE OkulYonetimSistemi;
USE OkulYonetimSistemi;

CREATE TABLE Ogrenciler (
    OgrenciID INT AUTO_INCREMENT PRIMARY KEY,
    Ad VARCHAR(50),
    Soyad VARCHAR(50),
    DogumTarihi DATE,
    Cinsiyet ENUM('Erkek', 'Kadin'),
    SinifID INT
);
CREATE TABLE Ogretmenler (
    OgretmenID INT AUTO_INCREMENT PRIMARY KEY,
    Ad VARCHAR(50),
    Soyad VARCHAR(50),
    DogumTarihi DATE,
    Cinsiyet ENUM('Erkek', 'Kadin'),
    Branş VARCHAR(50)
);
CREATE TABLE Dersler (
    DersID INT AUTO_INCREMENT PRIMARY KEY,
    DersAdı VARCHAR(50),
    OgretmenID INT,
    FOREIGN KEY (OgretmenID) REFERENCES Ogretmenler(OgretmenID)
);
CREATE TABLE Siniflar (
    SinifID INT AUTO_INCREMENT PRIMARY KEY,
    SinifAdı VARCHAR(50)
);
CREATE TABLE DersKayitlari (
    DersKayitID INT AUTO_INCREMENT PRIMARY KEY,
    OgrenciID INT,
    DersID INT,
    Not INT,
    FOREIGN KEY (OgrenciID) REFERENCES Ogrenciler(OgrenciID),
    FOREIGN KEY (DersID) REFERENCES Dersler(DersID)
);
INSERT INTO Siniflar (SinifAdı) VALUES ('1A'), ('1B'), ('2A'), ('2B');
INSERT INTO Ogretmenler (Ad, Soyad, DogumTarihi, Cinsiyet, Branş) VALUES 
('Ali', 'Yilmaz', '1975-05-20', 'Erkek', 'Matematik'),
('Ayse', 'Kaya', '1980-08-15', 'Kadin', 'Türkçe'),
('Mehmet', 'Demir', '1982-12-02', 'Erkek', 'Fen Bilgisi'),
('Fatma', 'Çelik', '1979-03-30', 'Kadin', 'Sosyal Bilgiler');
INSERT INTO Dersler (DersAdı, OgretmenID) VALUES 
('Matematik', 1),
('Türkçe', 2),
('Fen Bilgisi', 3),
('Sosyal Bilgiler', 4);
INSERT INTO Ogrenciler (Ad, Soyad, DogumTarihi, Cinsiyet, SinifID) VALUES 
('Ahmet', 'Yilmaz', '2005-05-20', 'Erkek', 1),
('Ayse', 'Kaya', '2006-08-15', 'Kadin', 1),
('Mehmet', 'Tekin', '2005-12-02', 'Erkek', 2),
('Fatma', 'Demir', '2006-03-30', 'Kadin', 2);
INSERT INTO DersKayitlari (OgrenciID, DersID, Not) VALUES 
(1, 1, 85),
(1, 2, 78),
(2, 1, 92),
(2, 2, 80),
(3, 3, 88),
(3, 4, 91),
(4, 3, 76),
(4, 4, 84);
SELECT * FROM Ogrenciler;
SELECT Ogrenciler.Ad, Ogrenciler.Soyad, AVG(DersKayitlari.Not) AS OrtalamaNot
FROM Ogrenciler
JOIN DersKayitlari ON Ogrenciler.OgrenciID = DersKayitlari.OgrenciID
GROUP BY Ogrenciler.OgrenciID;
SELECT Dersler.DersAdı, DersKayitlari.Not
FROM DersKayitlari
JOIN Dersler ON DersKayitlari.DersID = Dersler.DersID
WHERE DersKayitlari.OgrenciID = 1;
SELECT Branş, COUNT(*) AS OgretmenSayisi
FROM Ogretmenler
GROUP BY Branş;
