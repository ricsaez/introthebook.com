When we say something in a language, what we write is an **expression**. Sometimes we want to leave something open, to be chosen later. For that we write a **placeholder**: a name, such as `a`.

In this language, an expression has one of three forms. To describe them we use letters that are not part of the language: `e` stands for any expression and `x` for any placeholder, each independently, and `e, …` or `x, …` for any number of them, even none. In examples we write particular placeholders instead, such as `a` and `b`.

    x               a placeholder
    (x, …) ↦ e      a function
    e(e, …)         an application

A **function** takes placeholders, all different, and has a **body**, the expression after `↦`. For example, `(a) ↦ a` is a function, read "take `a`, give back `a`".

An **application** applies an expression to others, its **inputs**. A body reaches as far as it can, so a function that is applied is written in parentheses: `((a) ↦ a)((b) ↦ b)` applies `(a) ↦ a` to the input `(b) ↦ b`. Parentheses like these only show how an expression is grouped, and the rules below work on the grouping, not on how it is written.

A body can contain functions of its own. A placeholder used in a body **belongs** to the nearest function around it that takes that placeholder. Every placeholder used must belong to some function: otherwise nothing would ever be put in its place.

A function waits to be applied: nothing in its body is carried out before then. So a function on its own is finished, and a finished expression is a **value**. In this language, the values are exactly the functions.

We **carry out** an expression in **steps**, and every step happens at an application. In the rules below, `v` stands for a value and `v, …` for any number of them, `e'` for what `e` becomes in one step, and a `…` on its own for any expressions. A letter or `…` written on both sides of a rule stands for the same thing on both.

    ((x, …) ↦ e)(v, …)     becomes     e, with each v in place of the uses of its x that belong to this function
    e(…)                   becomes     e'(…), when e becomes e'
    v(…, e, …)             becomes     v(…, e', …), when e becomes e' and everything before e is a value

The first rule is the step itself. It needs one input for each placeholder: the first input goes with the first placeholder, the second with the second, and so on. The other two rules say where a step can happen: in the applied expression first, then in the inputs from left to right. These are the only steps, and none of them steps inside a function. For example:

    ((a, b) ↦ b)((c) ↦ c, (d) ↦ d)     becomes   (d) ↦ d
    ((a) ↦ a)(((b) ↦ b)((c) ↦ c))      becomes   ((a) ↦ a)((c) ↦ c), which becomes (c) ↦ c
    ((a) ↦ (a) ↦ a)((c) ↦ c)           becomes   (a) ↦ a, since the last a belongs to the inner function

When the whole expression is a value, no rule applies, and carrying out ends. Carrying out may instead get **stuck**, when the next step would apply a function to the wrong number of inputs, or go on forever, as `((a) ↦ a(a))((a) ↦ a(a))` does. Since there is only one way to carry out an expression, it reaches at most one value.

These rules make four choices. Inputs are carried out before they are put in place, so their steps are taken once, however many uses their placeholder has. Bodies are never stepped into, so a function is finished as soon as it is written. Steps happen in one order, so carrying out never has to choose. And a function needs one input for each placeholder, so a step puts something in place of every placeholder at once.
