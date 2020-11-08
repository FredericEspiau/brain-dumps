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
