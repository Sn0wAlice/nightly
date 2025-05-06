# SQL - Select

## Introduction : Le concept de table

En SQL, les données sont organisées dans des tables.

Une table ressemble à un tableau avec :

* des colonnes (champs, comme nom, âge, email)
* des lignes (chaque ligne est un enregistrement, ou "row")

**Exemple de table utilisateurs :**

| 1 | Alice | 30 | France   | 1 |
| - | ----- | -- | -------- | - |
| 2 | Bob   | 22 | Belgique | 0 |
| 3 | Clara | 35 | France   | 1 |
| 4 | David | 28 | Canada   | 1 |
| 5 | Emma  | 19 | France   | 0 |

***

## 1. La commande SELECT

La commande SELECT permet de lire des données dans une table.

Syntaxe de base :

```sql
SELECT colonne1, colonne2 FROM nom_de_table;
```

Exemple :

```sql
SELECT nom, age FROM utilisateurs;
```

Cela retourne tous les noms et âges de la table utilisateurs.

| Alice | 30 |
| ----- | -- |
| Bob   | 22 |
| Clara | 35 |
| David | 28 |
| Emma  | 19 |

Afficher toutes les colonnes :

```sql
SELECT * FROM utilisateurs;
```

| 1 | Alice | 30 | France   | 1 |
| - | ----- | -- | -------- | - |
| 2 | Bob   | 22 | Belgique | 0 |
| 3 | Clara | 35 | France   | 1 |
| 4 | David | 28 | Canada   | 1 |
| 5 | Emma  | 19 | France   | 0 |

***

## 2. Filtrer les résultats avec WHERE

La clause WHERE permet de filtrer les lignes selon une condition.

Exemple : utilisateurs âgés de plus de 25 ans

```sql
SELECT * FROM utilisateurs WHERE age > 25;
```

<table data-header-hidden><thead><tr><th>id</th><th width="149">nom</th><th>age</th><th>payss</th><th>actif</th></tr></thead><tbody><tr><td>1</td><td>Alice</td><td>30</td><td>France</td><td>1</td></tr><tr><td>3</td><td>Clara</td><td>35</td><td>France</td><td>1</td></tr><tr><td>4</td><td>David</td><td>28</td><td>Canada</td><td>1</td></tr></tbody></table>

Exemple : utilisateurs français

```sql
SELECT nom FROM utilisateurs WHERE pays = 'France';
```

| Alice |
| ----- |
| Clara |
| Emma  |

***

## 3. Les opérateurs&#x20;

### 3.1 De comparaison

Voici les opérateurs les plus courants avec WHERE :

| Opérateur | Signification     | Exemple          |
| --------- | ----------------- | ---------------- |
| <> ou !=  | différent         | pays <> 'France' |
| <         | inférieur         | age < 25         |
| >         | supérieur         | age > 25         |
| <=        | inférieur ou égal | age <= 30        |
| >=        | supérieur ou égal | age >= 18        |

***

### 3.2. Les opérateurs logiques

Ils permettent de combiner plusieurs conditions :

* AND : toutes les conditions doivent être vraies
* OR : au moins une condition doit être vraie
* NOT : inverse la condition

Exemple : utilisateurs actifs en France

```sql
SELECT * FROM utilisateurs WHERE pays = 'France' AND actif = 1;
```

| 1 | Alice | 30 | France | 1 |
| - | ----- | -- | ------ | - |
| 3 | Clara | 35 | France | 1 |

Exemple : utilisateurs belges ou canadiens

```sql
SELECT * FROM utilisateurs WHERE pays = 'Belgique' OR pays = 'Canada';
```

| 2 | Bob   | 22 | Belgique | 0 |
| - | ----- | -- | -------- | - |
| 4 | David | 28 | Canada   | 1 |

Exemple : utilisateurs non actifs

```sql
SELECT * FROM utilisateurs WHERE NOT actif = 1;
```

| 2 | Bob  | 22 | Belgique | 0 |
| - | ---- | -- | -------- | - |
| 5 | Emma | 19 | France   | 0 |

***

## 4. Autres filtres utiles

### BETWEEN

filtre une valeur entre deux bornes incluses

```sql
SELECT * FROM utilisateurs WHERE age BETWEEN 20 AND 30;
```

| 1 | Alice | 30 | France   | 1 |
| - | ----- | -- | -------- | - |
| 2 | Bob   | 22 | Belgique | 0 |
| 4 | David | 28 | Canada   | 1 |

### IN

correspond à une liste de valeurs

```sql
SELECT * FROM utilisateurs WHERE pays IN ('France', 'Belgique');
```

| 1 | Alice | 30 | France   | 1 |
| - | ----- | -- | -------- | - |
| 2 | Bob   | 22 | Belgique | 0 |
| 3 | Clara | 35 | France   | 1 |
| 5 | Emma  | 19 | France   | 0 |

### LIKE

#### &#x20;: recherche avec motif (texte)

* % représente plusieurs caractères
* \_ représente un seul caractère

Exemple : noms commençant par “A”

```sql
SELECT * FROM utilisateurs WHERE nom LIKE 'A%';
```

| 1 | Alice | 30 | France | 1 |
| - | ----- | -- | ------ | - |

***

## 5. Trier les résultats avec ORDER BY

Par défaut, les résultats ne sont pas triés. Utilise ORDER BY pour les ordonner.

Exemple : trier par âge croissant

```sql
SELECT * FROM utilisateurs ORDER BY age ASC;
```

| 5 | Emma  | 19 | France   | 0 |
| - | ----- | -- | -------- | - |
| 2 | Bob   | 22 | Belgique | 0 |
| 4 | David | 28 | Canada   | 1 |
| 1 | Alice | 30 | France   | 1 |
| 3 | Clara | 35 | France   | 1 |

Exemple : trier par nom décroissant

```sql
SELECT * FROM utilisateurs ORDER BY nom DESC;
```

| 5 | Emma  | 19 | France   | 0 |
| - | ----- | -- | -------- | - |
| 4 | David | 28 | Canada   | 1 |
| 3 | Clara | 35 | France   | 1 |
| 2 | Bob   | 22 | Belgique | 0 |
| 1 | Alice | 30 | France   | 1 |

***

## 6. Limiter le nombre de résultats

Avec LIMIT, tu peux restreindre le nombre de lignes retournées.

Exemple : premiers utilisateurs

```sql
SELECT * FROM utilisateurs LIMIT 3
```

| 1 | Alice | 30 | France   | 1 |
| - | ----- | -- | -------- | - |
| 2 | Bob   | 22 | Belgique | 0 |
| 3 | Clara | 35 | France   | 1 |

***

## Conclusion

La commande SELECT est la base de toute interrogation SQL.

Avec WHERE, les opérateurs de comparaison et logiques, tu peux déjà faire de puissantes recherches dans tes tables, sans avoir besoin de JOIN ni de structure complexe.
