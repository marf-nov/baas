## Trigerid - päästik

[Põhimõisted](README.md) | [Kasutajad](kasutaja.md) | [TrigeridSQL](triger.md) | [Trigerid XAMPP](xtrigger.md) | [Protseduurid](protseduurid.md) | [Võtmed/Keys](keys.md) | [Küsimused](küsimused.md) | [Sales](sales.md) | [Retseptiraamat](retseptiraamat.md)

SQL triggerid on spetsiaalsed andmebaasi objektid, mis käivituvad automaatselt, kui toimub teatud sündmus
```sql
--Jälgib andmete sisestamine tabelis linnad ja teeb vastava kirje tabelis logi
CREATE TRIGGER linnaLisamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR INSERT
AS
INSERT INTO logi(kuupaev, kasutaja, toiming, andmed)
SELECT
GETDATE(),  --kuupaev
system_user, --kasutaja mis on sisselogitud serverisse
'on tehtud INSERT käsk',  --toiming
concat('linn: ',inserted.linnanimi, 'rahvaarv: ', inserted.rahvaarv)  --andmed tabelist linnad
FROM inserted;
```
<img width="773" height="726" alt="{734A1D8C-8D69-4ABE-A03B-EE82790571DF}" src="https://github.com/user-attachments/assets/e224a2b8-7321-4a17-acba-efa00e736937" />

```sql
-- Delete triger
CREATE TRIGGER linnaKustutamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR DELETE
AS
INSERT INTO logi(kuupaev, kasutaja, toiming, andmed)
SELECT
GETDATE(),  --kuupaev
system_user, --kasutaja mis on sisselogitud serverisse
'on tehtud DELETE käsk',  --toiming
concat('linn: ', deleted.linnanimi, 'rahvaarv: ', deleted.rahvaarv)  --andmed tabelist linnad
FROM deleted;
```

<img width="579" height="330" alt="{17DD4A7A-4937-42E4-AA23-A2732BFD7CCB}" src="https://github.com/user-attachments/assets/87069bc3-14a0-4ab7-9fec-a418a1ef888b" />

```sql
--Kombineerime INSERT ja Delete triggerid
DISABLE trigger linnalisamine ON linnad;
disable trigger linnaKustutamine ON linnad;


CREATE TRIGGER linnaLisaminejakus
ON linnad --tabelinimi, mis on vaja jälgida
FOR INSERT, Delete
AS
BEGIN
set nocount on;
	INSERT INTO logi(kuupaev, kasutaja, toiming, andmed)

	SELECT
	GETDATE(),  --kuupaev
	system_user, --kasutaja mis on sisselogitud serverisse
	'on tehtud INSERT käsk',  --toiming
	concat('linn: ', inserted.linnanimi, 'rahvaarv: ', inserted.rahvaarv)  --andmed tabelist linnad
	FROM inserted

	UNION all

	SELECT
	GETDATE(),  --kuupaev
	system_user, --kasutaja mis on sisselogitud serverisse
	'on tehtud DELETE käsk',  --toiming
	concat('linn: ', deleted.linnanimi, 'rahvaarv: ', deleted.rahvaarv)  --andmed tabelist linnad
	FROM deleted;
End;
```

<img width="573" height="615" alt="{7ED270F0-C7B2-4554-BDA2-76830EDEC7DB}" src="https://github.com/user-attachments/assets/a6c97b3d-3afa-452a-8334-284d3150acd3" />

```sql
--update
CREATE TRIGGER linnaUendamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR UPDATE
AS
INSERT INTO logi(kuupaev, kasutaja, toiming, andmed)
SELECT
GETDATE(),  --kuupaev
system_user, --kasutaja mis on sisselogitud serverisse
'on tehtud UPDATE käsk',  --toiming
concat('vanad andmed - linn: ', deleted.linnanimi, 'rahvaarv: ', deleted.rahvaarv,
'uued andmed - linn: ', inserted.linnanimi, ', rahvaarv - ', inserted.rahvaarv)  --andmed tabelist linnad
FROM deleted INNER JOIN inserted
ON deleted.linnID=inserted.linnID;
```
<img width="579" height="362" alt="{849D6261-995B-43EE-A67B-2C57AE8920BA}" src="https://github.com/user-attachments/assets/2b3bf47d-1f77-4220-8d2a-e49d145865d6" />

```sql
--kasutaja sekretaarMariia, parool 12345
--õigused - sekretaatMariia ei saa luua ehk muuta trigerid, ei häi tabeli logi,
--saab ainult näha, lisada ja kasutada tabelist linnad

Grant select, insert, delete on linnad to sekretaarMariia;
deny select, delete on linnad to sekretaarMariia;
```
<img width="570" height="169" alt="{180DB6A9-CA99-4857-887F-AD6E1724B824}" src="https://github.com/user-attachments/assets/7aefc9bb-3a5d-444d-9140-d9d312c8b492" />
<img width="374" height="146" alt="{0630F73F-4F4E-457E-9B2B-F4F7C5A50E52}" src="https://github.com/user-attachments/assets/b078a44b-a224-42c6-892d-d1f06e0c86a5" />
help
<img width="350" height="365" alt="{039CCB6C-F195-474C-BEFE-04DD7469D2DA}" src="https://github.com/user-attachments/assets/00551115-b7af-463e-8e98-3393c86c7797" />

