# CQRS

C'est le client (par exemple l'application front end) qui donne l'identifiant à la commande au moment de créer un aggrégat

Comme la commande ne renvoie pas d'information, le client doit fournir un identifiant pour pouvoir l'utiliser ensuite

## Sources à étudier

Articles

- [Clarified CQRS](https://udidahan.com/2009/12/09/clarified-cqrs/)
- [Creating IDs with CQRS and Event Sourcing in Java and .NET](https://thomasjaeger.wordpress.com/2016/01/09/creating-ids-with-cqrs-and-event-sourcing-in-java-and-net/)
- [CQS versus server generated IDs](https://blog.ploeh.dk/2014/08/11/cqs-versus-server-generated-ids/)

Code

- [Simple CQRS example](https://github.com/gregoryyoung/m-r)
- [CQRS Front-end](https://github.com/OpenCircleEndy/cqrs-frontend)

Vidéo

- [CQRS Global Introduction](https://www.youtube.com/watch?v=EkEz3pcLdgY)

## Sources étudiées

Tips

- [Why do commands include the entity id when creating entities?](https://github.com/gregoryyoung/m-r/issues/17) - 2020-10-15
