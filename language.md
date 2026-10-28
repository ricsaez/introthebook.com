# A language from a few parts

This language describes computation with two kinds of value: **data** and **functions**. Data can hold functions, functions can take and give back data or other functions, and these combinations can be used inside further combinations. **Types** describe which values fit where.

The rule throughout is the same: **each part of an expression can be any expression of the type its place requires**. Patterns and types are built the same way, each from its own forms.

## 1. Expressions, values, and types

An **expression** is something we write in the language. A **value** is an expression that is already finished. Computing means working an expression out, step by step. It may reach a value or continue forever.

We describe each form of the language by its general shape. A **placeholder** is a letter that stands for any part of the right kind: `e` for an expression, `v` for a value, and `A`, `B`, and `C` for types. A prime gives another placeholder of the same kind, so `e'` is another expression. Filling every placeholder in a general form gives one particular instance of it.

A **type** says what kind of value an expression can give. We write:

    e : A

This reads “e has type A.”

There is one kind of type for each kind of value:

| Type | Its values |
|---|---|
| A data type | Data made with its constructors |
| A function type | Functions with the specified inputs and output |

The simplest data type lists its values. Using `T` for the type's name and `c`, `c'`, … for the names of its values:

    T = c | c' | …

The `|` means “or.” These names are the type's **constructors**: the ways to make data of this type. For example:

    Bool = false | true

A `Bool` is `false` or `true`; nothing else is a `Bool`:

    true : Bool

We can give an expression a **name**. Using `n` for the name, the first line says its type, and the second what it stands for:

    n : A
    n = e

For example:

    answer : Bool
    answer = true

A name can stand for any expression of its declared type. A definition can use its own name, or other names defined later.

## 2. Functions and calls

A **function** says which expression to use for an input. Its type says what it accepts and what it gives back:

    A → B

This reads “given an A, give back a B.”

We make a function by listing **cases**, separated by `|`. Each case says which expression to use for one constructor:

    (c) ↦ e | (c') ↦ e' | …

The first case reads “given `c`, use `e`.” The arrow `→` describes a function's type; `↦` says what to give back. For example:

    not : Bool → Bool
    not = (true) ↦ false | (false) ↦ true

Given `true`, give back `false`; given `false`, give back `true`.

Giving a function an input is a **call**. Using `f` for a function:

    f(e)

If `f : A → B` and `e : A`, then `f(e) : B`. For example:

    not(true) : Bool

To work out a call, use the case that matches its input. We write `>>` to show a step; it is explanatory notation, not part of the language. If `f` has the case `(c) ↦ e`:

    f(c) >> e

For example:

    not(true) >> false

A case can also accept any value of the input type, by giving it a name. This name is a **variable**:

    (x) ↦ e

Within `e`, `x` stands for the value given. A call `f(v)` then steps to `e`, with each `x` in `e` replaced by `v`:

    same : Bool → Bool
    same = (b) ↦ b

    same(true) >> true

The part of a case before `↦` is its **pattern**; we use `p` for one. So far, a pattern is a constructor or a variable.

A function can take several inputs. Its type lists their types, a call lists the inputs, and each case has one pattern per input:

    (A, B, …) → C
    f(e, e', …)
    (p, p', …) ↦ e

If `f : (A, B) → C`, `e : A`, and `e' : B`, then `f(e, e') : C`. For example:

    and : (Bool, Bool) → Bool
    and = (true, b) ↦ b | (false, b) ↦ false

    and(true, false) >> false

Every possible combination of inputs must match **exactly one** case. Cases cover all possibilities without overlapping. Here, two cases cover all four pairs of Bools.

## 3. Data and patterns

A constructor can hold values, called its **contents**. In the declaration, we list their types:

    T = c(A, B, …) | …

Given contents of those types, the constructor makes data of type `T`:

    c(e, e', …) : T

when `e : A`, `e' : B`, and so on. For example:

    BoolPair = bits(Bool, Bool)

    bits(true, false) : BoolPair

`bits` keeps both contents together as one value. `bits(true, false)` is finished: a constructor holds its contents and has no cases to run.

Patterns can describe data with contents. They have three forms, which can nest:

    x                 any value; call it x
    c                 a constructor with no contents
    c(p, …)           a constructor whose contents match the patterns p, …

For example:

    left : BoolPair → Bool
    left = (bits(a, b)) ↦ a

    left(bits(true, false)) >> true

`bits(true, b)` matches pairs whose first part is `true`; `bits(a, b)` matches every pair. Each variable appears once in a case's patterns and can be used any number of times in its result.

Only data can be inspected this way. An input of function type is matched by a variable, which the result can then call. A pattern describes how data was made and contains no calls to compute. A case's result can be any expression of the required type.

A constructor's contents can include the type being defined:

    T = c | c'(T)

Starting from `c`, this gives one value after another:

    c
    c'(c)
    c'(c'(c))
    …

For example, the natural numbers:

    Nat = zero | succ(Nat)

`succ` means “one more”:

    zero                         0
    succ(zero)                   1
    succ(succ(zero))             2
    …

Each value is finite, but there is no limit on how many layers we can build. Patterns can inspect those layers: `succ(n)` matches every positive number; `succ(succ(n))` matches every number at least two.

Data types combine three shapes:

    T = c | c' | …         alternatives
    T = c(A, B, …)         contents held together
    T = c | c'(T)          another layer of the same type

## 4. Working expressions out

A call can appear wherever an expression of its type is needed, including inside another call or a constructor. If `e >> e'`, the expression around it takes the same step:

    f(e) >> f(e')
    c(e) >> c(e')

For example:

    not(not(true)) >> not(false) >> true
    bits(not(true), true) >> bits(false, true)

A name takes a step to the expression it names. If `n = e`:

    n >> e

In examples, we skip this step and keep writing the name.

To work out a call:

1. Work out the function being called, then its inputs, from left to right.
2. Find the case they match.
3. Replace the call with that case's result, replacing its variables with the values they matched.
4. Continue working out the result.

A constructor's contents are also worked out from left to right. Data is finished when its contents are values. A function is already a value: its case results wait until it is called, and only the matching case's result is used.

A function can call itself. This is **recursion**. One shape for it, for a function named `f` and using `g` for another function:

    f = (c) ↦ e | (c'(x)) ↦ g(f(x))

The second case works out `f` on `x`, one layer inside the input, and gives the result to `g`. Each call removes a layer, until the first case applies. For example:

    even : Nat → Bool
    even = (zero) ↦ true | (succ(n)) ↦ not(even(n))

    even(succ(zero))
    >> not(even(zero))
    >> not(true)
    >> false

The same shape works with more inputs:

    add : (Nat, Nat) → Nat
    add = (zero, n) ↦ n | (succ(m), n) ↦ succ(add(m, n))

    add(succ(zero), succ(zero))
    >> succ(add(zero, succ(zero)))
    >> succ(succ(zero))

This is 1 + 1 = 2. Since a definition can use names defined after it, functions can also call each other.

Nothing requires a recursive call to use a smaller input:

    f = (x) ↦ f(x)

Each step repeats the call:

    f(v) >> f(v) >> …

For example:

    loop : Bool → Bool
    loop = (b) ↦ loop(b)

    loop(true) >> loop(true) >> …

Having a type does not guarantee reaching a value. `and(false, loop(true))` also runs forever: both inputs must finish before its case is used, even if the case discards one.

Left to right is a choice. Working out the inputs in any other order would reach the same value. A different rule would change more: if a case could be used as soon as the inputs its patterns inspect are finished, `and(false, loop(true))` would become `false`. The order of steps can never change which value we reach, but it can decide whether we reach one at all.

To keep an expression from running, put it in a case that is not chosen:

    safe : Bool → Bool
    safe = (true) ↦ true | (false) ↦ loop(false)

    safe(true) >> true

## 5. Functions as values

A function type is a type, so it can fill any place a type can: an input, an output, or a constructor's contents.

    (A → B, …) → C         receives a function
    A → (B → C)            gives back a function
    T = c(A → B, …)        holds a function

A function received as an input is matched by a variable, and the result can call it:

    twice : (Bool → Bool, Bool) → Bool
    twice = (g, b) ↦ g(g(b))

    twice(not, true) >> not(not(true)) >> not(false) >> true

A function written with `↦` is a value even without a name, so it can be written anywhere an expression can, including as a case's result:

    (x) ↦ ((y) ↦ e)

For example:

    constant : Bool → (Bool → Bool)
    constant = (a) ↦ ((b) ↦ a)

    constant(true) >> (b) ↦ true

The returned function keeps the supplied value of `a`. Its own input, `b`, is a separate variable. We can call it directly:

    constant(true)(false) >> ((b) ↦ true)(false) >> true

An inner function can use variables from its surrounding case. If it introduces a variable with the same name, that name refers to the inner variable within the inner case, and replacing the outer variable stops there.

A constructor can hold a function, and a pattern can take it out again:

    Task = task(Bool → Bool, Bool)

    perform : Task → Bool
    perform = (task(g, b)) ↦ g(b)

    perform(task(not, true)) >> not(true) >> false

The types distinguish a function taking several inputs from a function returning another function:

    (A, B) → C     called as f(e, e')
    A → (B → C)    called as f(e)(e')

There is no fixed depth to these combinations: a function can receive a function that returns a function, and so on.

## 6. Types with parameters

A type can leave the types of its contents open. Using `X`, `Y`, … for **type variables**, a data type can take them as **parameters**:

    T(X, …) = c | c(A, …) | …

The contents' types `A`, … can use `X`, …. Each parameter stands for whichever type is chosen. For example:

    Pair(X, Y) = pair(X, Y)

This defines pairs for every choice of `X` and `Y`. To construct a pair, we supply the contents themselves:

    pair(true, zero) : Pair(Bool, Nat)

A name's type can use type variables too:

    first : Pair(X, Y) → X
    first = (pair(a, b)) ↦ a

The definition must work for every choice of `X` and `Y`. Each use can choose different types; within one use, the same letter always stands for the same type. This is **polymorphism**: one definition works at many types. Since the definition cannot know which type was chosen, an input whose type is a type variable is matched by a variable:

    identity : X → X
    identity = (x) ↦ x

    identity(true) >> true
    identity(zero) >> zero
    identity(not) >> not

Each use chooses the types from what it is given, or from the type its place requires. In `identity(true)`, `X` is `Bool`; in `identity(not)`, `X` is `Bool → Bool`.

Type parameters can be filled with any types, including function types and further combinations:

    Pair(Bool, Nat)              a Bool and a number
    Pair(Bool → Bool, Nat)       a function and a number
    Pair(Bool, Pair(Nat, Nat))   a Bool and a pair of numbers

The same `first` takes a stored function back out, ready to call:

    first(pair(not, zero))(true) >> not(true) >> false

## 7. Combining the pieces

A list combines alternatives, contents, another layer, and a type parameter:

    List(X) = empty | cons(X, List(X))

A list is empty, or an item followed by another list of the same kind:

    cons(true, cons(false, empty)) : List(Bool)

`map` combines that data with a function input, and applies the function to every item:

    map : (X → Y, List(X)) → List(Y)
    map = (f, empty) ↦ empty
        | (f, cons(x, rest)) ↦ cons(f(x), map(f, rest))

The empty list stays empty. For a nonempty list, transform its first item and repeat on the rest:

    map(not, cons(true, empty))
    >> cons(not(true), map(not, empty))
    >> cons(false, map(not, empty))
    >> cons(false, empty)

`X` and `Y` need not be the same: a function from numbers to Bools turns a list of numbers into a list of Bools. They can also be function types, so the same `map` can transform lists of functions.

Every possibility comes from combining these rules:

| Combine | What it allows |
|---|---|
| Several constructors | Alternatives: true or false, empty or nonempty |
| Several contents | Values held together: pairs, triples, larger records |
| Contents of the type being defined | Data of any finite size: numbers, lists, trees |
| Several cases | A different result for each way an input was made |
| Patterns inside patterns | Choices that look several layers into data |
| Calls inside expressions | One computation feeding another |
| A definition using its own name, or later names | Repetition for as long as needed |
| A function type in an input, output, or contents | Functions received, returned, or stored in data |
| Type parameters | The same definition used at different types |

Each result can be combined again. A list can hold pairs of functions; a function can return such a list; a type parameter can leave the functions' input type open.

## 8. The language in one place

Here `e` means an expression, `v` a value, `p` a pattern, `x` a variable, `n` a defined name, `c` a constructor, and `T` a data type name. For types, `X` is a type variable, and `A` and `B` stand for any types. The `…` means we can repeat that part.

    Expressions
        x                        a variable
        n                        a defined name
        c                        a constructor with no contents
        c(e, …)                  data with expressions for its contents
        e(e, …)                  a function call
        (p, …) ↦ e | …           a function

    Values
        c                        data with no contents
        c(v, …)                  data whose contents are values
        (p, …) ↦ e | …           a function whose case results wait

    Patterns
        x                        leave a value open
        c                        match a constructor with no contents
        c(p, …)                  match a constructor's contents

    Types
        X                        a type variable
        T                        a data type with no parameters
        T(A, …)                  a data type with its parameters filled
        (A, …) → B               a function type; with one input, A → B

    Named definitions
        n : A
        n = e

    Data declarations
        T = c | c(A, …) | …
        T(X, …) = c | c(A, …) | …

    Steps
        n >> e                               when n = e
        (… | (p, …) ↦ e | …)(v, …) >> e      when v, … match p, …
        e(…) >> e'(…)                        when e >> e'
        v(…, e, …) >> v(…, e', …)            when e >> e'
        c(…, e, …) >> c(…, e', …)            when e >> e'

A data declaration introduces a type and its constructors. Each constructor has the listed contents; their types may use the declaration's type parameters and the type being defined. A name's type may use type variables.

In the second step, the call becomes the result of the case whose patterns the values match, with each variable replaced by the value it matched. The function being called takes its steps first, then its inputs or a constructor's contents, from left to right: in `v(…, e, …)` and `c(…, e, …)`, everything before `e` is already a value. A value takes no steps.

Every name must be defined, and every variable must be introduced by the patterns of an enclosing case, once. Every expression must have the type its place requires. Calls supply the number of inputs the function expects; constructor expressions supply the number of contents its declaration lists. Every possible combination of input values must match exactly one case.

Together, these rules mean an expression that is not a value always has exactly one next step. Types ensure that only functions are called, and only with inputs their cases expect. Exactly one case means there is always a case to use, and never two to choose from. So working an expression out never gets stuck: it reaches a value or continues forever.

Expressions contain expressions, patterns contain patterns, and types contain types. Each can use its own forms again, with no fixed limit on nesting.

Computation uses four operations: **make** data with constructors, **choose** with cases, **combine** with calls, and **repeat** through names. Together they can express any computation.
