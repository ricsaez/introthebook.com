# Notes

Insights and open questions the book has not reached yet. When the book says something, or a question is settled, it leaves this file.

## What we are doing

The book introduces a primitive medium of expression: an irreducible set of ideas through which every idea that can be described as data and steps can be thought and said. To know something we must be able to say it, so the medium comes first. It must be minimal but complete: it can say anything that can be computed (power), and anything simple to think is simple to say (ease).

It is built in layers, each the previous one plus exactly what that one cannot do: the base (functions alone: power), the core (structure: kinds and shapes, so what is said can be checked), the extensions (ease: nothing simple is awkward), and later claims and proofs (saying what is true of values). Each layer is closed as a shape in the Note Book and explained in the Book.

## Why this matters

To know something, we have to be able to say it. So expression comes before knowledge, and a language comes before expression. This could open the introduction.

## Carrying out, seen from outside

- An expression that is not yet a value is like a question, and the value it comes to is like the answer: "the 1000th prime" and `7919`. They are about the same thing, but only the answer lets us see it. Computing is getting from one to the other by steps anyone can follow.
- Every language has different expressions for the same thing, such as "the capital of France" and "Paris". Only some languages have steps inside the language to get from one to the other.

## Change versus carrying out

- Carrying out: the same thing, said more directly. Nothing happens.
- Change: different situations, one after another. The dog at A, then at B.
- They meet when a change is phrased as an expression: "where the dog ends up" comes to "B".

## The oldest fork (parked)

- Both founding pictures of computing are built from steps by rules.
- Church: the steps act on the expression itself. Turing: the program stays fixed, and the steps act on something beside it, a tape.
- Both compute exactly the same things. Whichever is the base, the other comes back built from it: the old book chose Church, and change came back as an extension.
- That extension is effects: requests made from inside a computation and answered outside it, such as reading and changing a state. Without them, one local change, like a counter, forces every definition it passes through to carry the state along. With them, it is said once. They translate back, but by touching every definition on the way, so they add expressiveness rather than mere ease. This is where the book closes the fork: Church at the base, Turing's change rebuilt on top.
- A test for the book: one day it should be able to state this fork clearly in its own words.

## Open questions

- Values that act the same: beyond renaming, `(b) ↦ (c) ↦ c` and `(b) ↦ ((c) ↦ c)((c) ↦ c)` are different values that act the same. When are two values the same? This matters once claims about sameness arrive.
- What a placeholder really is: a link between what a function takes and the places that use it. The name only shows the link. Renaming is safe only while every use keeps its link: renaming `a` to `b` in `(a) ↦ (b) ↦ a` breaks it.
- The whole expression, outside every function, is an idea the page keeps circling without naming. Carrying out happens only there, so it never meets a placeholder, and an input put in place can never have a use caught by an inner function.
- A function with no placeholders, `() ↦ e`, is how a strict language holds something unfinished on purpose. That answers what strictness loses, and gives "even none" a reason.
- Telling a thing apart: to talk about something at all, we single it out from everything else. It may be a primitive under value.
- Is `(a, b) ↦ b` the same as `(a) ↦ (b) ↦ b`?
- To state carrying out in general, the book will need placeholders for expressions, written about the language rather than in it. What to call them, and how they relate to the language's own placeholders.
- With only functions, no example yet shows what functions are for.
- Still and moving is exact for expressions of a language we compute in. Whether all ideas split this way is a bigger question.

## For the types section

- A placeholder's type is written nowhere yet. The function's type, or the notation, must give it.
- Types will be checked on expressions before anything runs, while a type is the kind of value something is. Say the link: an expression has the type of the value it comes to.
- Name the kinds of rule when types arrive: what can be written, what is checked before carrying anything out, and what happens while carrying out.
- With functions alone, the only way to get stuck is the wrong number of inputs. The strong reason for types comes later, when functions fake other things and anything can be applied to anything.

## Groups (not needed yet)

- Some values of a type taken together, like the even numbers.
- Keep "is one of" (a value and its type) apart from "is part of" (a group and its type).
- In our language, a group will be said by a test: a function answering true or false.

## Held back

- "Shape" as an intuition for type. It fits what types are for, which things can go where, but not how values are built. Wait until the book says what types are for.
- The other things a language is made of, such as patterns and definitions: each when something needs it.

## The arc

Three layers, two barriers:
- The base: functions alone. It can compute anything.
- The core: the base with structure. Types say what kinds of things there are, and data says what shape each kind has; self-reference and "for every" come with them. All of it can be translated down to the base to run, but the translation erases exactly the structure. So the first barrier is structure, and checking is what structure makes possible.
- The extensions: each translates back into the core, one definition at a time, keeping the types. So the second barrier is translation, and crossing it adds only ease.
- Above them, a third barrier: anything that can be written as a definition in the language is not added to it at all; it is written in it. So an extension must translate back, and must also be something no definition can do (it binds names, needs the checker, or touches every definition).
- The tests decide where an idea belongs, but not that the list is complete. Only the base is complete by proof: it computes everything. The core and extensions are complete as far as we know: no simple idea is known to be awkward to say. A counterexample would show a missing idea. The tests also admit endless sugars for particular cases, such as special syntax for one data type; what keeps the extensions few is that each must be one idea in its general form.
- Why extensions are a category at all: definitions add words, but cannot add a new way of writing or a new job for the checker. Extensions are exactly the ideas that need one. A way to define new forms (macros) would turn the purely written ones, such as local definitions, into ordinary definitions; the ones that change what the checker does, such as interfaces and effects, would still have to be built in. Open: should the medium have a way to define forms?
- The aim of the extensions, in the author's words: nothing is awkward to write. Made precise, "awkward" means one of these fails: a thing can be said once; in one place; anything can go anywhere; few primitives, freely combined; words we define work as well as built-in ones; it reads like the idea; it means one thing and can be checked. When something simple is awkward, a criterion is failing, and that points to a missing idea. (From the old CLAUDE.md, for when the book reaches extensions.)
- The precise version exists: Felleisen, "On the Expressive Power of Programming Languages" (1990). A construct is mere sugar if a macro can translate each use of it in place, leaving the rest of the program untouched. Otherwise it adds expressive power, and the test is objective: adding it lets some programs that were indistinguishable before be told apart. Local definitions are sugar. State, exceptions and similar effects are not: removing them needs a rewrite of the whole program. This sharpens "translates one definition at a time" into "translates in place", and gives "expressiveness" a definition. Ease within sugar is still judged by the criteria above.
- So any new idea is placed by asking, in order: can a definition say it? Then it is written in the language. Does it translate back into the core, one definition at a time, keeping the types? Then it is an extension. Would translating it lose structure? Then it is core.

Each kind of value comes with a way to make it and a way to use it. For functions, `↦` makes one and applying uses it. For data, building from parts makes it and taking apart uses it.

From the base to the core: in the base, functions play every role. They pick between things, hold things together, count and repeat, refer to themselves, and work on anything at all. The core untangles these roles, giving each its own shape: new forms, restrictions and rules in the Note Book, read out in the Book, below a line under the base. For each role: show it played by functions alone, show what is tangled, give it its own shape, then read the shape. Once each role is its own kind of thing, "what kind of thing is this?" has an answer, which may be how types arrive.

The driving question: what can the language do with things? So far there is one movement, carrying out an application. Later: building things from parts, taking them apart, and referring back to something already defined. The old book had four: make, choose, combine (applying), repeat.

The path: a language of functions only; types, and what they rule out; data, self-reference and "for every"; lifting limits; the full shape. The old version of the full shape is in `old/old/language.md`.
