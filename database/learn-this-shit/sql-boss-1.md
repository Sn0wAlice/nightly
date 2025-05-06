# SQL - Boss 1

> _“Tu as étudié. Tu as écrit des requêtes. Tu as combattu les erreurs de syntaxe._

> _Maintenant, l’Oracle t’appelle. Prouve que tu mérites de passer au niveau supérieur.”_

***

### 📘 TABLE DE RÉFÉRENCE :&#x20;

Table : utilisateurs

Avant de commencer, voici la table utilisateurs actuelle, que le Boss garde précieusement :

| id | nom   | age | pays     | actif |
| -- | ----- | --- | -------- | ----- |
| 1  | Alice | 31  | France   | 1     |
| 2  | Bob   | 23  | Belgique | 0     |
| 3  | Clara | 36  | France   | 1     |
| 4  | David | 29  | France   | 1     |
| 5  | Emma  | 20  | France   | 0     |

***

### ✦ Partie 1 – QCM (1 point par bonne réponse)

Réponds sans tricher : une seule réponse correcte par question.

#### Q1. Que fait la requête suivante ?

```sql
SELECT * FROM utilisateurs WHERE age >= 30;
```

{% tabs %}
{% tab title="Liste" %}
A) Elle supprime tous les utilisateurs de plus de 30 ans

B) Elle retourne les utilisateurs dont l’âge est supérieur ou égal à 30

C) Elle modifie les âges et les rend tous égaux à 30

D) Elle retourne les utilisateurs actifs uniquement
{% endtab %}

{% tab title="Réponse" %}
✅ Réponse : B – Elle retourne les utilisateurs dont l’âge est supérieur ou égal à 30

Explication : SELECT + WHERE age >= 30 → lecture filtrée, aucune suppression.
{% endtab %}
{% endtabs %}

***

#### Q2. La requête suivante retourne :

```sql
SELECT nom FROM utilisateurs WHERE pays <> 'France';
```

{% tabs %}
{% tab title="Liste" %}
A) Alice

B) Clara

C) Bob

D) David
{% endtab %}

{% tab title="Réponse" %}
✅ Réponse : C – Bob

Explication : Le seul utilisateur dont pays <> 'France' est Bob.
{% endtab %}
{% endtabs %}

***

#### Q3. Quel opérateur permet de tester plusieurs valeurs d’un champ sans répéter OR ?

{% tabs %}
{% tab title="Liste" %}
A) %LIKE%

B) IN

C) BETWEEN

D) !=
{% endtab %}

{% tab title="Réponse" %}
✅ Réponse : B – IN

Explication : IN ('France', 'Belgique') est équivalent à pays = 'France' OR pays = 'Belgique'
{% endtab %}
{% endtabs %}

***

#### Q4. Que fait la commande suivante ?

```sql
DELETE FROM utilisateurs WHERE age < 25;
```

{% tabs %}
{% tab title="Liste" %}
A) Elle affiche les utilisateurs de moins de 25 ans

B) Elle les met inactifs

C) Elle les supprime

D) Elle change leur pays
{% endtab %}

{% tab title="Réponse" %}
✅ Réponse : C – Elle les supprime

Explication : DELETE + condition = suppression ciblée
{% endtab %}
{% endtabs %}

***

### ✦ Partie 2 – Requêtes à écrire (2 points par bonne requête)

Écris les requêtes exactes à partir des consignes. Pas d’erreur tolérée, tu es face au Boss.

R1. Sélectionne tous les utilisateurs actifs en France.

R2. Modifie l’âge de Bob pour le mettre à 24.

R3. Supprime l’utilisateur qui s’appelle Emma.

R4. Sélectionne uniquement les noms et pays des utilisateurs dont l’âge est strictement inférieur à 30.

R5. Supprime tous les utilisateurs inactifs.

{% tabs %}
{% tab title="Main" %}
Imagine tu fais une érreur..
{% endtab %}

{% tab title="R1" %}
```sql
SELECT * FROM utilisateurs WHERE actif = 1 AND pays = 'France';
```
{% endtab %}

{% tab title="R2" %}
```sql
UPDATE utilisateurs SET age = 24 WHERE nom = 'Bob';
```
{% endtab %}

{% tab title="R3" %}
```sql
DELETE FROM utilisateurs WHERE nom = 'Emma';
```
{% endtab %}

{% tab title="R4" %}
```sql
SELECT nom, pays FROM utilisateurs WHERE age < 30;
```
{% endtab %}

{% tab title="R5" %}
```sql
DELETE FROM utilisateurs WHERE actif = 0;
```
{% endtab %}
{% endtabs %}

***

### 🏁 Barème

* Partie QCM : /4
* Partie Requêtes : /10
* Total : /14

> Score de 10 ou plus : Tu passes au niveau supérieur

> Score entre 7 et 9 : Tu survis, mais tu dois t’entraîner dans le donjon des erreurs SQL

> Score < 7 : Retour au tutoriel du village de la clause WHERE
