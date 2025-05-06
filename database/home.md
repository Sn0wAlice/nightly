# Home

## 🧠 Introduction à SQL

SQL (prononcé “ess-ku-elle” ou “sequel”) signifie Structured Query Language. C’est un langage standardisé utilisé pour interagir avec des bases de données relationnelles.

### 🎯 À quoi sert SQL ?

SQL permet de :

* Créer et organiser des bases de données (tables, relations, etc.)
* Lire des données (avec des requêtes comme SELECT)
* Ajouter (INSERT), modifier (UPDATE) ou supprimer (DELETE) des données
* Gérer la sécurité, les droits d’accès et les performances

### 🔧 Comment fonctionne SQL ?

Les données sont stockées dans des tables, un peu comme dans un tableur :

* Chaque ligne = un enregistrement (ex: un utilisateur)
* Chaque colonne = un champ (ex: nom, email, âge)\


Exemple de table utilisateurs :

| id | nom   | email             |
| -- | ----- | ----------------- |
| 1  | Alice | alice@exemple.com |
| 2  | Bob   | bob@exemple.com   |

### 📌 Quelques commandes de base :

* SELECT \* FROM utilisateurs; → lire toutes les données
* INSERT INTO utilisateurs (nom, email) VALUES ('Charlie', 'charlie@exemple.com');
* UPDATE utilisateurs SET email = 'bob@newmail.com' WHERE id = 2;
* DELETE FROM utilisateurs WHERE id = 1;

### 🛠️ Où utilise-t-on SQL ?

SQL est utilisé dans la plupart des applications modernes : sites web, logiciels d’entreprise, services cloud, etc. Des systèmes comme MySQL, PostgreSQL, SQLite, SQL Server utilisent SQL.

