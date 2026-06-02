## Andmetebaaside komspektid

[Põhimõisted](README.dm) | [Kasutajad](-) | [TrigeridSQL](triger.md) | [Trigerid XAMPP](xtrigger.md) | [Protseduurid](protseduurid.md) | [Võtmed/Keys](-)



AndmebaasidLOGITpv24

andmebaaside haldusega seotud sql kood ja konspektid
Sisukord

    Põhimõisted
    Piirangud
    Andmetüübid

Põhimõisted

    Andmebaasi haldussüsteemid - tarkvara, millega abil saab luua andmebaas (mariaDB - XAMPP, SQL Server - SQL Server Management Studio (Server Name - (localdb)\MSSQLLocalDB))
    Andmebaas - struktureeritud andmete kogum
    Tabel - olem - сущности
    Veerg = väli - поле
    Rida = kirje - запись
    Primaarne võti - primary key -PK- veerg, unikaalse identifikatooriga (tavaliselt nimetakse id)
    Välisvõti (võõrvõti) - foreign key - FK - veerg, mis loob seose teise tabeli primaarne võtmega

SQL - structured query language - struktureeritud päringu keel

    päring - запрос
    {858DD690-0B0E-4468-8390-C9E340768F1B}

    DDL - Data Definition Language

    DML - Data Manipulation Language
    Piirangud
        ограничения - CONSTRAINT (5)
        PRIMARY KEY
        NOT NULL
        CHECK - valik
        UNIQUE
        FOREIGN KEY
    Andmetüübid

    1. int, smallint, decimal(5,2) - numbrilised
    2. varchar(30), char(5), TEXT - tekst/sümbolised
    3. date, time, datetime - kuupäeva
    4. boolean, bit, bool - loogilised

Tabelivahelised seosed

    üks-ühele (nt naine-mees)

    üks-mitmele (nt naine-lapsed) {F53CB300-386A-46D2-B084-D6DFE2CD9C21}

    mitme-mitmele (õpilased-tunnid)

ALTER TABLE - tabelii struktuuri muutmine

--1. uue veeru lisamine
ALTER TABLE tootaja ADD testVeerg int;
Select * from tootaja;
--2. veeru kustutamine
ALTER TABLE tootaja DROP COLUMN testVeerg;
--3. andmetüübi muutmine veerus
ALTER TABLE tootaja ALTER COLUMN testVeerg varchar(5);
--struktuuri kontrollimiseks kasutame protseduur sp_help
sp_help tootaja;
