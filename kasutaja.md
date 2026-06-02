##SQL Server – Kasutajate autentimine ja õiguste haldamine

[Põhimõisted](README.md) | [Kasutajad](kasutaja.md) | [ Trigerid](triger.md) | [Protseduurid](protseaduurid.md) | [Sales](sales.md) | [Võtmed/Keys](kodutoo)

Mis on autentimine SQL Serveris?
Autentimine tähendab kasutaja tuvastamist ehk kontrollimist, kas kasutajal on õigus SQL Serverisse sisse logida.

SQL Serveris kasutatakse kahte peamist autentimise tüüpi:

    1. Windows Authentication Selle puhul kasutatakse samu kasutajaandmeid, millega logitakse sisse Windows operatsioonisüsteemi.

    Kasutajanimi ja parool on seotud Windowsiga. Turvalisem lahendus. Paroole haldab Windows. Kasutaja ei pea eraldi SQL Serveri parooli teadma.

<img width="469" height="332" alt="{9E032259-637B-4CD9-8DA0-AAEFC6A0C3F5}" src="https://github.com/user-attachments/assets/b86140de-411f-458b-9e16-92da6dc50305" />


    2. SQL Server Authentication

    Selle puhul luuakse kasutaja otse SQL Serverisse. Kasutaja ei ole seotud Windowsiga. Määratakse eraldi kasutajanimi ja parool. Sobib veebirakenduste jaoks.

<img width="262" height="211" alt="{8C4815B7-6C0C-41F1-A27B-9682AD8DA07C}" src="https://github.com/user-attachments/assets/a6fdf15e-3189-4d5e-8bc1-5bf4a1507387" />


Näide kasutajast: DirectorIrina. Parool: director
Kasutaja loomine SQL Serveris

    1. Serveritaseme kasutaja loomine (Login) Sammud Ava:

Security → Logins Tee paremklikk ja vali:

New Login...
<img width="701" height="603" alt="{8D249765-B9C4-4BE8-A92F-393E1D80E2DB}" src="https://github.com/user-attachments/assets/d5747ccf-f5e9-4ad4-af77-c62a5c45e01c" />

Harjutamiseks võib eemaldada linnukese: User must change password at next login.

Server Roles Menüüst Server Roles saab määrata serveri üldised õigused.

Tavaliselt piisab rollist: public
<img width="693" height="602" alt="{E2779A95-8A79-4827-99EB-702405F4F195}" src="https://github.com/user-attachments/assets/9e4f17e1-fc63-4177-bc03-b5db56450f1d" />


    2. Andmebaasi kasutaja loomine (User) Ava:

Database → Security → Users Tee paremklikk: New User...
<img width="236" height="182" alt="{3A648CE3-3F78-4BC2-9BDE-0FCAF7E00620}" src="https://github.com/user-attachments/assets/5dcf9798-e6db-4d27-bead-11eec3d82726" />


Membership ja õigused Menüüst Membership saab määrata kasutaja rollid.

    db_datareader → võib lugeda SELECT

    db_datawriter → võib kirjutada INSERT, UPDATE, DELETE

<img width="697" height="467" alt="{925E8193-BEF6-47C3-8EFA-26BAE23B326E}" src="https://github.com/user-attachments/assets/58e88889-5d74-41d3-9f66-b2d62de7cdc0" />


Kasutaja õiguste kontroll

    1. tuleb sisselogida kasutajana directorAnastassia. Connect--> Database Engine

<img width="466" height="502" alt="{19BF020E-1656-4B12-A468-7A437D00602F}" src="https://github.com/user-attachments/assets/89ae2f15-b95a-4fc1-995a-8c59d4cdf01e" />


    2. saab tabeli sisu näha ja sisestada uus kiri. 

<img width="785" height="378" alt="{29AAE034-1455-4AA0-AE39-40BB66002C46}" src="https://github.com/user-attachments/assets/1193f159-069c-42ea-8eda-b7b7e6f20d9f" />


    3. kontrollime tegevus, mis ei ole lubatud kasutajale, näiteks tabeli loomine.

<img width="681" height="259" alt="{38ADBD11-4CEC-4BB2-8C6B-7167C6403BCE}" src="https://github.com/user-attachments/assets/55b8b36b-4134-4696-917e-0e19c7e37f45" />

    

#### SQL Server Authentication Mode muutmine
Kui ilmub viga: Error 18456, siis on tavaliselt lubatud ainult Windows Authentication.
Lahendus: Server → Properties -->
Security
 Vali: SQL Server and Windows Authentication mode

--GRANT - õiguste määramine
--DENY - õiguste keelamine

--db_datareader -SELECT 
--db_datawriter - INSERT, DELETE, UPDATE

--anname kasutajale directorIrina õigus 
--ainult kustutada ja uuendada tabelit 
--(DELETE, UPDATE, SELECT)

GRANT DELETE ON puhkus TO directorLiisa;
GRANT UPDATE ON puhkus TO directorLiisa;
GRANT SELECT ON puhkus TO directorLiisa;

--keelame INSERT
DENY INSERT ON puhkus TO directorLiisa;


<img width="811" height="546" alt="{EA3F706E-507A-49E7-B573-527F372DE904}" src="https://github.com/user-attachments/assets/04bf3a67-ec2e-4d9f-b3c6-e8219fa67b3d" />


SELECT Lugemine INSERT Lisamine UPDATE Muutmine DELETE Kustutamine

<img width="884" height="567" alt="{86D7F8CC-19BA-4264-A1B0-71958CAFB5C1}" src="https://github.com/user-attachments/assets/dca4e790-7879-41ab-8991-b7d37719ffe9" />

