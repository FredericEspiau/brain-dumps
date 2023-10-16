# Domain

- <https://medium.com/nick-tune-tech-strategy-blog/what-is-a-domain-99f658b22d7d>

A Domain, in the broad sense, is what an organization does and the world it does it in. Businesses identify a market and sell products and services. Each kind of organization has its own unique realm of know-how and way of doing things. That realm of understanding and its methods for carrying out its operations is its Domain. When you develop software for an organization, you are working in its Domain. It should be pretty obvious to you what your Domain is. You work in it.

One thing to be aware of is that the term Domain may be a bit overloaded. Domain can refer to both the entire domain of the business, as well as just one core or supporting area of it. I will do my best to distinguish each use of the term. When referring to just one area of the business, I will generally qualify it with the use of Core Domain, Subdomain, and the like.

## Sector, Industry, Field

I like StoneyB’s description on Stack Exchange of sector, industry, and field as being different types of domain. We can learn a few things from these examples.

- Field is most often used to designate a domain of professional specialization.

- Industry, which designates a domain of commercial endeavour.

- Sector is similar to industry but is usually employed to distinguish domains on a structural rather than input/output basis.

Firstly, there are types of domain which add constraints on how items are grouped. For example, an industry is a domain where domain concepts are grouped based on commercial endeavour like the ride sharing industry.

Secondly, there are more specific words for describing different types of domains. To reduce undesired ambiguity in communication consider using the specific definitions.

Thirdly, domains are not inherently mutually exclusive. The same concept could appear in multiple domains like a sector, an industry, and a field.

## Levels of Scale

The word domain can also be problematic when the level of scale is not easy to implicitly determine from the context. I encounter this problem quite frequently when the word domain is used in the context of business and software architecture.

A system is composed of parts which can be grouped into areas, and those areas can be grouped into bigger areas, and those bigger areas can be grouped into even bigger areas. Domain can apply to any of those levels.

I recommend using terminology to make levels of scale explicitly, like the levels of scale terminology from sociology, when discussing architecture: micro, meso, and macro levels of scale.

- Micro Domain — an area of expertise or knowledge which can be owned by a single team.

- Meso Domain — an area encompassing multiple micro domains owned by a small community of teams working towards similar goals, probably with an overlap in domain concepts.

- Macro Domain — an area encompassing multiple meso domains, a very high-level view of the system.

## Business Domains in the World of Software/Product Development

In software development, we typically deal with business domains. Areas of expertise in which the business develops tools (aka products) to support people who have a purpose (e.g. users and customers).

[Illustration](domain.png)

---

<https://medium.com/nick-tune-tech-strategy-blog/domains-subdomain-problem-solution-space-in-ddd-clearly-defined-e0b49c7b586c>

# Domains

Domain-Driven Design sticks closely to the Cambridge Dictionary definition of a domain:

an area of interest or an area over which a person has control:

She treated the business as her private domain.

These documents are in the public domain (= available to everybody).

This definition of a domain is very fuzzy. What is an area of interest? It can be anything. A domain is effectively an arbitrary boundary around some subset of concepts in the universe.

Domains are subjective and they are not mutually exclusive. The same concepts can exist in many different domains. Here’s an example I use in talks and workshops:

[How to group](./how-to-group.jpeg)

If the coloured shapes in the image above represent concepts, how would they be grouped into domains? As you can guess, there are a number of ways to do this.

We can group the square shapes into the Squares domain, and the circles in the Circles domain. But the blue square and the blue circle could also belong to the Blue domain.

[Different grouping](./diffrent-grouping.jpeg)

When modelling systems we have to choose the most appropriate domain boundaries with which to align our software and organisational boundaries. Even if we align we align by ‘colour’ the shape domain is still a domain that exists.

Every domain I model and every modelling workshop I run, different people like to slice up systems across different domain boundaries. This is normal, embrace the fuzziness and apply design thinking.

## Subdomains

What’s the difference between a domain and a subdomain? This one is easy — subdomain is not a word that exists in the dictionary. The word subdomain is used prominently in the world of web hosting, but what does it mean in DDD?

In DDD, a subdomain is a relative term. Domain and subdomain can be used interchangeably. When we use the word subdomain, we are emphasising that the domain we are talking about is a child of another higher-level domain which we have identified.

Every subdomain is, therefore, a domain, and most domains are a subdomain. The only time I wouldn’t say a domain is also a subdomain is when our model does not contain a higher-level parent domain.

## Core, Generic, Supporting (Sub)Domains

People are often confused when they hear that a core domain is actually a subdomain. In his DDD books, Eric Evans refers to them as Core Domains, but he also refers to them as subdomains. Confusing much?

When you view domains and subdomains as fuzzy, and subdomains also as domains, using core domain and core subdomain interchangeably doesn’t really matter. It’s fuzzy but not ambiguous.

Core Domain sounds better, Core Subdomain emphasises that there is a higher-level domain to which this belongs.

## Subdomains vs Bounded Contexts

Perhaps the most confusing and controversial topic of all is subdomains vs bounded contexts. Two types of boundaries.

Some people suggest that subdomains are part of the problem space an chosen by “The Business” whereas bounded contexts are software boundaries defined engineers.

To me this has a very antiquated feel about it. And it makes sense. DDD was published 20 years ago, at a time when there was a split between the business and the separate IT department. There was a big gap between business boundaries and software boundaries.

But in modern, product-led organizations, this no longer makes sense. IT is embedded in “the business”. There is no separate IT team. Empowered engineering teams own the products they are building, and they shape their own team topologies. The business, organizational, domain, and software boundaries are all defined by the same people.

## Problem Space vs Solution Space: Proposing a Better Model For DDD

In Wardley’s Strategy Cycle, there are the following elements (with my simplified definitions):

- Purpose: what is the problem being solved / goal to be achieved in our domain(s) of interest?

- Landscape: what is the current state of the domain(s) we are interested in

- Climate: what forces are acting on the domain(s) and how are they likely to evolve

- Doctrine: universally good practices we should apply

- Leadership: what is our solution… what changes are we going to make in existing and new domain(s)

## Are Domains/Subdomains Problem or Solution Space?

This question can’t really be answered unless we have a clear definition of problem or solution space. But I’ll have a go anyway.

Do you really know what problem space and subdomain are or do you just know the name?

User needs and problems exist in a (sub)domain(s), the current state of the world has (sub)domains, the solution will involve multiple (sub)domains and it will alter the state of the world (which has domains). Therefore, (sub)domains logically exist in all spaces.

How can a subdomain only exist in the problem space when the design determines which subdomains we need to build solutions in? Ergo, some domains are only relevant to the solution and not the problem.

[Space example](./space-example.jpeg)

New solutions create new problems, or in the words of Simon Wardley Higher Order Systems Create New Sources of Worth.

I still recommend avoid using problem/space and instead be more specific about what you actually mean: purpose, landscape, climate, doctrine, leadership, or something else.

Whenever using the terms problem space and solution space, you need to clarify from which perspective you are speaking. Your problem space is someone else’s solution space. It’s your view of the domain.

## Domains are Hierarchical

If a domain can contain subdomains, and a subdomain is a domain… then a subdomain can contain more fine-grained subdomains. Domains and subdomains are a hierarchical concept.

When designing socio-technical systems, we often want to show domains at different levels. Leadership of an organisation might want to see the companies 7 top-level domains. Software architects might want to see the domain boundaries for 100 microservices.

The Enterprise Architecture world uses the concept of Business Capabilities at different levels. Business Capabilities can be viewed as domains and subdomains.

[Domains are hierarchical](./domains-are-hierarchical.jpeg)

## Subdomain vs Bounded Context

This is one of the most confusing things about DDD, but when you have a clear definition of subdomain it’s actually the simplest to explain.

I’ve already established that a (sub)domain is a non-mutually-exclusive, arbitrary subset of concepts in the universe. A bounded context is the boundary of a model that represents those concepts, their relationships, and their rules. The same subdomain could be represented by an infinite number of modelling choices.

A model in DDD can be represented in a variety of formats such as post-it notes or code. Anything that shows domain concepts, relationships, rules, and so on.

Since a bounded context is a boundary for a model, it could include concepts from multiple subdomains. Or a single subdomain could be modelled as multiple bounded contexts.

[Subdomain vs bounded](./subdomain-vs-bounded-context.jpeg)

---

# Domain modeling

- [Functional and Algebraic Domain Modeling](https://www.youtube.com/watch?v=BskNvfNjU_8&feature=youtu.be)

# Functional and Reactive Domain Modeling

## Introduction

Domain model: representation de comment le métier fonctionne

Pour un système de banque, on a des entités comme Banque, Compte, Currency, Transaction et Report

Responsabilité du modèle: s'assurer que les users ont une bonne experience quand ils utilisent le système

Implémenter un modèle de domaine -> traduire processus métier en logiciel

### What is a domain model ?

When was the last time you withdrew cash from an ATM? Or deposited cash into your bank account? Or used internet banking to check whether your monthly pay has been credited to your checking account? Or asked for a portfolio statement from your bank? All these italicized terms relate to the business of personal banking. We call this the domainof personal banking.

Domain -> secteur d'intérêt de l'entreprise

Modèle du domaine:

- abstractions conçues
- comportements implémentées
- intéractions UI

Plus formellement, un domaine modèle -> relation entre les entitées du domaine problème et autres détails tels que

- objets de ce domaine

Banque, Compte, Transaction

- comportement qu'on ces objets pour interargir

On peut faire un débit sur un compte, on émet un reçu pour le client

- langage du domaine

- contexte dans lequel le modèle opère

Gros challenge: gérer la complexité du domaine

Complexités essentielles -> inhérentes au problème et ne peuvent être évitées
