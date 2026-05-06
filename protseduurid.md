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
