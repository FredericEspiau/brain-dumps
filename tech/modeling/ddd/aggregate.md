# Agrégat

Pattern du DDD

La structure d'un modèle du domaine est composée d'entities et de value objects qui représentent des concepts dans le problem domaine

Gérer l'interaction entre objets du domaine est la grosse source de complexité

# Définition

Groupe d'objets domaines (que ce soient des entities ou des value objects) qui sont traités comme une unité

Tous ces objets sont liés par des invariants

Les objets de l'agrégat sont liés entre eux par une entité racine, appelée la racine de l'aggrégat

Cette racine est souvent une entity

Toute modification dans l'aggregate devra passer par une méthode la racine

La racine de l'agrégat garantit la cohérence des modifications apportées au sein de l'agrégat en interdisant aux objets externes d'accéder ou de contenir des références à ses constituants

Un aggregate est un bon moyen pour gérer les invariants métiers et les règles de gestion

Un aggrégat est l'élément de base du transfert de stockage des données, une transaction ne doit pas franchir les limites des aggregates

Exemple:

- une commande et les articles qui la composent

# Points clés à retenir

- Les agrégats devraient être basés sur des invariants de domaine.
- Les agrégats doivent être modifiés de manière à ce que leurs invariants soient parfaitement cohérents au sein d'une même transaction
- Qualifier les associations en ajoutant des contraintes pour réduire la complexité technique
- Faire en sorte d'avoir des agrégats plus petits afin de réduire le verrouillage des transactions et les complexités liées à la cohérence


# Sources

- https://martinfowler.com/bliki/DDD_Aggregate.html
- https://blog.engineering.publicissapient.fr/2018/06/25/craft-les-patterns-tactiques-du-ddd/
- https://dzone.com/articles/domain-driven-design-aggregate

# A traiter

- https://khalilstemmler.com/articles/typescript-domain-driven-design/aggregate-design-persistence/
- https://khalilstemmler.com/articles/typescript-domain-driven-design/updating-aggregates-in-domain-driven-design/
- https://www.youtube.com/watch?v=RHg53wMflCc
- https://www.youtube.com/watch?v=Xf_aLAK1RfE
- https://hermanpeeren.nl/varia/a-better-word-for-aggregate-root