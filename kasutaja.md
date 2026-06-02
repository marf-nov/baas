##SQL Server – Kasutajate autentimine ja õiguste haldamine


[Põhimõisted](README.dm) | [Kasutajad](-) | [TrigeridSQL](triger.md) | [Trigerid XAMPP](xtrigger.md) | [Protseduurid](protseduurid.md) | [Võtmed/Keys](keys.md) | [Küsimused](küsimused.md) | [Sales](sales.md)

Mis on autentimine SQL Serveris?
Autentimine tähendab kasutaja tuvastamist ehk kontrollimist, kas kasutajal on õigus SQL Serverisse sisse logida.

SQL Serveris kasutatakse kahte peamist autentimise tüüpi:

    1. Windows Authentication Selle puhul kasutatakse samu kasutajaandmeid, millega logitakse sisse Windows operatsioonisüsteemi.

    Kasutajanimi ja parool on seotud Windowsiga. Turvalisem lahendus. Paroole haldab Windows. Kasutaja ei pea eraldi SQL Serveri parooli teadma.

<img width="469" height="332" alt="{9E032259-637B-4CD9-8DA0-AAEFC6A0C3F5}" src="https://github.com/user-attachments/assets/b86140de-411f-458b-9e16-92da6dc50305" />


    2. SQL Server Authentication

    Selle puhul luuakse kasutaja otse SQL Serverisse. Kasutaja ei ole seotud Windowsiga. Määratakse eraldi kasutajanimi ja parool. Sobib veebirakenduste jaoks.
    
<img width="319" height="115" alt="{651D35CF-E407-4CDD-A7E9-30AEB224B4CD}" src="https://github.com/user-attachments/assets/4e6c8e71-f078-487e-b55d-e430c6ccc2da" />




Näide kasutajast: DirectorIrina. Parool: director
Kasutaja loomine SQL Serveris

    1. Serveritaseme kasutaja loomine (Login) Sammud Ava:

Security → Logins Tee paremklikk ja vali:

<img width="702" height="655" alt="{F1B670FA-8F88-4030-8E26-A19B783C97E5}" src="https://github.com/user-attachments/assets/e268a6b5-d7c2-448c-80b7-a746f124615e" />


Harjutamiseks võib eemaldada linnukese: User must change password at next login.

Server Roles Menüüst Server Roles saab määrata serveri üldised õigused.

Tavaliselt piisab rollist: public

<img width="710" height="662" alt="{EAE79A78-A66B-4BD1-B9E9-FAB9BBDBA0E0}" src="https://github.com/user-attachments/assets/9fc7bb5f-6f4f-4cc2-820b-1f82e8e6414c" />



    2. Andmebaasi kasutaja loomine (User) Ava:

Database → Security → Users Tee paremklikk: New User...

<img width="297" height="128" alt="{29E4B165-79DF-4B0A-858B-2DD285A37823}" src="https://github.com/user-attachments/assets/49ec0d8c-02fc-4a81-ab51-68f096b4f9eb" />



Membership ja õigused Menüüst Membership saab määrata kasutaja rollid.

    db_datareader → võib lugeda SELECT

    db_datawriter → võib kirjutada INSERT, UPDATE, DELETE

<img width="706" height="498" alt="{82367D90-D481-462D-BB4B-6FB41AEDFCCA}" src="https://github.com/user-attachments/assets/ac5e4e43-3512-4c2c-8787-f0a9859a397d" />



Kasutaja õiguste kontroll

    1. tuleb sisselogida kasutajana directorAnastassia. Connect--> Database Engine

<img width="476" height="505" alt="{8F3971CF-30B9-45A1-9F09-74AD9B4478D7}" src="https://github.com/user-attachments/assets/2b5ab6f5-9001-4318-b42a-567d974afd28" />



    2. saab tabeli sisu näha ja sisestada uus kiri. 

<img width="494" height="192" alt="{C9D135A6-87E4-4F22-BA37-3B104EA03920}" src="https://github.com/user-attachments/assets/bf474d49-60bb-4a9d-ba89-4d9bd14c37c7" />



    3. kontrollime tegevus, mis ei ole lubatud kasutajale, näiteks tabeli loomine.

<img width="362" height="184" alt="{3CEE472E-7F50-4F85-B668-4EA277E28FFA}" src="https://github.com/user-attachments/assets/522442f7-c4e9-4749-a4af-ea9e2d39b759" />

    

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


<img width="546" height="187" alt="{89122010-DC74-45F7-89A8-F928266CC98B}" src="https://github.com/user-attachments/assets/d65f38d3-35b8-48c3-b796-70fe6fcc6ec5" />



SELECT Lugemine INSERT Lisamine UPDATE Muutmine DELETE Kustutamine

<img width="884" height="567" alt="{86D7F8CC-19BA-4264-A1B0-71958CAFB5C1}" src="https://github.com/user-attachments/assets/dca4e790-7879-41ab-8991-b7d37719ffe9" />

