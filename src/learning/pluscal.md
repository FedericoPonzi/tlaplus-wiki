# Learning Pluscal
PlusCal is an algorithm language—a language for writing and debugging algorithms.  It is especially good for algorithms to be implemented with multi-threaded code.  Instead of being compiled into code, a PlusCal algorithm is translated into a TLA+ specification.  An algorithm written in PlusCal is debugged using the TLA+ tools—mainly the TLC model checker.  Correctness of the algorithm can also be proved with the TLAPS proof system, but that requires a lot of hard work and is seldom done. 

The easiest way to learn pluscal, is to read the Pluscal tutorial by Lamport. Free, and readable online; it can be found here: https://lamport.azurewebsites.net/tla/tutorial/contents.html

## C-style vs P-Style

Pluscal comes into two flavors: C-Style and P-Style. For a more complete overview of Pluscal, there is a manual available for each style: [c-manual.pdf](https://lamport.azurewebsites.net/tla/c-manual.pdf), [p-style.pdf](https://lamport.azurewebsites.net/tla/p-manual.pdf).

To have a taste of each style:
```
P-Syntax
---- MODULE Playground ----
EXTENDS TLC, Naturals

(*--algorithm Playground
    variable x = 0, y = 0;
begin
    while x > 0 do
        if y > 0 then y := y-1;
                    x := x-1
                else x := x-2
        end if
    end while;
    print y;
end algorithm; 
*)
====
```

```
C-Syntax style
---- MODULE Playground ----
EXTENDS TLC, Naturals
(*--algorithm Playground {
    variables x = 0, y = 0; {
        while (x > 0) { 
            if (y > 0) { 
                y := y-1;
                x := x-1;
            } else x := x-2 
        };
        print y;
    }
}*)
====
```
