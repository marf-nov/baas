## SQL Keys (võtmed andmebaasis)

[Põhimõisted](README.dm) | [Kasutajad](kasutaja.md) | [TrigeridSQL](triger.md) | [Trigerid XAMPP](xtrigger.md) | [Protseduurid](protseduurid.md) | [Võtmed/Keys](keys.md) | [Küsimused](küsimused.md) | [Sales](sales.md)

## Primary Key

Primary Key on väli, mis identifitseerib iga kirje tabelis unikaalselt.

1. ei tohi olla NULL
2. tabelis saab olla ainult üks Primary Key
3. kasutatakse andmete unikaalseks tuvastamiseks
```sql
CREATE TABLE Raamat (
RaamatID int IDENTITY(1,1) PRIMARY KEY,
Pealkiri varchar(100)
)
```



RaamatID on iga raamatu unikaalne number.

## Foreign Key

Foreign Key on väli, mis viitab teise tabeli Primary Key-le.

1. kasutatakse tabelite sidumiseks
2. ei pea olema unikaalne
```sql
CREATE TABLE Osakond (
OsakondID int IDENTITY(1,1) PRIMARY KEY,
Nimetus varchar(50)
)

CREATE TABLE Töötaja (
TöötajaID int IDENTITY(1,1) PRIMARY KEY,
Nimi varchar(50),
OsakondID int,
FOREIGN KEY (OsakondID) REFERENCES Osakond(OsakondID)
)
```



Töötaja on seotud osakonnaga läbi OsakondID.

## Unique Key

Unique Key tagab, et veeru väärtused ei kordu.

1. väärtused peavad olema unikaalsed
2. tavaliselt lubab 1 NULL väärtuse
3. ühes tabelis võib olla mitu UNIQUE võtme
```sql
CREATE TABLE Liige (
LiigeID int IDENTITY(1,1) PRIMARY KEY,
Kasutajanimi varchar(50) UNIQUE
)
```



Kasutajanimed peavad olema erinevad.

## Simple Key

Simple Key koosneb ühest veerust.
```sql
CREATE TABLE Film (
FilmID int IDENTITY(1,1) PRIMARY KEY,
Pealkiri varchar(100)
)
```



FilmID on üksik võti.

## Composite Key

Composite Key koosneb mitmest veerust.
```sql
CREATE TABLE Broneering (
KlientID int,
TubaID int,
PRIMARY KEY (KlientID, TubaID)
)
```



KlientID ja TubaID koos moodustavad võtme.

## Compound Key

Compound Key on mitmest veerust koosnev võti.
```sql
CREATE TABLE Broneering (
KlientID int,
TubaID int,
PRIMARY KEY (KlientID, TubaID)
)
```



Raamat ja liige koos määravad ühe laenutuse.

## Super Key

Super Key on üks või mitu veergu, mis tuvastavad kirje unikaalselt.

1.võib sisaldada lisaveerge
```sql
CREATE TABLE Auto (
AutoID int IDENTITY(1,1) PRIMARY KEY,
RegNumber varchar(20),
VIN varchar(50)
)
```



Näited Super Key-dest:
- AutoID
- (AutoID, RegNumber)
- (AutoID, VIN)

AutoID üksi on juba piisav kirje tuvastamiseks.

## Candidate Key

Candidate Key on võimalikud võtmed, millest saab valida Primary Key.
```sql
CREATE TABLE Konto (
KontoID int IDENTITY(1,1) PRIMARY KEY,
Kasutajanimi varchar(50) UNIQUE,
Email varchar(100) UNIQUE
)
```



Kasutajanimi ja Email võivad mõlemad olla Candidate Key.

## Alternate Key

Alternate Key on Candidate Key, mis ei ole Primary Key.
```sql
CREATE TABLE Mängija (
MängijaID int IDENTITY(1,1) PRIMARY KEY,
Hüüdnimi varchar(50) UNIQUE
)
```



Primary Key on MängijaID ja Alternate Key on Hüüdnimi.
















