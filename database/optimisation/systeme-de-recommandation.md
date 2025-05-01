# Système de recommandation

Ce tutoriel vous explique comment construire un système de recommandation basé sur le comportement utilisateur, 100 % en SQL dans ClickHouse, avec support Big Data via `MATERIALIZED VIEW`. ([https://clickhouse.com/docs/materialized-view/incremental-materialized-view](https://clickhouse.com/docs/materialized-view/incremental-materialized-view))

### 📌 Objectif

Pour chaque contenu vu (`content_uuid`), on veut recommander les contenus les plus souvent vus **juste après** par d'autres utilisateurs, dans une **fenêtre glissante** (par exemple, les 10 vues suivantes).

### 📦 Étape 1 — Création de la table source

```sql
DROP TABLE IF EXISTS views;

CREATE TABLE views (
    id UInt64,
    user_uuid String,
    content_uuid String
) ENGINE = MergeTree()
ORDER BY (user_uuid, id);
```

Cette table stocke les parcours des utilisateurs, chaque ligne représentant une vue.

* **id**: ordre chronologique (auto-incrément)
* **user\_uuid**: identifiant utilisateur
* **content\_uuid**: contenu consulté

### 🧪 Étape 2 — Insertion de données de test

```sql
INSERT INTO views (id, user_uuid, content_uuid) VALUES
    (1, 'user1', 'A'),
    (2, 'user1', 'B'),
    (3, 'user1', 'C'),
    (4, 'user1', 'D'),
    (5, 'user1', 'E'),
    (6, 'user2', 'A'),
    (7, 'user2', 'C'),
    (8, 'user2', 'E'),
    (9, 'user2', 'F'),
    (10, 'user2', 'G'),
    (11, 'user3', 'B'),
    (12, 'user3', 'C'),
    (13, 'user3', 'D'),
    (14, 'user3', 'A'),
    (15, 'user3', 'E');
```

### ⚙️ Étape 3 — Création de la MATERIALIZED VIEW

```sql
DROP TABLE IF EXISTS transitions_mv;

CREATE MATERIALIZED VIEW transitions_mv
ENGINE = SummingMergeTree()
ORDER BY (from_content, to_content)
POPULATE
AS
SELECT
    a.content_uuid AS from_content,
    b.content_uuid AS to_content,
    count() AS transition_count
FROM views AS a
JOIN views AS b
    ON a.user_uuid = b.user_uuid
   AND b.id > a.id
   AND b.id <= a.id + 10  -- 10 c'est une valeur brute, stv 20 faut update ça
GROUP BY from_content, to_content;
```

✅ Le mot-clé _POPULATE_ permet de remplir la vue avec les données déjà existantes.

* **from\_content**: le contenu initial
* **to\_content**: contenu vu ensuite dans la fenêtre
* **transition\_count**: nombre de fois où cette transition a été observée

### 🔍 Étape 4 — Interroger la vue

Requête pour récupérer les 20 meilleurs contenus à recommander après avoir vu 'A'&#x20;

```sql
SELECT
    to_content,
    transition_count
FROM transitions_mv
WHERE from_content = 'A'
ORDER BY transition_count DESC
LIMIT 20;
```

#### Calcul de probabilité (bonus)

Si tu veux un score normalisé (probabilité que **to\_content** suive **from\_content**) :

```sql
SELECT
    from_content,
    to_content,
    transition_count,
    round(transition_count / sum(transition_count) OVER (PARTITION BY from_content), 3) AS probability
FROM transitions_mv
WHERE from_content = 'A'
ORDER BY probability DESC
LIMIT 20;
```
