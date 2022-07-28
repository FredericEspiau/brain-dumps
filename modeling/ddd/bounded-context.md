# Bounded context

## Definition

A description of a boundary (typically a subsystem, or the work of a particular team) within which a particular model is defined and applicable

This is an odd definition. The adjective "bounded" gives the impression it is a kind of context: a bounded one, as opposed to an unbounded one. This "definition" however states a bounded context is a description: a description of a boundary. I'd not call that a "bounded context", but a "context boundary". The context was mainly called "bounded" to emphasise that it has boundaries.

## A context is a model

When you describe (selected aspects of) a domain, you are modeling. The choice of what aspects you select is part of the modeling process. To describe a domain you need a language. Using language is modeling: the words you use are always an interpretation of what you describe. That language, used in modeling, is what is called the "ubiquitous language" in DDD. It is the language of the model, an intrinsic part of the model.

If you have a domain and a model of that domain, then what is that context in which the model is applicable? It cannot be the model itself, for that would be a circular definition. So it must be something wider than the model for which it is a context. I always saw a "bounded context" as a part of the domain. But when you are describing that context and its boundaries, you are describing the domain... and so you are modeling. You cannot describe the domain without modeling. The context is a model too. The boundaries you describe can be boundaries that exist in the domain, but even if that is the case, the boundaries of a context are always an interpretation, a design choice: you describe the boundaries of a model.

## Occam's razor

So we have a (domain) model and an outer model, called the (bounded) context. That outer model is used to define the boundaries of the inner model. Let's apply Occam's razor to clean this up a bit: if I define the boundaries of my domain model, I don't need a (bounded) context. I have a model and that model has boundaries. That's it. I can clearly communicate that to all stakeholders using the model. There is no advantage in using a  "bounded context" besides the domain model.

# Done
- https://hermanpeeren.nl/varia/why-i-dont-need-a-bounded-context