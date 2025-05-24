# Crypto API

L'API **Krypto** permet d'interroger des adresses de portefeuilles blockchain afin d'obtenir des informations sur les transactions associées.&#x20;

{% hint style="info" %}
Clef d'api on demand, envoie moi un email pour en savoir plus
{% endhint %}

Elle prend en charge plusieurs types de requêtes, notamment :

* Vérification de la validité d'une clé API.
* Récupération d'informations liées à une adresse de portefeuille.
* Exploration des transactions (envoyées, reçues ou toutes) avec des paramètres avancés tels que la profondeur de traçage (\`_depth_\`) et la valeur minimale (\`_min_\`).

Toutes les requêtes nécessitent une clé API à fournir dans l'en-tête **Authorization**.

{% hint style="warning" %}
⚠️ Les paramètres comme \`depth\` et \`min\` sont soumis à des restrictions pour des raisons de performance et de sécurité.
{% endhint %}

La doc de l'api:

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}
