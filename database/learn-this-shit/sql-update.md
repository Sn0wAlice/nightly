# SQL - Update

La commande UPDATE permet de modifier des valeurs existantes dans une table.

#### Syntaxe :

```sql
UPDATE nom_de_table
SET colonne1 = nouvelle_valeur1, colonne2 = nouvelle_valeur2
WHERE condition;
```

***

#### Table de départ : utilisateurs

| id | nom   | age | pays     | actif |
| -- | ----- | --- | -------- | ----- |
| 1  | Alice | 30  | France   | 1     |
| 2  | Bob   | 22  | Belgique | 0     |
| 3  | Clara | 35  | France   | 1     |
| 4  | David | 28  | Canada   | 1     |
| 5  | Emma  | 19  | France   | 0     |

***

#### Exemple 1 : activer un utilisateur inactif

```sql
UPDATE utilisateurs SET actif = 1 WHERE id = 2;
```

Résultat (ligne modifiée uniquement) :

| id | nom | age | pays     | actif |
| -- | --- | --- | -------- | ----- |
| 2  | Bob | 22  | Belgique | 1     |

***

#### Exemple 2 : changer le pays de David

```sql
UPDATE utilisateurs SET pays = 'France' WHERE nom = 'David';
```

Résultat (ligne modifiée uniquement) :

| id | nom   | age | pays   | actif |
| -- | ----- | --- | ------ | ----- |
| 4  | David | 28  | France | 1     |

***

#### Exemple 3 : augmenter l’âge de tous les utilisateurs de 1 an

```sql
UPDATE utilisateurs SET age = age + 1;
```

Résultat (toutes les lignes) :

| id | nom   | age | pays     | actif |
| -- | ----- | --- | -------- | ----- |
| 1  | Alice | 31  | France   | 1     |
| 2  | Bob   | 23  | Belgique | 1     |
| 3  | Clara | 36  | France   | 1     |
| 4  | David | 29  | France   | 1     |
| 5  | Emma  | 20  | France   | 0     |

***

#### Exemple 4 : désactiver tous les utilisateurs de moins de 25 ans

```sql
UPDATE utilisateurs SET actif = 0 WHERE age < 25;
```

Résultat (lignes concernées) :

| id | nom  | age | pays     | actif |
| -- | ---- | --- | -------- | ----- |
| 2  | Bob  | 23  | Belgique | 0     |
| 5  | Emma | 20  | France   | 0     |
