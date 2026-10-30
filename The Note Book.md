    e ::= x                 a name
          (x, …) ↦ e        a function
          e(e, …)           an application

    v ::= (x, …) ↦ e        a value

    A placeholder followed by , … stands for any number of them, even none.
    A … on its own stands for the other parts, unchanged.
    A body reaches as far as it can. Parentheses only show grouping, and the rules work on the grouping.

    The names a function takes are all different.
    A use of a name belongs to the nearest function around it that takes that name, and every use must belong to one.

    ((x, …) ↦ e)(v, …)   becomes   e[x := v, …]     one v for each x, matched in order
    e(…)                 becomes   e'(…)            when e becomes e'
    v(…, e, …)           becomes   v(…, e', …)      when e becomes e', and everything before e is a value

    e' is what e becomes in one step.
    e[x := v, …] puts each v at the uses of its x that belong to this function.
