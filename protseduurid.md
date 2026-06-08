## SQL protseduurid


[Põhimõisted](README.dm) | [Kasutajad](kasutaja.md) | [TrigeridSQL](triger.md) | [Trigerid XAMPP](xtrigger.md) | [Protseduurid](protseduurid.md) | [Võtmed/Keys](keys.md) | [Küsimused](küsimused.md) | [Sales](sales.md) | [Retseptiraamat](retseptiraamat.md)

store protseduur - salvestatud protseduurid - sama mis on funktsioonid programmeerimises, mingi tegevus, mis on salvestatud andmebaasi, ja mida saab autmaatsel teha (INSERT, SELECT, UPDATE)

```sql
--1.categories
create table categories(
category_id int PRIMARY KEY identity(1,1),
category_name varchar(25) UNIQUE);

INSERT INTO categories(category_name)
VALUES ('Arvuti');

SELECT * FROM categories;
--protseduur mis lisab andmeid tabelisse ja kohe kuvab neid(INSERT, SELECT)
Create Procedure lisaCategories
--protseduurid
@uusKategooria varchar(36)
AS
BEGIN
--kirjeldus
   INSERT into categories(category_name)
   VALUES (@uusKategooria);
   SELECT * FROM categories;
END;
--kutse
EXEC lisaCategories 'Telefon';
```
<img width="272" height="238" alt="{8A7C79A9-E683-4650-BB93-562BFAC75733}" src="https://github.com/user-attachments/assets/f95061d9-177f-4225-b967-b7654c1df76f" />

```sql
-- protsiduur mis kustutab kategooria id järgi
Create procedure kustutaCategories
@kustutaid int
as
begin
   select * from categories;
   delete from categories where category_id=@kustutaid;
   select * from categories;
end

--kutse 
exec kustutaCategories 1
```
<img width="249" height="395" alt="{02400186-0473-4DF9-B578-688A0C5206AF}" src="https://github.com/user-attachments/assets/575ed2ef-d239-4b23-b1ca-da81e234e21b" />

```sql
--protsiduur mis kuvab kategooriad sisestatud esimese tahe järgi
Create procedure otsingtaht
@taht char(1)
as
begin
   select * from categories
   where category_name like @taht + '%';
end

--kutse
exec otsingtaht 'T'
```
<img width="269" height="201" alt="{4DF8F7A5-B2E3-404E-9798-AB42B4D7D1D1}" src="https://github.com/user-attachments/assets/16f4bdd5-b563-4077-b318-bdbc7be67d45" />

```sql
--brends
CREATE TABLE brands(
brand_id int PRIMARY KEY identity(1,1),
brand_name varchar(15) UNIQUE);

INSERT INTO brands(brand_name)
VALUES ('Motorola');
SELECT * FROM brands;


--products
Create TABLE products(
product_id int PRIMARY KEY identity(1,1),
product_name varchar(50) not null,
brand_id int,
FOREIGN KEY (brand_id) references  brands(brand_id),
category_id int,
FOREIGN KEY (category_id) references categories(category_id),
model_year int,
list_price money);

SELECT * FROM products;

INSERT INTO products
VALUES ('Motorola CB12', 3, 2, 2020, 20026);

--protseduur, mis kuvab tooded kus on hind suurem, kui sisetatud hind
Create procedure suuremHind
@hind int
as
begin
    select * from products
	where list_price >@hind;
end;

--kutse
exec suuremhind 20023
```
<img width="488" height="231" alt="{68673457-EFD6-4B3F-9579-32B1702BCF6A}" src="https://github.com/user-attachments/assets/87e452d4-e96c-433d-97da-806f1d90cf3b" />

```sql
--5. output parameetrid (min ja max)
CREATE PROCEDURE minmaxHind
    @minHind MONEY OUTPUT,
    @maxHind MONEY OUTPUT
AS
BEGIN
    SELECT 
        @minHind = MIN(list_price),
        @maxHind = MAX(list_price)
    FROM products;
END;

--kutse
DECLARE @minHind MONEY, @maxHind MONEY;

EXEC minmaxHind @minHind OUTPUT, @maxHind OUTPUT;

PRINT 'Min hind = ' + CONVERT(varchar, @minHind);
PRINT 'Max hind = ' + CONVERT(varchar, @maxHind);
```
<img width="504" height="310" alt="{5EC5DE18-BF34-492D-93B2-4513842A7E54}" src="https://github.com/user-attachments/assets/a9893015-0fb6-4c95-b5d2-e0cf82157d4b" />

```sql
--6. ALTER TABLE - universaalne protseduur; mis tootab uks millise tabeliga
--muudab struktuuri(veeru lisamine -ADD, veeru kustamine -DROP
CREATE PROCEDURE muudatus
    @tegevus varchar(10),
    @tabelinimi varchar(25),
    @veerunimi varchar(25),
    @tyyp varchar(25) = NULL
AS
BEGIN
    DECLARE @sqltegevus varchar(max);

    SET @sqltegevus = CASE 
        WHEN @tegevus = 'add' THEN 
            CONCAT('ALTER TABLE ', @tabelinimi, ' ADD ', @veerunimi, ' ', @tyyp)

        WHEN @tegevus = 'drop' THEN 
            CONCAT('ALTER TABLE ', @tabelinimi, ' DROP COLUMN ', @veerunimi)
    END;

    PRINT @sqltegevus;
    EXEC (@sqltegevus);
END;

--kutse
--add
exec muudatus 'add', 'categories', 'testveerg', 'int';
SELECT * FROM categories;
--drop
exec muudatus 'drop', 'categories', 'testveerg', 'int';
SELECT * FROM categories;
```
<img width="493" height="312" alt="{851C8CA5-93A9-4E9E-A2B6-C19F95DB7B87}" src="https://github.com/user-attachments/assets/b5ece80f-10b5-40a7-9125-c9a15c35b248" />
<img width="517" height="332" alt="{8AFC503B-7E07-40F2-9D7A-79D31EFEEFC3}" src="https://github.com/user-attachments/assets/bf0acc80-c635-494e-840a-a9af32570507" />




