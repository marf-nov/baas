## Trigerid - päästik
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
