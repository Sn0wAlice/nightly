# SQL - Delete

La commande DELETE permet de supprimer une ou plusieurs lignes d’une table.

Syntaxe :

```sql
DELETE FROM nom_de_table WHERE condition;
```

{% hint style="danger" %}
Sans condition, _toutes les lignes sont supprimées_ (à éviter sans précaution).
{% endhint %}

***

#### Table de départ (après les UPDATE précédents)

| id | nom   | age | pays     | actif |
| -- | ----- | --- | -------- | ----- |
| 1  | Alice | 31  | France   | 1     |
| 2  | Bob   | 23  | Belgique | 0     |
| 3  | Clara | 36  | France   | 1     |
| 4  | David | 29  | France   | 1     |
| 5  | Emma  | 20  | France   | 0     |

***

#### Exemple 1 : supprimer un utilisateur par son id

```sql
DELETE FROM utilisateurs WHERE id = 5;
```

Table après suppression :

| id | nom   | age | pays     | actif |
| -- | ----- | --- | -------- | ----- |
| 1  | Alice | 31  | France   | 1     |
| 2  | Bob   | 23  | Belgique | 0     |
| 3  | Clara | 36  | France   | 1     |
| 4  | David | 29  | France   | 1     |

***

#### Exemple 2 : supprimer tous les utilisateurs inactifs

```sql
DELETE FROM utilisateurs WHERE actif = 0;
```

Table après suppression :

| id | nom   | age | pays   | actif |
| -- | ----- | --- | ------ | ----- |
| 1  | Alice | 31  | France | 1     |
| 3  | Clara | 36  | France | 1     |
| 4  | David | 29  | France | 1     |

***

#### Exemple 3 : supprimer les utilisateurs de plus de 35 ans

```sql
DELETE FROM utilisateurs WHERE age > 35;
```

Table après suppression :

| id | nom   | age | pays   | actif |
| -- | ----- | --- | ------ | ----- |
| 1  | Alice | 31  | France | 1     |
| 4  | David | 29  | France | 1     |

***

#### Exemple 4 : supprimer tous les enregistrements (attention)

```sql
DELETE FROM utilisateurs;
```

Table après suppression :

_(vide)_

{% hint style="success" %}
C'est pour les mêmes sur twitter, delete la table user azy, je te regarde
{% endhint %}
