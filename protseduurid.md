## SQL protseduurid
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

