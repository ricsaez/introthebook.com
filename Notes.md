# Notes

Insights and open questions the book has not reached yet. When the book says something, or a question is settled, it leaves this file.

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

Each kind of value comes with a way to make it and a way to use it. For functions, `↦` makes one and applying uses it. For data, building from parts makes it and taking apart uses it.

The driving question: what can the language do with things? So far there is one movement, carrying out an application. Later: building things from parts, taking them apart, and referring back to something already defined. The old book had four: make, choose, combine (applying), repeat.

The path: a language of functions only; types, and what they rule out; data, self-reference and "for every"; lifting limits; the full shape. The old version of the full shape is in `old/old/language.md`.
