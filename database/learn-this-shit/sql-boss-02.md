# SQL - Boss 02

> _“Les bases étaient simples. Mais maintenant tu marches dans les couloirs du Nexus._

> _Un lieu où les requêtes s’imbriquent, où les tables se croisent._

> _Prépare ton clavier, héros de la clause.”_

***

### 📘 Base de données du Boss

Table utilisateurs

| id | nom   | pays\_id |
| -- | ----- | -------- |
| 1  | Alice | 1        |
| 2  | Bob   | 2        |
| 3  | Clara | 1        |
| 4  | David | 3        |
| 5  | Emma  | NULL     |

#### Table pays

| id | nom\_pays |
| -- | --------- |
| 1  | France    |
| 2  | Belgique  |
| 3  | Canada    |
| 4  | Suisse    |

#### Table commandes

| id | utilisateur\_id | montant |
| -- | --------------- | ------- |
| 1  | 1               | 120     |
| 2  | 1               | 85      |
| 3  | 2               | 100     |
| 4  | 3               | 130     |
| 5  | 4               | 95      |

***

### ✦ Partie 1 – QCM (1 point par bonne réponse)

Q1. Que renvoie cette requête ?

```sql
SELECT nom FROM utilisateurs WHERE pays_id IN (SELECT id FROM pays WHERE nom_pays = 'France');
```

{% tabs %}
{% tab title="Liste" %}
A) Les utilisateurs vivants en France

B) Tous les pays sauf la France

C) Les utilisateurs ayant passé des commandes en France

D) Erreur, IN ne peut pas être utilisé comme ça
{% endtab %}

{% tab title="Réponse" %}
✅ Réponse : A – Les utilisateurs vivants en France

Explication : La sous-requête retourne 1, et la requête principale sélectionne les utilisateurs avec pays\_id = 1.
{% endtab %}
{% endtabs %}

***

Q2. Quelle est la différence entre LEFT JOIN et INNER JOIN ?

{% tabs %}
{% tab title="Liste" %}
A) LEFT JOIN ignore les correspondances

B) LEFT JOIN inclut les lignes sans correspondance à droite

C) INNER JOIN garde toutes les lignes même sans correspondance

D) Aucune, elles sont identiques
{% endtab %}

{% tab title="Réponse" %}
✅ Réponse : B – LEFT JOIN inclut les lignes sans correspondance à droite

Explication : LEFT JOIN garde _toutes les lignes de gauche_ (utilisateurs), même si aucun pays ne correspond.
{% endtab %}
{% endtabs %}

***

Q3. Quel résultat produit ce code ?

```sql
SELECT AVG(montant)
FROM commandes
WHERE utilisateur_id IN (SELECT id FROM utilisateurs WHERE pays_id = 1);
```

{% tabs %}
{% tab title="Liste" %}
A) Moyenne de toutes les commandes

B) Moyenne des commandes des utilisateurs français

C) Somme des commandes de Clara uniquement

D) Erreur car il manque un JOIN
{% endtab %}

{% tab title="Réponse" %}


✅ Réponse : B – Moyenne des commandes des utilisateurs français

Explication : La sous-requête filtre les utilisateurs ayant pays\_id = 1 → Clara et Alice.

Leurs commandes :

* Alice : 120 + 85 = 205
*   Clara : 130

    Moyenne = (120 + 85 + 130) / 3 = 111.67
{% endtab %}
{% endtabs %}

***

Q4. Avec ce code :

```sql
SELECT u.nom, p.nom_pays
FROM utilisateurs u
LEFT JOIN pays p ON u.pays_id = p.id;
```

{% tabs %}
{% tab title="Liste" %}
A) Tous les utilisateurs sauf ceux sans pays

B) Tous les utilisateurs et leur pays s’ils en ont un

C) Tous les pays même sans utilisateurs

D) Rien du tout car ce n’est pas un FULL JOIN
{% endtab %}

{% tab title="Réponse" %}
✅ Réponse : B – Tous les utilisateurs et leur pays s’ils en ont un

Explication : Tous les utilisateurs sont gardés, et ceux sans pays\_id auront NULL.
{% endtab %}
{% endtabs %}

***

### ✦ Partie 2 – Requêtes à écrire (2 points par bonne réponse)

R1. Affiche le nom des utilisateurs ayant passé au moins une commande (utilise une sous-requête dans IN).

R2. Affiche le nom des utilisateurs et leur pays (même ceux sans pays).

R3. Affiche les pays qui n’ont aucun utilisateur associé (sous-requête obligatoire).

R4. Affiche les noms des utilisateurs ayant dépensé plus de 100€ au total (agrégation + sous-requête avec GROUP BY).

R5. Affiche le nom des utilisateurs et le montant total qu’ils ont dépensé (jointure + GROUP BY).

{% tabs %}
{% tab title="Main" %}
Salut Salut, moi j'aime le sql
{% endtab %}

{% tab title="R1" %}
```sql
SELECT nom FROM utilisateurs
WHERE id IN (SELECT utilisateur_id FROM commandes);
```
{% endtab %}

{% tab title="R2" %}
```sql
SELECT u.nom, p.nom_pays
FROM utilisateurs u
LEFT JOIN pays p ON u.pays_id = p.id;
```
{% endtab %}

{% tab title="R3" %}
```sql
SELECT nom_pays FROM pays
WHERE id NOT IN (SELECT pays_id FROM utilisateurs WHERE pays_id IS NOT NULL);
```
{% endtab %}

{% tab title="R4" %}
```sql
SELECT nom FROM utilisateurs
WHERE id IN (
  SELECT utilisateur_id
  FROM commandes
  GROUP BY utilisateur_id
  HAVING SUM(montant) > 100
);
```
{% endtab %}

{% tab title="R5" %}
```sql
SELECT u.nom, SUM(c.montant) AS total_depense
FROM utilisateurs u
JOIN commandes c ON u.id = c.utilisateur_id
GROUP BY u.nom;
```
{% endtab %}
{% endtabs %}

***

### 🧮 Barème

* QCM : /4
* Requêtes : /10
* Total Boss 02 : /14

> Score 12–14 : Tu es un Architecte Relationnel

> 9–11 : Tu es un Forgeron de Données confirmé

> 6–8 : Apprenti dans l’Ordre des Sous-requêtes

> < 6 : Reviens dans le village des INNER JOIN

