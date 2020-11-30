
# Les repos

Salut tout le monde. J'ai quelques questions à propos des repository.
Passez vous les objets du domaine au repsitory ?
Est ce que  l'usage d'un ORM est autorisé pour l'implementation d'un repository ?
Quels sont les objets que vous avez en retour d'un repository ?

oui
oui
oui

l’implem du repo appartient au monde des adapters

donc a le couplage sur le domaine

---

Passer les objets du domaine dans le repo, cad t'as pas de DTO ?

---

y’a pas de DTO pour ça

---

Je suis perdu d'un coup là, peut être c'est différent en DDD que l'utilisation classique quon vois d'un repo non ?

---

En DDD, un repo prend des objets du domaine et retourne aussi l'objet du domaine. Après dans la vraie vie, tu vas voir des repos qui vont retourner des DTOs directement. (edited) 

---

Eu coup côté persistance si c'est du SQL tu auras plusieurs table avec leurs relation qui sont tous dirigé de l'aggregate Root aux entités et VOs qu'il contient sinon avec une NOSQL, tu auras un document qui est créé à partir de cette Aggregate Root ? (edited) 

---

Oui c'est çà. Et pour le NOSQL c'est quand tu as une base documents que ça va te créer un document.

---

Oui je pensais à mongo en faite

---

Si tu as un Data Model, ton repo va mapper de ton domaine vers to  data model.

---

Yes, mais du coup le choix d'avoir un data model ça serais purement technique, puisque lorsqu'on voudra modifier une entité il faut embarquer tout l'aggregate ?

---

Pas obliger encore une fois. Il y'a plusieurs manières de faire. Y'en a qui vont utiliser du Lazy Loading et ne pas charger tout l'agregate mais juste ce qu'ils ont besoin pour la mise à jour.

---

en terme d'organisation il faudrait donc mettre le repository (du moins l'interface) dans le domaine ?

---

L'interface c'est ton port donc dans le domaine oui


# Auth
Salut, j'aimerai avoir vos avis et votre point de vue sur l'organisation de plusieurs modules entre eux. J'ai un module authentication, qui a deux features : login et register.
Ces deux features interagissent avec un utilisateur, cependant, je souhaiterai connaître les bonnes pratiques pour organiser  correctement  mon projet. Globalement, comment organisez-vous votre projet lorsque vous avez des éléments qui vont être utilisés dans plusieurs features ? Un dossier common par exemple ?
La plupart des exemples pour l'authentification contiennent un module User (database entity, implémentation du repository, controller, etc)  et un module Auth  (edited) 

---

c’est un bounded context à part entière
