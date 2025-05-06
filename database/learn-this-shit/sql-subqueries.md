# SQL - Subqueries

## Qu’est-ce qu’une sous-requête ?

Une sous-requête est une requête SQL imbriquée dans une autre requête.

Elle permet d’utiliser le résultat d’une requête comme valeur ou source de données pour une autre.



Il existe deux grandes familles :

1. Sous-requêtes scalaires (renvoient une seule valeur)
2. Sous-requêtes relationnelles (renvoient une ou plusieurs lignes)

***

### Table d’exemple utilisée :&#x20;

| id | nom   | age | pays     | actif |
| -- | ----- | --- | -------- | ----- |
| 1  | Alice | 31  | France   | 1     |
| 2  | Bob   | 24  | Belgique | 0     |
| 3  | Clara | 36  | France   | 1     |
| 4  | David | 29  | France   | 1     |
| 5  | Emma  | 20  | France   | 0     |

***

### 1. Sous-requête scalaire (dans WHERE)

#### Objectif : afficher les utilisateurs ayant l’âge&#x20;

#### maximum

```sql
SELECT * FROM utilisateurs
WHERE age = (SELECT MAX(age) FROM utilisateurs);
```

Sous-requête : SELECT MAX(age) → retourne 36

Requête principale : sélectionne les utilisateurs où age = 36

Résultat :

| id | nom   | age | pays   | actif |
| -- | ----- | --- | ------ | ----- |
| 3  | Clara | 36  | France | 1     |

***

### 2. Sous-requête dans IN

Objectif : afficher les utilisateurs qui viennent de pays apparaissant dans une autre table

Supposons cette table supplémentaire :

Table pays\_autorises

| nom\_pays |
| --------- |
| France    |
| Belgique  |

Requête :

```sql
SELECT * FROM utilisateurs
WHERE pays IN (SELECT nom_pays FROM pays_autorises);
```

Sous-requête : retourne ('France', 'Belgique')

Requête principale : sélectionne tous les utilisateurs venant de ces pays

Résultat : Tous les utilisateurs sauf ceux d’un pays non listé (dans notre exemple, tous sont conservés)

***

### 3. Sous-requête dans SELECT (valeur dérivée)

#### Objectif : afficher chaque utilisateur avec l’âge moyen de tous les utilisateurs

```sql
SELECT nom, age,
  (SELECT AVG(age) FROM utilisateurs) AS age_moyen
FROM utilisateurs;
```

Sous-requête : calcule une seule fois l’âge moyen (ici : (31+24+36+29+20)/5 = 28)

Requête principale : ajoute cette valeur dans chaque ligne

Résultat :

| nom   | age | age\_moyen |
| ----- | --- | ---------- |
| Alice | 31  | 28         |
| Bob   | 24  | 28         |
| Clara | 36  | 28         |
| David | 29  | 28         |
| Emma  | 20  | 28         |

***

### 4. Sous-requête dans FROM

#### Objectif : afficher uniquement les utilisateurs dont l’âge est au-dessus de la moyenne

```sql
SELECT u.nom, u.age
FROM utilisateurs u
JOIN (SELECT AVG(age) AS moyenne FROM utilisateurs) m
ON u.age > m.moyenne;
```

Sous-requête : retourne moyenne = 28

Requête principale : garde les utilisateurs dont age > 28

Résultat :

| nom   | age |
| ----- | --- |
| Alice | 31  |
| Clara | 36  |
| David | 29  |

{% hint style="info" %}
On check les join juste apres
{% endhint %}

***

### Bonnes pratiques

* Toujours tester la sous-requête seule d’abord pour vérifier son résultat
* Vérifier que les sous-requêtes scalaires ne retournent qu’une seule ligne
* Préférer les sous-requêtes dans FROM pour des calculs complexes ou intermédiaires
