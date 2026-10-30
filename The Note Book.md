    e ::= x                     placeholder
          (x, …) ↦ e            function, with body e
          e(e, …)               application

    v ::= (x, …) ↦ e            value

    ((x, …) ↦ e)(v, …)   becomes   e[x := v, …]       if as many v as x
    e(…)                 becomes   e'(…)              if e becomes e'
    v(…, e, …)           becomes   v(…, e', …)        if e becomes e', and all before e are values

    where
      ::=              the forms, one per line
      e, e', x, v      any expression, placeholder (a name, such as a), value; not part of the language
      x, …             any number of x, even none; likewise e, … and v, …
      … alone          any number of e, even none
      letters, …       independent within a form; the same on both sides of a rule
      (x, …) ↦ e       the x, … all different; the body reaches as far as it can
      ( )              around an applied function: grouping only, and the rules work on the grouping
      x used in e      belongs to the nearest function around it that takes x; in what we write, there must be one
      e[x := v, …]     each v, in order, in place of the uses of its x that belong to this function
      becomes          one step; the three rules above are the only ones
