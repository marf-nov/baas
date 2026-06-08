



--Tabelid
--tabel kasutajad
CREATE TABLE kasutaja (
kasutaja_id INT PRIMARY KEY IDENTITY(1,1),
eesnimi VARCHAR(50),
perenimi VARCHAR(50),
email VARCHAR(150));
SELECT * FROM kasutaja;

--tabel kategoria
CREATE TABLE kategooria (
kategooria_id INT PRIMARY KEY IDENTITY(1,1),
kategooria_nimi VARCHAR(50));
SELECT * FROM kategooria;


--tabel toiduained
CREATE TABLE toiduaine (
toiduaine_id INT PRIMARY KEY IDENTITY(1,1),
toiduaine_nimi VARCHAR(100));
SELECT * FROM toiduaine;


--tabel yhik
CREATE TABLE yhik (
yhik_id INT PRIMARY KEY IDENTITY(1,1),
yhik_nimi VARCHAR(100));
SELECT * FROM yhik;


--tabel retseptid
CREATE TABLE retsept (
retsept_id INT PRIMARY KEY IDENTITY(1,1),
retsepti_nimi VARCHAR(100),
kirjeldus VARCHAR(200),
juhend VARCHAR(500),
sisestatud_kp DATE,
kasutaja_id INT,
FOREIGN KEY (kasutaja_id) REFERENCES kasutaja(kasutaja_id),
kategooria_id INT,
FOREIGN KEY (kategooria_id) REFERENCES kategooria(kategooria_id));
SELECT * FROM retsept;


--tabel koostis
CREATE TABLE koostis (
koostis_id INT PRIMARY KEY IDENTITY(1,1),
kogus INT,
retsept_retsept_id INT,
FOREIGN KEY (retsept_retsept_id) REFERENCES retsept(retsept_id),
toiduaine_id INT,
FOREIGN KEY (toiduaine_id) REFERENCES toiduaine(toiduaine_id),
yhik_id INT, FOREIGN KEY (yhik_id) REFERENCES yhik(yhik_id));
SELECT * FROM koostis;


--tehtud
CREATE TABLE tehtud (
tehtud_id INT PRIMARY KEY IDENTITY(1,1),
tehtud_kp DATE,
retsept_id INT,
FOREIGN KEY (retsept_id) REFERENCES retsept(retsept_id));
SELECT * FROM tehtud;
