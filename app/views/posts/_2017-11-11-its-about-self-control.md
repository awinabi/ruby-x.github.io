Since i currently have no time to do actual work, i've been doing some research.

Reading about other implementations, especially transpiling ones. Opal, ruby to
javascript, and jruby, ruby to java, or jvm instructions.

## Reconsidering the madness

One needs to keep an open mind off course. "Reinventing" the wheel is not good, they
say. Off course we don't invent any wheels in IT, we just like the way that sounds,
but even building a wheel, when you can buy one, is bad enough.
And off course i have looked at using other peoples code from the beginning.

A special eye went towards the go language this time. Go has a built in assembler, i
didn't know that. Sure compilers use assembler stages, but the thing about go's
spin on it is that it is quite close to what i call the risc layer. Ie it is machine
independent and abstracts many of *real* assemblers quirks away. And also go does
not expose the full assembler spectrum , so there are ways to write assembler within
go. All very promising.

Go has closures, also very nice, and what they call escape analysis. Meaning that while
normally go will use the stack for locals, it has checks for closures and moves
variables to the heap if need be.

So many goodies. And then there is the runtime and all that code that exists already,
so the std lib would be a straight pass through, much like mri. On top one of the best
gc's i've heard about, tooling, lot's of code, interoperability and a community.

The price is off course that one (me) would have to become an expert in go. Not too
bad, but still. As a preference i naturally tend to ruby, but maybe one can devise
a way to automate the bridge somewhat. Already found a gem to make extensions in go.

And, while looking, there seems to be one or two ruby in go projects already out there.
Unfortunately interpreters :-(

## Sort of dealbreaker

Looking deeper into transpiling and using the go runtime i read about the type system.
It's a good type system i think, and go even provides reflection. So it would be
nice to use it. This would provide good interoperability with go and use the existing
facilities.

Just to scrape the alternative: One could use arrays as the basic structure to build
objects. Much in the same way MRI does. This would mean *not* using the type system,
but instead building one. Thinking of the wheels ... no, no go.

So a go type for each of what we currently have as Type. Since the current system
is built around immutable types, this seems a good match. The only glitch is that,
eg when adding an instance or method to an existing object, the type of that object
would have to change. A glitch, nothing more, just breaking the one constant static
languages are built on. But digging deep into the go code, i am relatively
certain one could deal with that.

Digging deeper i read more about the go interfaces. I really can't see a way to have
*only* specific (typed) methods or instances. I mean the current type model is about
types names and the number of slots, not typing every slot, as go. Or for methods,
the idea is to have a name and a certain amount of arguments, and specific implementations for each type of self. Not a separate implementation for each possible combination of types. This means using go's interfaces for variables and methods.

And here it comes: When using the reflect package to ensure the type safety at runtime,
go is really slow.
10+ [times slower](http://blog.burntsushi.net/type-parametric-functions-golang/)
maybe. I'm guessing it is not really their priority.

Also, from an architecture kind of viewpoint, having all those interfaces doesn't seem
good. Many small objects, basically one interface object for every object
in the system, just adds lots of load. Unnecessary, ugly.

## The conclusion

I just read about a go proposal to have int overflow panic. Too good.

But in the end, i've decided to let go go. In some ways it would seem transpiling
to C would be much easier. Use the array, bake our types, bend those pointers.
While go is definitely the much better language for working in, for transpiling into
it seems to put up more hurdles than provide help.

Having considered this, i can understand rubinius's choice of c++ much better.
The object model fits well. Given just a single slot for dynamic expansion one
could make that work. One would just have to use the c++ classes as types, not as ruby
classes. Classes are not types, not when you can modify them!

But at the end it is not even about which code you're writing, how good the fit.

It is about design, about change. To make this work (this meaning compiling a dynamic language to binary), flexibility is the key. It's not done, much is unclear, and one
must be able to change and change quickly.

Self change, just like in life, is the only real form of control. To maximise that
i didn't use metasm or llvm, and it is also the reason go will not feature in this
project. At the risk of never actually getting there, or having no users. Something
Sinatra sang comes to mind, about doing it a specific way :-)

There is still a lot to be learnt from go though, as much from the language as the
project. I find it inspiring that they moved from a c to a go compiler in a minor
version. And that what must be a major language in google has less commits than
rails. It does give hope.

PPS: Also revisited llvm (too complicated) and crystal (too complicated, bad fit in
type system) after this. Could still do rust off course, but the more i write, the
more i  hear the call of simplicity (something that a normal person can still understand)
