# Agrégat

Pattern du DDD

Groupe d'objets domaines (que ce soient des entities ou des value objects) qui sont traités comme une unité

Les objets de l'agrégat sont liés entre eux par une entité racine, appelée la racine de l'aggrégat

Cette racine est souvent une entity

Toute modification dans l'aggregate devra passer par une méthode la racine

La racine de l'agrégat garantit la cohérence des modifications apportées au sein de l'agrégat en interdisant aux objets externes d'accéder ou de contenir des références à ses constituants

Un aggregate est un bon moyen pour gérer les invariants métiers et les règles de gestion

