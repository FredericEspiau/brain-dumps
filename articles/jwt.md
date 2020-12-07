# JWT

Peut-on se fier uniquement au JWT pour gérer les autorisations

Oui si les autorisations sont basées sur des groupes
Si on a une logique applicative poussée, il faut récupérer l'identité dans le JWT et ensuite exécuter la logique

L'autorisation est à gérer par le framework (middleware) quand c'est possible, du coup au niveau controller ou au global

La logique auth et auth ne devrait pas polluer le use-case

Ne pas gérer les autorisations dans le token en revanche permet de modifier les ACL à la volée, sans avoir à attendre que l'user se reconnecte ou à regénérer son token

Durée de vie courte pour un JWT: 1 heure

Sources:
- https://stackoverflow.com/questions/47224931/is-setting-roles-in-jwt-a-best-practice/53527119#53527119
- https://hasura.io/blog/best-practices-of-using-jwt-with-graphql/#silent_refresh
