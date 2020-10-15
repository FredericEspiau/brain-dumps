# Implementation detail

Un code doit être écrit en considérant deux types de personnes :
- les développeurs (vous et les autres), qui ont besoin de comprendre le code
- les utilisateurs finaux (ceux qui ne ), qui n'ont pas besoin de comprendre le code

Implementation details are things which users of your code will not typically use, see, or even know about.

It's a behavior produced by code which may be relied on by consuming code, though that behavior is not specified by the spec the code is written to. Hence, other implementations of the same spec may not exhibit the same behavior, and will break that consuming code. That's why it's bad to rely on them.

