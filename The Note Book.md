    e ::= x                 a placeholder: a name, such as a
          (x, …) ↦ e        a function, whose body is e
          e(e, …)           an application

    v ::= (x, …) ↦ e        a value

    ::= lists the forms of e and of v, one per line.
    e, x and v are not part of the language: they stand for any expression, placeholder and value.
    In a form, each letter stands for any piece of its kind, independently. In a rule, a letter written more than once stands for the same thing each time.
    … stands for any number of things, even none: more of the kind just before it, or else any expressions. A … written on both sides of a rule stands for the same things on both.
    A body reaches as far as it can, so an applied function is written in parentheses. Such parentheses only show grouping, and the rules work on the grouping.

    The placeholders a function takes are all different.
    A use of a placeholder belongs to the nearest function around it that takes that placeholder, and every use must belong to one.

    ((x, …) ↦ e)(v, …)   becomes   e[x := v, …]     one v for each x, matched in order
    e(…)                 becomes   e'(…)            when e becomes e'
    v(…, e, …)           becomes   v(…, e', …)      when e becomes e', and everything before e is a value

    These are the only steps. e' is what e becomes in one step.
    e[x := v, …] puts each v in place of the uses of its x that belong to this function.
