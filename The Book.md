When we say something in a language, what we write is an **expression**. Sometimes we want to leave something open, to be chosen later. For that we write a **placeholder**: a name, such as `a`.

In this language, an expression has one of three forms. To describe them we use letters that are not part of the language: `e` stands for any expression and `x` for any placeholder, and `e, …` or `x, …` for any number of them, even none.

    x               a placeholder
    (x, …) ↦ e      a function
    e(e, …)         an application

Each letter in a form is filled in independently, so the two `e`s in `e(e, …)` can be different expressions. In examples we write particular placeholders instead, such as `a` and `b`.

A **function** takes placeholders and has a **body**, the expression after `↦`. For example, `(a) ↦ a` is a function, read "take `a`, give back `a`", and `(a, b) ↦ b` reads "take `a` and `b`, give back `b`". A function can also take no placeholders, as in `() ↦ (a) ↦ a`. The placeholders a function takes are all different: in `(a, a) ↦ a`, we could not tell which one the body means.

An **application** applies an expression to others, its **inputs**: the inputs are what gets chosen for the placeholders. A body reaches as far as it can, so a function that is applied is written in parentheses: `((a) ↦ a)((b) ↦ b)` applies `(a) ↦ a` to the input `(b) ↦ b`. Without them, `(a) ↦ a((b) ↦ b)` would be a function whose body is `a((b) ↦ b)`. Parentheses like these only show how an expression is grouped. An application can also have no inputs, as in `(() ↦ (a) ↦ a)()`.

A body can contain functions of its own, and they may take the same placeholder. A placeholder used in a body **belongs** to the nearest function around it that takes it. In `(a) ↦ (b) ↦ a`, the `a` belongs to the outer function. In `(a) ↦ (a) ↦ a`, the last `a` belongs to the inner function, and the outer `a` is not used at all.

We **carry out** an expression in **steps**. Each step happens at an application, and puts its inputs in place of the placeholders of the function being applied. A function waits to be applied: no step happens inside it. So there is nothing to carry out in a function that is not applied; it is a **value**. In this language, the values are exactly the functions.

In the rules below, `v` stands for a value and `v, …` for any number of them, even none. A `…` alone stands for any number of expressions, even none. A letter or `…` written on both sides of a rule stands for the same thing on both. And "`e` becomes `e'`" means one step.

    ((x, …) ↦ e)(v, …)     becomes     e, with each v in place of the uses of its x that belong to this function,
                                       when there are as many v as x, matched in order
    e(…)                   becomes     e'(…), when e becomes e'
    v(…, e, …)             becomes     v(…, e', …), when e becomes e' and everything before e is a value

The first rule is the step itself: the first input goes in place of the first placeholder, the second in place of the second, and so on. The other two rules say where a step can happen: first in the applied expression, then in the inputs, from left to right. These are the only steps.

The simplest step puts one input in place:

    ((a) ↦ a)((b) ↦ b)                 becomes   (b) ↦ b

With several placeholders, the inputs go in order, and a placeholder may go unused:

    ((a, b) ↦ b)((c) ↦ c, (d) ↦ d)     becomes   (d) ↦ d

A function may take no placeholders. Its body still waits until the function is applied, with no inputs, even when the body needs nothing from outside:

    (() ↦ ((a) ↦ a)((b) ↦ b))()        becomes   ((a) ↦ a)((b) ↦ b), which becomes (b) ↦ b

An input goes in wherever its placeholder is used, even inside an inner function, where nothing is carried out. The result below is a function whose body still holds an application, and it is a value:

    ((a) ↦ (b) ↦ a(a))((c) ↦ c)        becomes   (b) ↦ ((c) ↦ c)((c) ↦ c)

But an input does not go where the use belongs to an inner function:

    ((a) ↦ (a) ↦ a)((c) ↦ c)           becomes   (a) ↦ a

So every placeholder used must belong to some function. Only a step puts anything in place of a placeholder, and only of the uses that belong to the function being applied. A placeholder that belonged to no function would never be replaced, and carrying out could reach it with no rule to apply.

The applied expression is carried out first, by the second rule, and then the inputs, by the third. The parentheses each result needs follow from the grouping, since the rules work on how an expression is grouped, not on how it is written:

    ((a) ↦ a)((b) ↦ b)(((c) ↦ c)((d) ↦ d))
        becomes   ((b) ↦ b)(((c) ↦ c)((d) ↦ d))
        becomes   ((b) ↦ b)((d) ↦ d)
        becomes   (d) ↦ d

Inputs are carried out from left to right, each before it goes in, even one whose placeholder is never used:

    ((a, b) ↦ b)(((c) ↦ c)((d) ↦ d), ((c) ↦ c)((b) ↦ b))
        becomes   ((a, b) ↦ b)((d) ↦ d, ((c) ↦ c)((b) ↦ b))
        becomes   ((a, b) ↦ b)((d) ↦ d, (b) ↦ b)
        becomes   (b) ↦ b

A placeholder can be applied, or be an input. Once a value is in its place, that application takes its own step. An input used twice is still carried out only once:

    ((a) ↦ a(a))(((b) ↦ b)((c) ↦ c))
        becomes   ((a) ↦ a(a))((c) ↦ c)
        becomes   ((c) ↦ c)((c) ↦ c)
        becomes   (c) ↦ c

Carrying out ends when the whole expression is a value, since no rule applies to a value. It can also get **stuck**. When an application's applied expression and inputs are all values but there are not as many inputs as placeholders, no rule applies, although there is still something to carry out. That happens with too few inputs or too many:

    ((a) ↦ a())((b) ↦ b)               becomes   ((b) ↦ b)(), which is stuck: one placeholder, no inputs
    ((a) ↦ a)((b) ↦ b, (c) ↦ c)        is stuck: one placeholder, two inputs

Or it can go on forever:

    ((a) ↦ a(a))((a) ↦ a(a))           becomes   ((a) ↦ a(a))((a) ↦ a(a)), the same expression again

At each point at most one rule applies, and in one place, so there is only one way to carry out an expression, and it reaches at most one value.

The rules make four choices, and each could have gone another way. Inputs could have gone in unfinished; carrying them out first means each is carried out once, however many times it is used, even none. Bodies could have been carried out early; leaving them alone means a function is a value as soon as it is written. Steps could have happened in any order; one fixed order means carrying out never has to choose. And a function given too few inputs could have waited for the rest, and one given too many could have passed the rest on to its result; asking for exactly as many inputs as placeholders means every step fills every placeholder and uses every input.
