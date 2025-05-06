# STRING Deep Search

Lors de la mise en place d’un moteur de recherche interne basé uniquement sur **ClickHouse**, une problématique centrale réside dans la capacité à gérer à la fois:

* les recherches de phrases exactes (ex. : "aimer le chocolat")&#x20;
* les recherches de mots-clés dispersés (ex. : aimer AND le AND chocolat)
* manque de possibilité claire de comparateurs logique (OR, AND, !=, ...)

Y compris sur des documents volumineux pouvant contenir plusieurs centaines de pages. Or, l’indexation classique sur le contenu brut (content) ne permet pas de capturer efficacement la présence de mots clés répartis sur plusieurs sections d’un même document. Il faut donc cook une nouvelle solution

### 1. Les dépendances.

Notre problème, c'est qu'on est dépendant de trop de facteurs, surtout avec la taille du texte variable et souvent pouvant être conséquente. Dans un premier temps, il faut donc couper le projet en plusieurs parties (table du coup sûr Clickhouse hein).

* La partie de texte "simple" avec les courtes informations, mais globale des documents
* La partie avec des "chunks", tel que tous les chunks soit des parts normés et semi égale de portion de texte.
* La partie token où on va jouer avec les mots mêmes du texte dans des tableaux.

Ce qui nous donne les tables suivantes

{% tabs %}
{% tab title="documents" %}
<table><thead><tr><th>doc_id</th><th>title</th><th width="98">....</th><th>url</th></tr></thead><tbody><tr><td>jnbhghhze</td><td>L'amour du chocolats</td><td></td><td>https://...</td></tr><tr><td>ugyiazjdkf</td><td>je sais pas</td><td></td><td>https://...</td></tr><tr><td>zefhiuoje</td><td>un nom de livre</td><td></td><td>https://...</td></tr></tbody></table>
{% endtab %}

{% tab title="doc_chunks" %}
<table><thead><tr><th width="124">doc_id</th><th width="165">chunk_id</th><th>content</th></tr></thead><tbody><tr><td>jnbhghhze</td><td>uuidv4</td><td>J'aime le chocolats</td></tr><tr><td>jnbhghhze</td><td>uuidv4</td><td>je sais pas quoi dire</td></tr><tr><td>jnbhghhze</td><td>uuidv4</td><td>vraiment les exemples nulls</td></tr></tbody></table>
{% endtab %}

{% tab title="doc_tokens" %}
<table><thead><tr><th width="137">doc_id</th><th width="157" data-type="number">token_weight</th><th>tokens</th></tr></thead><tbody><tr><td>jnbhghhze</td><td>2</td><td>["je", "tu", "le"]</td></tr><tr><td>jnbhghhze</td><td>3</td><td>["lui", "les", "bol"]</td></tr><tr><td>jnbhghhze</td><td>4</td><td>["nous", "aime", "foix"]</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

### 2. L'architecture

Cette architecture nous permet de couper le problème en plusieurs parties, car du coup, nous avons quasiment une table dédiée à chaque problème.

Nous avons donc la possibilité SQL de chercher du texte brut

```sql
SELECT DISTINCT doc_id
FROM doc_chunks
WHERE content ILIKE '%aimer le chocolat%';
```

Mais du coup aussi le fait de chercher dans les array de tokens

```sql
-- ligne 1
SELECT doc_id FROM document_tokens_advanced
WHERE token_length = 2 AND hasAll(tokens, ['je', 'il'])

INTERSECT

-- ligne 2
SELECT doc_id FROM document_tokens_advanced
WHERE token_length = 4 AND hasAll(tokens, ['manger'])

INTERSECT

-- ligne 3
SELECT doc_id FROM document_tokens_advanced
WHERE token_length = 6 AND hasAll(tokens, ['chocolat'])
```

### 3. Merge

Bon du coup, on peut merge comme ça

```sql
-- Partie 1 : doc_id des chunks contenant la phrase exacte
SELECT DISTINCT doc_id
FROM doc_chunks
WHERE content ILIKE '%aimer le chocolat%'

INTERSECT

-- Partie 2.1 : doc_id avec tokens de taille 2 ['je', 'il']
SELECT doc_id
FROM document_tokens_advanced
WHERE token_length = 2 AND hasAll(tokens, ['je', 'il'])

INTERSECT

-- Partie 2.2 : tokens de taille 6 ['manger']
SELECT doc_id
FROM document_tokens_advanced
WHERE token_length = 6 AND hasAll(tokens, ['manger'])

INTERSECT

-- Partie 2.3 : tokens de taille 8 ['chocolat']
SELECT doc_id
FROM document_tokens_advanced
WHERE token_length = 8 AND hasAll(tokens, ['chocolat']);
```

### 4. Langage de build

À faire : un truc qui build une requête SQL avec un langage naturel incluant les conditions du type AND, OR, ==, ! = et tout le bordel
