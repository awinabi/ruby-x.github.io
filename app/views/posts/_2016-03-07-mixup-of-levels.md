Writing Soml helped a lot to separate the levels, or phases of the ruby compilation process. Helped
me that is, to plan the ruby compiler.

But off course i had not written the ruby compiler, i have only
[planned](https://dancinglightning.gitbooks.io/the-object-machine/content/object/dynamic_types.html)
how the dynamic nature could be implemented, using soml. In very short summary, the plan was to
extend somls feature with esoteric multi-return features and use that to jump around different
implementation when types change.

## The benefit of communication

But first a thanks. When i was in the US, i talked to quite a few people about my plans. Everything
helped, but special thanks goes to Caleb for pointing out two issues.

The simpler one is that what i had named Layout, is usually called Type. I have changed the code
and docs now and must admit it is a better name.

The other thing Caleb was right about is that Soml is what is called an intermediate representation.
This rubbed a little, especially since i had just moved away from a purely intermediate
representation to an intermediate language. But still, we'll see below that the language is not
enough to solve the dynamic issues. I have already created an equivalent intermediate
representation (to the soml ast) and will probably let go of the language completely, in time.

So thanks to Caleb, and a thumbs up for anyone else reading, to **make contact**

## The hierarchy of languages

It seemed like such a good idea. Just like third level languages are compiled down to second (ie c
to assembler), and second is compiled to first (ie assembler to binary),  so fourth level would
get compiled down to third. Such a nice clean world, very appealing.

Until i started working on the details. Specifically how the type (of almost anything) would change
in a statically typed language. And in short, I ran into yet another wall.

So back it is to using an intermediate representation. Alas, at least it is a working one, so down
from there to executable, it is know to work.

## Cross function jumps

Let's call a method something akin to what ruby has. It's bound to a type, has a name and arguments.
But both return types and argument types are not specified. Then function could be a specific
implementation of that method, specific to a certain set of types for the arguments. The return type  
is still not fixed.

A compiler can generate all possible functions for a method as the set of basic types is small. Or
it could be a little cleverer and generate stubs and generate the actual functions on demand, as
probably only a fraction of the theoretical possibilities will be needed.

Now, if we have an assignment, say to an argument, from a method call, the type of the variable
may have to change according to the return type.
So the return will be to different addresses (think of it as an if) and so in each branch,
code can be inserted to change the type. But that makes the rest of the function behave wrongly as
it assumes the type before the change.

And this is where the cross function jumps come. Which is also the reason this can not be expressed
in a language. The code then needs to jump to the same place, in a different function.

The function can be pre-compiled or compiled on demand at that point. All that matters is that the
logic of the function being jumped to is the same as where the jump comes from. And this is
guaranteed by the fact that both function are generated from the same (untyped ruby) source code.

## Next steps

So what's left to do here: There is the little matter of implementing this plan.

Maybe it leads to another wall, maybe this is it. Fingers crossed.
