


```sql
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
```

<img width="252" height="306" alt="image" src="https://github.com/user-attachments/assets/18981812-bd16-4283-b796-ee81b1562a2d" />

```sql
--Protseduurid
--Lisamine kasutaja
CREATE Procedure lisaKasutaja
@eesnimi varchar(50),
@perenimi varchar(50),
@email varchar(150)
AS
BEGIN
	INSERT INTO kasutaja
	VALUES (@eesnimi,@perenimi,@email);
	SELECT * FROM kasutaja;
END;

--kutse
EXEC lisaKasutaja 'Yelisaveta','Ilnitskaja','eli@gmail.com';
```

<img width="690" height="249" alt="image" src="https://github.com/user-attachments/assets/b67bbbe7-e6d2-4f2d-8212-0e3845f2c88a" />

```sql
--Lisamine kategooria
CREATE Procedure lisaKategooria
@nimi varchar(30)
AS
BEGIN
	INSERT INTO kategooria(kategooria_nimi)
	VALUES (@nimi);
	SELECT * FROM kategooria;
END;

--kutse
EXEC lisaKategooria 'Magus toit';
```

<img width="511" height="510" alt="image" src="https://github.com/user-attachments/assets/ccb3a58e-ed36-478b-bf7b-dbae6cbba973" />

```sql
--Lisamine toiduaine
CREATE Procedure lisaToiduaine
@nimi varchar(100)
AS
BEGIN
	INSERT INTO toiduaine(toiduaine_nimi)
	VALUES (@nimi);
	SELECT * FROM toiduaine;
END;

--kutse
EXEC lisaToiduaine 'Oli';
```

<img width="488" height="563" alt="image" src="https://github.com/user-attachments/assets/0b5300a6-28d7-4c9b-99d5-3b177c884255" />

```sql
--Lisamine yhik
CREATE Procedure lisaYhik
@nimi varchar(100)
AS
BEGIN
	INSERT INTO yhik(yhik_nimi)
	VALUES (@nimi);
	SELECT * FROM yhik;
END;

--kutse
EXEC lisaYhik 'ml';
```
<img width="407" height="529" alt="image" src="https://github.com/user-attachments/assets/accf0ce9-364e-4b08-a4a9-0825ba9d9695" />

```sql
--Lisamine retsept
CREATE PROCEDURE lisaRetsept
@nimi VARCHAR(100),
@kirjeldus VARCHAR(200),
@juhend VARCHAR(500),
@kp DATE,
@kasutaja INT,
@kategooria INT
AS
BEGIN
	INSERT INTO retsept(retsepti_nimi,kirjeldus,juhend,sisestatud_kp,kasutaja_id,kategooria_id)
	VALUES(@nimi,@kirjeldus,@juhend,@kp,@kasutaja,@kategooria);
	SELECT * FROM retsept;
END;
--kutse
EXEC lisaRetsept 'Uusalgus salat','Kerge salat soobib inimestel kes on dietel', 'Toomat, kurk, tunakala', '2026-11-22',2,2;
```
<img width="827" height="492" alt="image" src="https://github.com/user-attachments/assets/442d8f65-310c-45d0-955a-d909aa955758" />

```sql
--Lisamine koostised
CREATE PROCEDURE lisaKoostis
@kogus INT,
@retsept INT,
@toiduaine INT,
@yhik INT
AS
BEGIN
	INSERT INTO koostis(kogus,retsept_retsept_id,toiduaine_id,yhik_id)
	VALUES(@kogus,@retsept,@toiduaine,@yhik);
END;
--kutse
EXEC lisaKoostis 3,4,6,1;
```
<img width="743" height="454" alt="image" src="https://github.com/user-attachments/assets/1592049a-8a60-46e5-a71b-15dce9bef576" />

```sql
--Lisamine tehtud
CREATE PROCEDURE lisaTehtud
@kuupaev DATE,
@retsept INT
AS
BEGIN
	INSERT INTO tehtud(tehtud_kp,retsept_id)
	VALUES(@kuupaev,@retsept);
	SELECT * FROM tehtud;
END;
--kutse
EXEC lisaTehtud '2026-05-26',3;

```
<img width="524" height="430" alt="image" src="https://github.com/user-attachments/assets/7b1d01e4-e70b-4351-8c58-25ac01af169c" />

```sql
--Muutmine tabelid
CREATE PROCEDURE muudaTabel
	@tegevus VARCHAR(10),
	@tabelinimi VARCHAR(50),
	@veerunimi VARCHAR(50),
	@tyyp VARCHAR(50)=NULL
AS
BEGIN
	DECLARE @sqltegevus VARCHAR(MAX)

	SET @sqltegevus = CASE
		WHEN @tegevus='add' THEN
			CONCAT ('ALTER TABLE ', @tabelinimi, ' ADD ', @veerunimi, ' ', @tyyp)

		WHEN @tegevus='drop' THEN
			CONCAT('ALTER TABLE ', @tabelinimi, ' DROP COLUMN ', @veerunimi)

		WHEN @tegevus='alter' THEN
			CONCAT('ALTER TABLE ', @tabelinimi, ' ALTER COLUMN ', @veerunimi, ' ', @tyyp)

		END;

	PRINT @sqltegevus;
	EXEC(@sqltegevus);

END;
```

<img width="691" height="274" alt="image" src="https://github.com/user-attachments/assets/3e4a50e2-aeb7-43e6-a3fa-2f4bb022cce0" />

```sql
--select
--Päring kuvab kasutaja eesnime, perekonnanime ja tema retseptide nimetused.
SELECT kasutaja.eesnimi, kasutaja.perenimi, retsept.retsepti_nimi FROM kasutaja, retsept
WHERE kasutaja.kasutaja_id=retsept.kasutaja_id;
```

<img width="761" height="276" alt="image" src="https://github.com/user-attachments/assets/ac7a4bd7-a358-479a-887f-1ce454676945" />

```sql
--Päring kuvab retsepti nimetuse ja sellele vastava kategooria.
SELECT retsept.retsepti_nimi,kategooria.kategooria_nimi FROM retsept, kategooria
WHERE retsept.kategooria_id=kategooria.kategooria_id;
```

<img width="800" height="238" alt="image" src="https://github.com/user-attachments/assets/20e917a9-d438-427a-b667-d38bcdfe88c1" />

```sql
--Päring kuvab koostises kasutatud toiduained ja nende kogused.
SELECT toiduaine.toiduaine_nimi, koostis.kogus FROM toiduaine, koostis
WHERE toiduaine.toiduaine_id=koostis.toiduaine_id;
```

<img width="785" height="216" alt="image" src="https://github.com/user-attachments/assets/240a7d45-3899-42fe-834e-140c1162f00c" />

```sql
--Too lisamine
--kommentaar table
CREATE TABLE kommentaar(
kommentaar_id INT IDENTITY(1,1) PRIMARY KEY,
tekst VARCHAR(200),
retsept_id INT,
FOREIGN KEY(retsept_id) REFERENCES retsept(retsept_id));

--Protseduurid
CREATE PROCEDURE lisaKommentaar
@tekst VARCHAR(200),
@retsept INT
AS
BEGIN
	INSERT INTO kommentaar
	VALUES(@tekst,@retsept);
	SELECT * FROM kommentaar;
END;

EXEC lisaKommentaar 'Väga hea töö', 4;
```

<img width="489" height="438" alt="image" src="https://github.com/user-attachments/assets/ef0eea69-c7a0-4c43-af11-8f6ad93d70f4" />


```sql
--kustutamine protseduurid
CREATE PROCEDURE kustutaKommentaar
@id INT
AS
BEGIN
	DELETE FROM kommentaar
	WHERE kommentaar_id=@id;
	SELECT * FROM kommentaar;
END;


EXEC kustutaKommentaar 161;
```
<img width="415" height="417" alt="image" src="https://github.com/user-attachments/assets/1638ded2-e76a-4b8c-b8de-042d134a59f5" />


```sql
--Staff kontroll 
USE RetseptiRaamat;

INSERT INTO toiduaine
VALUES ('Banaan');
```

<img width="412" height="306" alt="image" src="https://github.com/user-attachments/assets/b6fe4979-7404-4550-b5a6-af61e14f143a" />


```sql
SELECT * FROM kasutaja;
```

<img width="419" height="371" alt="image" src="https://github.com/user-attachments/assets/0a82a17c-15d3-401e-85af-67006a79cc31" />


```sql
UPDATE toiduaine SET toiduaine_nimi='Test'
WHERE toiduaine_id=1;
```

<img width="512" height="261" alt="image" src="https://github.com/user-attachments/assets/b0dd58bb-f3bb-4167-8325-0282e5b0a2e2" />



```sql
DELETE FROM kategooria
WHERE kategooria_id=1;

```

<img width="424" height="324" alt="image" src="https://github.com/user-attachments/assets/d1665dd8-a79b-4964-a782-3883d3e5ad46" />

























