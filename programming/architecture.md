# Architecture

- [The Grand Unified Theory of Software Architecture](https://news.ycombinator.com/item?id=24915497)
- [Simple patterns for building complex applications](https://www.cosmicpython.com/)

# CQRS

C'est le client (par exemple l'application front end) qui donne l'identifiant à la commande au moment de créer un aggrégat

Comme la commande ne renvoie pas d'information, le client doit fournir un identifiant pour pouvoir l'utiliser ensuite

## Sources à étudier

Articles

- [Clarified CQRS](https://udidahan.com/2009/12/09/clarified-cqrs/)
- [Creating IDs with CQRS and Event Sourcing in Java and .NET](https://thomasjaeger.wordpress.com/2016/01/09/creating-ids-with-cqrs-and-event-sourcing-in-java-and-net/)
- [CQS versus server generated IDs](https://blog.ploeh.dk/2014/08/11/cqs-versus-server-generated-ids/)
- [Command Handlers](https://buildplease.com/pages/fpc-10/)
- [Answer to the article CQRS Is an Anti-Pattern for DDD](https://sylvainleroy.com/2020/09/24/answer-to-the-article-cqrs-is-an-anti-pattern-for-ddd/)

Code

- [Simple CQRS example](https://github.com/gregoryyoung/m-r)
- [CQRS Front-end](https://github.com/OpenCircleEndy/cqrs-frontend)

Vidéo

- [CQRS Global Introduction](https://www.youtube.com/watch?v=EkEz3pcLdgY)

## Sources étudiées

Tips

- [Why do commands include the entity id when creating entities?](https://github.com/gregoryyoung/m-r/issues/17) - 2020-10-15

# Clean architecture

Articles

- [JavaScript: Using use-case interactors](https://medium.com/@dtinth/clean-javascript-using-use-case-interactors-f3a50c138154)
- [Command Handlers](https://buildplease.com/pages/fpc-10/)
- [Hexagonal Architecture - A guide](https://beyondxscratch.com/2017/08/19/hexagonal-architecture-the-practical-guide-for-a-clean-architecture/)
- [Très bonne explication - Juan Manuel Garrido de Paz](https://jmgarridopaz.github.io/content/hexagonalarchitecture.html)
- [How to implement the Clean Architecture?](http://www.plainionist.net/Implementing-Clean-Architecture/)
- [What is a use case?](http://www.plainionist.net/Implementing-Clean-Architecture-UseCases/)
- [Clean Architecture : Part 1](https://crosp.net/blog/software-architecture/clean-architecture-part-1-databse-vs-domain/)
- [Usecase dependencies](https://stackoverflow.com/questions/40458666/the-clean-architecture-usecase-dependencies)
- [The Domain](https://buildplease.com/pages/fpc-1/)
- [Why you need Use Cases/Interactors](https://proandroiddev.com/why-you-need-use-cases-interactors-142e8a6fe576)
- [Clean Architecture - Et si on passait à côté ?](https://jordanchapuy.com/posts/2020/11/clean-architecture-et-si-on-passait-a-cote/)
- [The "right" boundary](https://twitter.com/JuanMGarridoPaz/status/1332420424257449985)
- [use case driven](http://tpierrain.blogspot.com/)
- [Layers, Onions, Ports, Adapters: it's all the same](https://blog.ploeh.dk/2013/12/03/layers-onions-ports-adapters-its-all-the-same/)
- [Comparison of Domain-Driven Design and Clean Architecture Concepts](https://khalilstemmler.com/articles/software-design-architecture/domain-driven-design-vs-clean-architecture/)

Code

- [Matthew Renze](https://github.com/matthewrenze)
- [C#](https://github.com/matthewrenze/clean-architecture-demo/tree/repo-and-uow)
- [Wealcome](https://github.com/mica16/wealcome-restaurants-tdd-clean-archi-demo)
- [TDD & Hexagonal](https://github.com/PCreations/real-world-tdd/tree/hexagonal)
- [Haskell](https://github.com/thma/PolysemyCleanArchitecture)
- [NestJs](https://github.com/damienbeaufils/nestjs-clean-architecture-demo)
- [BDD, ATTD and Clean Architecture](https://gitlab.com/bbohec/atdd-clean-architecture-casino/-/tree/master)
- [Hexagonal Architecture + DDD + CQRS in PHP](https://github.com/CodelyTV/php-ddd-example)
- [TypeScript DDD & Hexa](https://github.com/CodelyTV/typescript-ddd-skeleton)
- [React](https://github.com/dimitridumont/hexagonal-architecture-react)
- [React](https://github.com/eduardomoroni/react-clean-architecture)
- [Explicit architecture - PHP](https://github.com/hgraca/explicit-architecture-php)
- [White label](https://github.com/stemmlerjs/white-label)

Vidéo

- [Je vous montre ce que j'apprends #01 : Architecture Hexagonale](https://www.youtube.com/watch?v=_jR8eUlNqK0)

# Autre

- [Fullstack React GraphQL TypeScript Tutorial](https://www.youtube.com/watch?v=I6ypD7qv3Z8&feature=youtu.be)
- [Architecture Playbook](https://nocomplexity.com/documents/arplaybook/introduction.html)
- [The Software Architecture Chronicles](https://herbertograca.com/2017/07/03/the-software-architecture-chronicles/)

Histoire de l'architecture logicielle

- [Systems design for advanced beginners](https://news.ycombinator.com/item?id=23904000)
- [ORMless; a Memento-like pattern for object persistence](https://matthiasnoback.nl/2018/03/ormless-a-memento-like-pattern-for-object-persistence/)
