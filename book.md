# The Book

This book builds a small language, one piece at a time. Each piece is added because of something the pieces before it cannot say, and each comes with examples that mean something. Most are about the game rock, paper, scissors.

The language has two parts. The **core** is for saying things: things, functions that turn things into other things, and the kinds things come in. **Effects** are for doing things: keeping a score that changes, giving up, trying every possibility, talking to the world outside.

As we go, we keep a **map** of the language: a few lines that list every form it has. Each section adds to it. By the end, the map is the whole language on one page, and every way its pieces combine can be read off it.

## 1. Things

The simplest thing to say is a thing that is just itself. We say what kind of thing it is by listing every thing of that kind:

    Hand = rock | paper | scissors

The `|` reads "or". This **declares** a **type**, `Hand`: a kind of thing. A `Hand` is `rock`, `paper` or `scissors`, and nothing else. `rock`, `paper` and `scissors` are **constructors**: names that stand only for themselves. A type **lists** the constructors declared in it.

Here is another type:

    Bool = false | true

Each type is declared once, and each constructor belongs to one type only.

Anything we write that says something is an **expression**. A constructor is the simplest expression: it says itself, and there is nothing in it to work out.

So far, things only sit there. Nothing turns one into another.

## 2. Functions

A **function** turns things into other things. To say what it does, we say what it gives back for each thing it can be given. Each of these is a **case**:

    (rock) ↦ paper | (paper) ↦ scissors | (scissors) ↦ rock

`↦` reads "gives", and `|` again reads "or": rock gives paper, or paper gives scissors, or scissors gives rock. In each case, what is in parentheses is what the case is for, and what comes after `↦` is its **body**: what the case gives back. This function gives back the hand that beats the one it is given.

To use a function more than once, we give it a name:

    answer : (Hand) → Hand
    answer = (rock) ↦ paper | (paper) ↦ scissors | (scissors) ↦ rock

The second line **defines** the name `answer`: from now on, `answer` stands for this function. The first line **declares** it, saying what kind of thing it is. `(Hand) → Hand` reads "takes a `Hand` and gives back a `Hand`". That is a type too, a **function type**. A name given this way is a **defined name**.

To use a function, we **apply** it to an **input**, written after it in parentheses:

    answer(rock)

This is a question: what does `answer` give back for `rock`? To find out, we **carry it out**, in **steps**:

    answer(rock)
        becomes   ((rock) ↦ paper | (paper) ↦ scissors | (scissors) ↦ rock)(rock)
        becomes   paper

In the first step, the name becomes the function it stands for. In the second, the function is applied: its cases are tried in order, and the application becomes the body of the first case that is for `rock`. Nothing is left to work out in `paper`, so `paper` is the answer.

The parentheses around the function only keep it in one piece. Without them, the input `(rock)` would seem to belong to the last case.

From here on, "comes to" means "becomes, in one step or more", and we often skip the step where a name becomes what it stands for:

    answer(rock)      comes to   paper

One more function, which turns each `Bool` into the other:

    not : (Bool) → Bool
    not = (false) ↦ true | (true) ↦ false

    not(true)         comes to   false

## 3. The map

We have now written four kinds of expression:

    rock                                 a constructor
    not                                  a defined name
    (false) ↦ true | (true) ↦ false      a function
    not(true)                            an application

To describe them all at once, and not just these examples, we use letters that are never written in the language itself. `e` stands for any expression, `c` for any constructor, and `n` for any defined name:

    e ::= c                  a constructor
          n                  a defined name
          (c) ↦ e | …        a function
          e(e)               an application

`::=` reads "is one of", and each line is one **form**. So this says: an expression is a constructor, or a defined name, or a function, or an application. In the form of a function, `| …` means "and more cases like the first, as many as needed": a function has one case or more.

This is the first piece of the map. Its letters are not expressions, constructors or names. Each marks a place where any expression, constructor or name can go.

The map shows something the examples don't. Look at the forms with an `e` inside them. A case's body is an `e`, so a body can be any expression, not just a constructor. An application is `e(e)`, so the function applied can be any expression, and so can its input, including another application:

    answer(answer(rock))

A form with `e` inside it can hold any form, itself included, so forms go inside forms as deep as we like. Four forms combine without end.

Types have a map of their own. Using `T` for the name of a type, and `A` and `B` for any types:

    A ::= T                  a type declared with its constructors       Hand
          (A) → B            a function type                             (Hand) → Hand

A function type has types inside it, so it too can hold any type, a function type included.

And what we write is one of three things:

    T = c | …                declares the type T, with its constructors
    n : A                    declares n, of type A
    n = e                    defines n as e

Every section from here on adds to the map.

## 4. Carrying out

In `answer(answer(rock))` there is an application inside an application. Which is carried out first?

The rule is: an input is worked out before it goes in. So first `answer(rock)`, then `answer` of what it gave:

    answer(answer(rock))
        comes to   answer(paper)
        comes to   scissors

The answer to the answer to rock: paper, then scissors.

An expression with nothing left to work out is a **value**. Using `v` for any value:

    v ::= c                  a constructor
          (c) ↦ e | …        a function

A function is a value even though its bodies are expressions: nothing happens inside a function until it is applied. Only then is a body carried out, and only the body of the case that is used.

Here is all of carrying out so far. "`e` becomes `e'`" means one step, and `e'` is what `e` becomes; `f` stands for any function:

    n          becomes   what n is defined as
    f(v)       becomes   the body of f's first case for v, when f is a function

Where a step happens: in an application, first in the function being applied, until it is a value; then in its input, until that is a value; then the application takes its own step. So at each point there is one next step at most, and carrying out never has a choice to make.

But writing a case for every input soon says the same thing more than once.

## 5. Placeholders

This function gives back the hand that a given hand beats, which is the answer to its answer:

    beaten : (Hand) → Hand
    beaten = (rock) ↦ answer(answer(rock)) | (paper) ↦ answer(answer(paper)) | (scissors) ↦ answer(answer(scissors))

It says the same thing three times. To say it once, for whatever hand is given, a case can have a **placeholder** where it had a constructor: a new name that stands for whatever the function is given.

    beaten = (a) ↦ answer(answer(a))

The case is for any `Hand`, and its body uses `a` where the hand given goes. When the function is applied, the input is put in place of the placeholder:

    beaten(rock)
        comes to   answer(answer(rock))
        comes to   scissors

A placeholder is like the letters on the map, but written in the language. On the map, `e` marks a place for any expression; in a body, `a` marks a place for whatever the function is given. A placeholder means something only in the body of its own case. A name declared as a constructor is a constructor wherever it is written; any other name in a case's parentheses is a placeholder.

What a case is for is now a **pattern**, one of two things. Using `p` for any pattern and `x` for any placeholder:

    p ::= c                  a constructor: matches only itself
          x                  a placeholder: matches anything

A value **matches** a pattern when the pattern describes it. Cases are tried in order, and the first that matches is used, so a last case with a placeholder catches everything the cases before it leave. That is how a function says "otherwise":

    isRock : (Hand) → Bool
    isRock = (rock) ↦ true | (a) ↦ false

    isRock(paper)     comes to   false

The map changes in two places. A placeholder is a new form of expression, and a case's pattern can now be any pattern:

    e ::= x                  a placeholder                                 new
          c
          n
          (p) ↦ e | …        a function                                    (c) widened to (p)
          e(e)

And applying puts the input in place:

    f(v)       becomes   the body of f's first case that v matches, with v in place of the placeholder, if the pattern is one

## 6. Several inputs

Some questions are about two things at once: does one hand beat another? A function can take several inputs, separated by commas, and then each case has one pattern for each:

    beats : (Hand, Hand) → Bool
    beats = (rock, scissors) ↦ true | (scissors, paper) ↦ true | (paper, rock) ↦ true | (a, b) ↦ false

Rock beats scissors, scissors beat paper, paper beats rock; otherwise, the first hand doesn't win. Several values match a case when each matches its own pattern, in order:

    beats(paper, rock)      comes to   true
    beats(rock, paper)      comes to   false

In the second, only the last case matches. Without placeholders, `beats` would need nine cases, one for each pair of hands. The placeholders of one case are all different: in `(a, a) ↦ a`, we couldn't tell which input `a` means.

Another function of two inputs, which asks whether both are `true`:

    and : (Bool, Bool) → Bool
    and = (true, b) ↦ b | (false, b) ↦ false

Inputs are worked out one at a time, from left to right, each to a value, before the function takes its step:

    and(not(false), beats(rock, paper))
        comes to   and(true, beats(rock, paper))
        comes to   and(true, false)
        comes to   false

A function can also take no inputs at all, and is then applied to none. Its body waits until then, so it is a question kept for later:

    later : () → Hand
    later = () ↦ answer(rock)

    later()           comes to   paper

On the map, functions, applications and function types now take any number of inputs. In `p, …`, the `…` means "any number of them, even none", and likewise in `e, …` and `A, …`:

    e ::= (p, …) ↦ e | …     a function                                    widened
          e(e, …)            an application                                widened
    A ::= (A, …) → B         a function type                               widened

A function is given exactly as many inputs as its cases have patterns, and applying puts each in place:

    f(v, …)    becomes   the body of f's first case whose patterns v, … match, with each value in place of its placeholder

## 7. Checking

So far we have only applied functions to what they were made for. What if a function is given something it has no case for?

    answer(true)
        becomes   ((rock) ↦ paper | (paper) ↦ scissors | (scissors) ↦ rock)(true)

No case matches `true`, so no rule applies, yet something is left to work out. Carrying out is **stuck**. It gets stuck in the same way when something that is not a function is applied, or a function is given the wrong number of inputs:

    rock(paper)         stuck: rock is not a function
    beats(rock)         stuck once beats becomes its function: it takes two inputs, and is given one

In each, a value is somewhere it doesn't fit. And we already wrote down what fits: `answer : (Hand) → Hand` says `answer` takes a `Hand`. **Checking** makes sure that what we write agrees with it, before anything is carried out. We find the type of each expression from the types of its parts, with one rule for each form on the map. We write `e : A`, read "`e` has type `A`":

    c : T                       when T lists c                                 rock : Hand
    n : A                       when n is declared n : A                       answer : (Hand) → Hand
    x : A                       when its pattern gives it A
    e(e', …) : B                when e : (A, …) → B, and e', … : A, …          answer(rock) : Hand
    ((p, …) ↦ e | …) : (A, …) → B
                                when in every case the patterns fit A, …, and the body : B;
                                and any values of types A, … match some case

`e', … : A, …` means as many expressions as types, each of its own type, in order. A pattern **fits** a type when it describes values of that type: a constructor fits the type that lists it, and a placeholder fits any type, and gives the placeholder that type in the body.

So checking a function goes case by case. In `beats`, every pattern fits `Hand`, every body is a `Bool`, and any two hands match some case, since the last case matches anything. So `beats : (Hand, Hand) → Bool`, as declared.

None of the stuck expressions has a type:

    answer(true)        answer takes a Hand, and true : Bool
    rock(paper)         rock : Hand, which is not a function type
    beats(rock)         beats takes two inputs, and is given one

Neither does a function that has no case for some value: `(rock) ↦ paper` does not have type `(Hand) → Hand`, since `paper` and `scissors` match no case.

An expression that has a type is **checked**. What we write is checked when, for each definition `n = e`, `e` has the type declared for `n`, and the expression we carry out has a type. Since every name's type is declared, a definition can use any name, even one defined after it.

A checked expression never gets stuck. Each step keeps its type. And while it is not a value, there is always a next step: when an application's parts are all values, the one applied has a function type, so it is a function; it is given as many inputs as it takes, of the types it takes, so some case matches them.

## 8. Holding

So far a value is one thing: one hand, one `Bool`. But a round of the game is two hands, one for each player. A constructor can hold things, its **contents**, of the types written after it:

    Round = round(Hand, Hand)

A constructor that holds contents is applied to them, like a function:

    round(rock, paper)

But it takes no step: it holds them. `round(rock, paper)` is a value: a round in which the first player showed rock and the second paper. Values built from constructors are **data**. Contents are worked out before they go in, like any input:

    round(rock, answer(rock))      comes to   round(rock, paper)

To use data, we take it apart with a pattern: a constructor, with patterns for its contents.

    won : (Round) → Bool
    won = (round(a, b)) ↦ beats(a, b)

    won(round(paper, rock))        comes to   true

The pattern `round(a, b)` matches any round, and gives `a` the first hand and `b` the second. The contents of a pattern are patterns, so a pattern can look more than one layer in:

    draw : (Round) → Bool
    draw = (round(rock, rock)) ↦ true | (round(paper, paper)) ↦ true | (round(scissors, scissors)) ↦ true | (r) ↦ false

    draw(round(paper, rock))       comes to   false

`round(paper, rock)` doesn't match `round(paper, paper)`, since `rock` doesn't match `paper`; only the last case matches it.

On the map, a constructor can now be declared holding contents. Using `d` for a constructor as it is declared:

    T = d | …                declares the type T                           widened from T = c | …
    d ::= c                  a constructor holding nothing
          c(A, …)            a constructor holding contents of types A, …

There is no new form of expression: `round(rock, paper)` is already an application, `e(e, …)`. But there is a new value, and a new pattern:

    v ::= c(v, …)            data: a constructor holding values             new
    p ::= c(p, …)            data built by c, with contents matching p, …   new

`v` is inside its own new form, so data can hold data. `p` is inside its own new form, so patterns nest.

And two rules for checking:

    c : (A, …) → T           when T lists c(A, …)                           round : (Hand, Hand) → Round
    c(p, …) fits T           when T lists c(A, …), and p, … fit A, …

A checked expression still never gets stuck: a constructor that holds contents, given as many values as it holds, is data, and so a value.

## 9. Repeating

Every type so far has only so many values: a `Round` is one of nine. Some kinds of thing have no end. A constructor's contents can be of the very type being declared:

    Nat = zero | succ(Nat)

A `Nat` is `zero`, or `succ` holding a `Nat`; `succ` reads "one more than". So `zero`, `succ(zero)`, `succ(succ(zero))` are zero, one, two, and so on, with no end. The rounds of a game are built the same way:

    Rounds = empty | cons(Round, Rounds)

Rounds are none, or a round followed by more rounds:

    cons(round(paper, rock), cons(round(rock, paper), empty))

A pattern sees only as many layers as it is written with. To work through rounds of any length, a function must go once around for each layer, so it uses itself. A definition can use its own name:

    more : (Bool, Nat) → Nat
    more = (true, n) ↦ succ(n) | (false, n) ↦ n

    wins : (Rounds) → Nat
    wins = (empty) ↦ zero | (cons(r, rest)) ↦ more(won(r), wins(rest))

`wins` counts the rounds the first player won. In no rounds, none. In a round followed by the rest: as many as in the rest, and one more if this round was won.

    wins(cons(round(paper, rock), cons(round(rock, paper), empty)))
        comes to   more(won(round(paper, rock)), wins(cons(round(rock, paper), empty)))
        comes to   more(true, wins(cons(round(rock, paper), empty)))
        comes to   more(true, more(won(round(rock, paper)), wins(empty)))
        comes to   more(true, more(false, wins(empty)))
        comes to   more(true, more(false, zero))
        comes to   more(true, zero)
        comes to   succ(zero)

Each time around takes one layer off the rounds, until they are `empty` and the first case ends it. One round was won.

Adding works the same way. Zero and `n` make `n`; one more than `m`, and `n`, make one more than `m` and `n`:

    add : (Nat, Nat) → Nat
    add = (zero, n) ↦ n | (succ(m), n) ↦ succ(add(m, n))

    add(succ(zero), succ(zero))      comes to   succ(succ(zero))

Nothing makes a definition use itself on something smaller, so repeating need not stop:

    loop : (Bool) → Bool
    loop = (a) ↦ loop(a)

    loop(true)      comes to   loop(true), and so on, forever

`loop` is checked. A type says what kind of answer an expression gives, if it gives one, not that it gives one. So a checked expression comes to a value of its type, or goes on forever. It never gets stuck.

Repeating added nothing to the map. It is two uses of forms already there: a type's name inside the contents of its own constructors, and a name inside its own definition. The map already allows both. The `A`s in `c(A, …)` can be any type, the one being declared included, and the `e` in `n = e` can be any expression, `n` included.

## 10. Functions as values

A function is a value, like a hand. So whatever can be done with a value can be done with a function: it can be an input, be given back, or be held in data.

A function given a function:

    twice : ((Hand) → Hand, Hand) → Hand
    twice = (f, a) ↦ f(f(a))

    twice(answer, rock)       comes to   scissors

The type reads: takes a function from `Hand` to `Hand`, and a `Hand`, and gives back a `Hand`. In the body, the placeholder `f` stands for a function, so it can be applied. As with any input, `answer` is worked out before it goes in: what goes in place of `f` is the function `answer` stands for.

A function that gives back a function:

    against : (Hand) → (Hand) → Bool
    against = (a) ↦ (b) ↦ beats(b, a)

    against(rock)             comes to   (b) ↦ beats(b, rock)
    against(rock)(paper)      comes to   true

`against(rock)` is a test: does this hand beat rock? A body reaches as far as it can, so the body of the outer function is the whole inner function. In the same way, `(Hand) → (Hand) → Bool` takes a `Hand` and gives back a `(Hand) → Bool`. And `rock` went in place of `a` inside the inner function: an input goes wherever its placeholder is used, even where nothing is carried out yet.

Data holding a function. A player plays by a rule: given the hand the other player showed last, it gives the hand to show next.

    Player = player((Hand) → Hand)

    next : (Player, Hand) → Hand
    next = (player(f), a) ↦ f(a)

    next(player(answer), rock)      comes to   paper

This player always shows what would have beaten the last hand.

A pattern can take data apart, but nothing takes a function apart: only a placeholder fits a function type. A function given a function can apply it, and nothing else.

Again the map needed nothing new. Every combination here was already on it. The types in `(A, …) → B` and in `c(A, …)` can be function types, a body can be a function, and the applied part of `e(e, …)` can itself be an application, as in `against(rock)(paper)`. Every value is data or a function, so every place a value can go holds one or the other:

    (Hand) → Bool                      takes data, gives data               isRock
    (Hand) → (Hand) → Bool             takes data, gives a function         against
    ((Hand) → Hand, Hand) → Hand       takes a function, gives data         twice
    ((Hand) → Hand) → (Hand) → Hand    takes a function, gives a function   (f) ↦ (a) ↦ f(f(a))
    Round = round(Hand, Hand)          data holding data                    round(rock, paper)
    Player = player((Hand) → Hand)     data holding a function              player(answer)

## 11. Types left open

`Rounds` holds rounds. A list of hands would need another type just like it, with constructors of its own, and counting its items would need another function just like the one for rounds. The idea is the same each time; only the type of the items changes.

So a type can leave something open too. In a type, a **placeholder** is a name that is not a declared type, such as `X`, and it stands for any type. A type can take placeholders, and is then filled with types wherever it is used:

    List(X) = empty | cons(X, List(X))
    Pair(X, Y) = pair(X, Y)

`List(Hand)` is a list of hands, and `List(Round)` a list of rounds, which is what `Rounds` was. `Pair(Bool, Nat)` holds a `Bool` and a `Nat`.

A defined name's type can have placeholders too, and then its definition works for any type in their place:

    length : (List(X)) → Nat
    length = (empty) ↦ zero | (cons(a, rest)) ↦ succ(length(rest))

    map : ((X) → Y, List(X)) → List(Y)
    map = (f, empty) ↦ empty | (f, cons(a, rest)) ↦ cons(f(a), map(f, rest))

    length(cons(rock, cons(paper, empty)))            comes to   succ(succ(zero))
    map(answer, cons(rock, cons(paper, empty)))       comes to   cons(paper, cons(scissors, empty))

`map` gives back the list of what `f` gives for each item. Each use of a name chooses a type for each placeholder in its type, the same throughout that use: here `Hand` for both `X` and `Y`. The definition itself is checked without knowing which type `X` is, so it cannot look at what it is given there: only a placeholder pattern fits `X`.

On the map:

    T(X, …) = d | …          a type that takes types                        widened
    A ::= T(A, …)            a type, filled; T alone if it takes none       widened from T
          X                  a placeholder in a type                        new

The `A`s in `T(A, …)` are any types, so types nest too: `List(List(Hand))`, `List((Hand) → Hand)`, `Pair(Round, List(Bool))`.

### The core

That is the whole core. Every value is data or a function, and each is made one way and used one way:

|          | made by                           | used by                          |
|----------|-----------------------------------|----------------------------------|
| data     | a constructor, holding values     | a pattern, which takes it apart  |
| function | cases, each a pattern and a body  | applying it to inputs            |

Two kinds of name refer to values: a placeholder stands for what a case was given, and a defined name stands for its definition, which may use the name itself.

Everything in the core says something, and nothing changes: an expression and what it comes to say the same thing, the second more directly. The rest of the book is about doing.

## 12. State, written out

As they play, the players keep score: after each round the first player wins, their score goes up by one. A score changes, and in the core nothing changes. But a change can be said with values: the score before, and the score after.

    score : (Round, Nat) → Nat
    score = (r, n) ↦ more(won(r), n)

Given a round and the score before it, `score` gives back the score after it. Two rounds, the first won and the second lost, starting from zero:

    score(round(rock, paper), score(round(paper, rock), zero))
        comes to   score(round(rock, paper), succ(zero))
        comes to   succ(zero)

The score is a **memory**: something kept, and changed as we go. Here it is written out as a value, handed from one step to the next. The inner `score` is an input, so it is carried out first, and the first round is counted first: the order of the rounds is the order in which the memory is handed on.

This works, but it has a cost. This function asks whether the first player won both of two rounds:

    both : (Round, Round) → Bool
    both = (r, s) ↦ and(won(r), won(s))

To keep the score as well, playing a round must give back two things, whether it was won and the new score:

    play : (Round, Nat) → Pair(Bool, Nat)
    play = (r, n) ↦ pair(won(r), more(won(r), n))

And `both` must be rewritten to take the score, hand it through each round in turn, and give it back with its answer:

    both : (Round, Round, Nat) → Pair(Bool, Nat)
    both = (r, s, n) ↦ ((pair(a, m)) ↦ ((pair(b, t)) ↦ pair(and(a, b), t))(play(s, m)))(play(r, n))

This reads: play `r` with the score `n`, and call what it gives back `a` and `m`; play `s` with `m`, and call what that gives back `b` and `t`; give back `and(a, b)`, with the score `t`.

`both` doesn't care about the score at all: it only hands it along. And every function that uses `both` must now do the same, and every function that uses those. The memory goes in and out of every function between the place it starts and the places that use it, and each of their types changes. One small idea, a score, touches everything on the way.

What we want is to mention the score only where it is used.

## 13. Requests and handlers

Take a simpler case of the same problem first. To know whether a hand wins, a function needs the hand the opponent plays. Written out, that hand would be one more input, handed through every function on the way, just like the score. Instead, the function can ask for it:

    effect Opponent = opponent : () → Hand

This declares an **effect**, `Opponent`, with one **request**, `opponent`. Like a function type, `() → Hand` says what the request takes and what it is answered with: it takes nothing, and is answered with a `Hand`. A request is written like an application:

    beatsOpponent : (Hand) → Bool ! {Opponent}
    beatsOpponent = (a) ↦ beats(a, opponent())

The type says that `beatsOpponent` takes a `Hand` and gives back a `Bool`, and that along the way it may make requests of `Opponent`: `! {Opponent}` reads "and may ask Opponent".

Carrying out `beatsOpponent(rock)` goes as far as this:

    beatsOpponent(rock)
        comes to   beats(rock, opponent())

The next thing to work out is the input `opponent()`, and no rule carries it out. A request is not a value, and takes no step of its own: it waits to be answered.

A **handler** answers it. It goes around the expression that asks:

    handle beatsOpponent(rock) with
          (x) ↦ x
        | (opponent(), k) ↦ k(scissors)

A handler has cases, like a function. The first is for the value the expression comes to in the end: here it gives that value back as it is. The second is for the request `opponent()`, and its body answers `scissors`. To see how, we need to know what `k` is.

When the request is made, the expression around it is waiting for an answer: `beats(rock, opponent())` is waiting for a hand, to put where `opponent()` is. That waiting expression is **the rest** of the computation, and it can be written as a function. Given an answer, it puts it where the request was, and goes on, with the handler still around it:

    (y) ↦ handle beats(rock, y) with …

Here, and from now on, `with …` stands for the same handler's cases, written out again. The request's case receives the rest as its placeholder `k`. So when `opponent()` is reached, the handler takes a step: it becomes the body of its case for the request, with the rest in place of `k`. Applying the rest to `scissors` goes on with `scissors` where the request was:

    handle beatsOpponent(rock) with …
        comes to   handle beats(rock, opponent()) with …
        becomes    ((y) ↦ handle beats(rock, y) with …)(scissors)
        becomes    handle beats(rock, scissors) with …
        comes to   handle true with …
        becomes    true

In the last step, the expression has come to a value, `true`, so the handler becomes the body of its first case, which gives `true` back.

`beatsOpponent` never says where the opponent's hand comes from: the handler does. A handler that answered `paper` would make the same `beatsOpponent(rock)` come to `false`, without changing `beatsOpponent` at all.

On the map, using `F` for the name of an effect, `r` for a request, and `h` for a handler's case:

    effect F = r : (A, …) → B | …    declares the effect F, with its requests         new

    e ::= r(e, …)                    a request                                         new
          handle e with h | …        a handler around e                                new

    h ::= (p) ↦ e                    for the value e comes to
          (r(p, …), x) ↦ e           for a request: patterns for its inputs, and a placeholder for the rest

The steps:

    handle v with h | …     becomes   the body of the first case for values that v matches, with v in place
    handle e with h | …     becomes   the body of the first case for the request r(v, …),
                                      with the request's inputs in place, and the rest in place of x,
                                      when the next thing to work out in e is r(v, …),
                                      and no handler inside e has a case for r

Otherwise, the expression inside a handler takes its steps as usual. A request that a handler has no case for passes it by, to a handler further out.

Requests and handlers are made one way and used one way, like data and functions:

|        | made by   | used by                      |
|--------|-----------|------------------------------|
| effect | a request | a handler, which answers it  |

## 14. Four ways to go on

The handler above went on once, with one answer. But it holds the rest as a value, `k`, and can do with it whatever a function can do with a function.

It can never go on:

    handle beatsOpponent(rock) with
          (x) ↦ x
        | (opponent(), k) ↦ false

        comes to   false

The case never applies `k`, so the rest never happens: the handler's answer stands in for the whole computation. This is how errors work. Something asks for what can't be given, and the handler decides what happens instead.

It can go on several times. This handler asks, for each hand the opponent could play, whether `rock` wins, and counts the wins:

    count : (Bool) → Nat
    count = (true) ↦ succ(zero) | (false) ↦ zero

    handle beatsOpponent(rock) with
          (x) ↦ count(x)
        | (opponent(), k) ↦ add(k(rock), add(k(paper), k(scissors)))

        comes to   succ(zero)

Each `k(…)` goes on from the request with a different hand, and comes to a value through the handler's first case:

    k(rock)          comes to   count(false), which comes to zero
    k(paper)         comes to   count(false), which comes to zero
    k(scissors)      comes to   count(true), which comes to succ(zero)

and `add(zero, add(zero, succ(zero)))` comes to `succ(zero)`. Against the three hands, rock wins once. This is search: one question, every answer tried.

It can also keep `k` in data, and go on later. That is how a generator gives back items one at a time, and how two computations take turns.

So a handler receives a request and the rest, and chooses how to go on: once, never, several times, or later. The same two forms, request and handler, say all four.

## 15. State, said once

Now the score:

    Unit = unit
    effect Score = get : () → Nat | put : (Nat) → Unit

`get` asks for the score, and `put` sets it. `put` has nothing to give back, so it is answered with `unit`, the only value of `Unit`, which says nothing.

To do one thing and then another:

    then : (Unit, X) → X
    then = (u, a) ↦ a

Since inputs are worked out from left to right, `then(e, e')` works out `e` first, then `e'`, and gives back what `e'` gave. In the core, the order of steps changed nothing that was said. With requests it does: asking for the score before setting it, or after, gives different answers. Order has become part of what is said.

Recording a round:

    record : (Bool) → Bool ! {Score}
    record = (true) ↦ then(put(succ(get())), true) | (false) ↦ false

Given that a round was won, `record` adds one to the score and gives back `true`; given that it was lost, it changes nothing. And `both`, rewritten once more:

    both : (Round, Round) → Bool ! {Score}
    both = (r, s) ↦ and(record(won(r)), record(won(s)))

Compare it with the first `both`, `(r, s) ↦ and(won(r), won(s))`. Only `record`, the one place that changes the score, mentions it. No score is handed in or out, and a function that uses `both` doesn't change.

A handler holds the memory:

    keep : (() → X ! {Score}, Nat) → Pair(X, Nat)
    keep = (c, n) ↦ (handle c() with
                          (x) ↦ ((m) ↦ pair(x, m))
                        | (get(), k) ↦ ((m) ↦ k(m)(m))
                        | (put(s), k) ↦ ((m) ↦ k(unit)(s)))(n)

`keep` takes a computation kept for later, `c`, and a starting score, `n`. Its handler doesn't give back an answer directly: it gives back a function waiting for the score so far, `m`. The `(n)` at the end gives it the starting score; the parentheses before it keep the handler in one piece, so it can be applied. Its cases read:

- when the computation comes to a value `x`: given the score `m`, give back `x` together with `m`;
- when asked for the score: given `m`, answer `m`, and go on with the score still `m`;
- when told to set the score to `s`: given `m`, answer `unit`, and go on with the score `s` instead.

The rest, `k`, has the handler around it, so it too gives back a function waiting for a score. That is why what `k(m)` gives back is given a score in turn: `k(m)(m)`.

Two rounds, the first won and the second lost, starting from zero:

    keep(() ↦ both(round(paper, rock), round(rock, paper)), zero)
        comes to   ((m) ↦ k(m)(m))(zero)                    the first request is get()
        becomes    k(zero)(zero)
        comes to   ((m) ↦ k'(unit)(succ(zero)))(zero)        the next request is put(succ(zero))
        becomes    k'(unit)(succ(zero))
        comes to   ((m) ↦ pair(false, m))(succ(zero))        the rest comes to false
        becomes    pair(false, succ(zero))

Here `k` is the rest after `get()`, and `k'` the rest after `put(succ(zero))`. Not both rounds were won, and the score is one.

The memory is handed along exactly as in section 12, in and out of every step. But now that is written once, in the handler, instead of in every function on the way.

## 16. Input and output

A request that no handler in the program answers goes outside it. Showing something on a screen, or reading what someone types, is a request answered by whatever runs the program:

    effect Console = show : (Hand) → Unit | read : () → Hand

    game : () → Bool ! {Console}
    game = () ↦ then(show(rock), beats(rock, read()))

`game` shows `rock`, reads the other player's hand, and says whether rock won. Carried out, it stops at `show(rock)` and waits; whatever runs it shows rock on the screen and answers `unit`. At `read()` it waits again, for a hand. Inside the language, a handler can stand in for the outside, which is how such a program is tested:

    handle game() with
          (x) ↦ x
        | (show(a), k) ↦ k(unit)
        | (read(), k) ↦ k(scissors)

        comes to   true

## 17. Checking effects

A function type now also says which effects its function may request:

    A ::= (A, …) → B ! E      a function type, and what it may request      widened

`E` lists effects, as in `{Opponent}` or `{Score, Console}`. When it lists none, `! E` is left out, so `(Hand) → Hand` requests nothing.

The rules add up what may be requested:

    r(e, …) : B ! {F}         when F lists r : (A, …) → B, and e, … : A, …

- An expression may request whatever its parts may, and an application may also request what its function's type lists.
- A function's type lists what its bodies may request.
- A handler answers the requests of one effect, so the expression around which it is written may request that effect, and the handler as a whole may not. In its case for values, the pattern fits the type of what the expression comes to. In each case for a request, the patterns fit the request's input types, and the rest, `k`, takes the request's answer type and gives back what the handler gives back. And there is a case for every request of the effect.

A type can leave effects open, as `X` leaves a type open. `map` applies `f`, so whatever `f` may request, `map` may too:

    map : ((X) → Y ! Z, List(X)) → List(Y) ! Z

`Z` is a placeholder for effects, and each use of `map` chooses them.

With these rules, a checked expression comes to a value of its type, goes on forever, or makes a request its type lists. So a checked program whose type lists no effects never gets stuck, and never makes a request that nothing answers. One whose type lists only `Console`, as `game`'s does, asks only the world outside.

## 18. The whole language

These letters describe the language, and are never written in it: `e` an expression, `v` a value, `x` a placeholder, `c` a constructor, `n` a defined name, `p` a pattern, `d` a constructor as declared, `T` a type's name, `F` an effect, `r` a request, `h` a handler's case, `A` and `B` types, `X` a placeholder in a type, `E` what may be requested, `Z` a placeholder for effects. `e, …` means any number of expressions, even none, and likewise for the other letters. `| …` after a case means more cases like it, as many as needed.

**What we write**: declarations and definitions, then one expression to carry out.

    T(X, …) = d | …                  a type, with its constructors; T alone if it takes no types
    d ::= c                          a constructor holding nothing
          c(A, …)                    a constructor holding contents
    effect F = r : (A, …) → B | …    an effect, with its requests
    n : A                            declares n
    n = e                            defines n

**Expressions**

    e ::= x                          a placeholder
          c                          a constructor
          n                          a defined name
          (p, …) ↦ e | …             a function: its cases, tried in order
          e(e, …)                    an application
          r(e, …)                    a request
          handle e with h | …        a handler

    p ::= x                          matches anything
          c                          matches only itself
          c(p, …)                    matches data built by c, with contents matching p, …

    h ::= (p) ↦ e                    for the value
          (r(p, …), x) ↦ e           for a request, and the rest

**Values**

    v ::= c                          a constructor
          c(v, …)                    data
          (p, …) ↦ e | …             a function

**Types**

    A ::= T(A, …)                    a type, filled; T alone if it takes none
          X                          a placeholder in a type
          (A, …) → B ! E             a function type, and what it may request; ! E left out when it lists none
    E ::= {F, …}                     effects
          Z                          a placeholder for effects

**Steps**

    n                          becomes   what n is defined as
    ((p, …) ↦ e | …)(v, …)     becomes   the body of the first case whose patterns v, … match,
                                         with what each placeholder matched in its place
    handle v with h | …        becomes   the body of the first case for values that v matches,
                                         with v in place
    handle e with h | …        becomes   the body of the first case for the request r(v, …),
                                         with the request's inputs in place, and the rest in place of x,
                                         when the next thing to work out in e is r(v, …),
                                         and no handler inside e has a case for r

The rest is `e` with a new placeholder `y` where the request was, inside the same handler, as a function of `y`: `(y) ↦ handle … with h | …`. Steps happen in an application, first in what is applied, then in its inputs from left to right; in a request, in its inputs from left to right; in a handler, in the expression inside it. Nothing happens inside a function until it is applied.

**Checking**: one rule for each form.

    c : T(X, …)                         when T(X, …) lists c
    c : (A, …) → T(X, …)                when T(X, …) lists c(A, …)
    n : A                               when n is declared n : A, with any types in place of its placeholders
    x : A                               when its pattern gives it A
    e(e', …) : B                        when e : (A, …) → B ! E, and e', … : A, …
    r(e, …) : B                         when F lists r : (A, …) → B, and e, … : A, …
    ((p, …) ↦ e | …) : (A, …) → B ! E   when in every case the patterns fit A, …, and the body : B;
                                        and any values of types A, … match some case
    handle e with h | …                 answers every request of one effect that e may make

    x fits A           giving x : A
    c fits T(…)        when c : T(…)
    c(p, …) fits B     when c : (A, …) → B, and p, … fit A, …

What may be requested adds up from the parts, and a handler takes away the effect it answers.

**The guarantee**: a checked expression comes to a value of its type, goes on forever, or makes a request its type lists. It never gets stuck.

**The shape**: every value is data or a function, and every effect is requests and a handler. Each is made one way and used one way:

|          | made by                           | used by                          |
|----------|-----------------------------------|----------------------------------|
| data     | a constructor, holding values     | a pattern, which takes it apart  |
| function | cases, each a pattern and a body  | applying it to inputs            |
| effect   | a request                         | a handler, which answers it      |

**Reading another language through the map.** Every language is this one with some of its forms chosen, some hidden, and its own way of writing them. Python, for example:

| Python                 | on the map                                     |
|------------------------|------------------------------------------------|
| `True`, `False`, `Enum`| constructors                                   |
| `def`, `lambda`        | functions                                      |
| `if`, `match`          | cases                                          |
| tuples, `dataclass`    | constructors holding contents                  |
| lists                  | `List(X)`                                      |
| `for`, `while`         | a definition that uses itself, with a memory   |
| `x = x + 1`            | a memory: `put(succ(get()))`, its handler hidden |
| `raise`, `try`         | a request whose handler never goes on          |
| `yield`                | a request whose handler goes on later          |
| `print`, `input`       | requests the outside answers                   |
| type hints             | checking                                       |

Five forms of expression for saying, two more for doing, and a few rules for each. Everything else is a combination of them.
