David Bau's `seedrandom`
========================

(This is a short article talking about the RNG used by Cookie Clicker.
It assumes familiarity with RNGs;
[I have written a primer on RNGs](https://github.com/staticvariablejames/ChooseYourOwnLump#randomness-and-planners)
in the manual for my mod Choose Your Own Lump.)

Cookie Clicker has several instances of seeded RNG,
but JavaScript notoriously does not have a native method for seeding `Math.random()`.
Cookie Clicker thus has to use a third-party library for that,
and the one chosen by Orteil is David Bau's `seedrandom`.
His random number generator is based on the RC4 engine;
more details can be seen in his blog post:
<https://davidbau.com/archives/2010/01/30/random_seeds_coded_hints_and_quintillions.html>

The most up-to-date version of his library
(at the time I've written this text)
is 3.0.5, from 2019-09-14,
but Cookie Clicker uses version 2.2.
Specifically,
the line 793 of <https://orteil.dashnet.org/cookieclicker/main.js>
is a copy of <https://cdnjs.cloudflare.com/ajax/libs/seedrandom/2.2/seedrandom.min.js>.
The unminified version can be found in <https://cdnjs.cloudflare.com/ajax/libs/seedrandom/2.2/seedrandom.js>,
and it contains some documentation about how it works.


Advice Summary
--------------

Write your code like this if you want to use repeatable RNG.
```javascript
    let currentRNG = Math.random;
    Math.seedrandom(Game.seed + "some string unique to this function");
    // The line above overwrites Math.random with a brand new function. We can then take it out:
    let myRNG = Math.random;
    Math.random = currentRNG;
    // Now call myRNG() to generate random numbers between 0 and 1
    let randomPercentage = myRNG() * 100; // etc
```


In Detail
---------

David Bau's library adds the function `seedrandom` to the `Math` object.
There are two ways of using this function:
- `Math.seedrandom("some string")` or `Math.seedrandom(someNumber)`.
  This replaces the current `Math.random` function with a new function
  that will generate random numbers based on the given seed.
  - More specifically,
    it constructs a seed for an RNG engine based on the given argument,
    so you may think of the given argument as the seed itself.
- `Math.seedrandom()`.
  This also replaces the current `Math.random` function,
  but it uses a pool of entropy maintained internally by `seedrandom` to generate a seed.
  Effectively,
  this uses a random, unpredictable seed to construct the RNG.
- There is technically a third way of calling this function:
  `Math.seedrandom("some string", true)`.
  In this case it will use both the given string and the internal entropy pool to generate the seed.
  You will never use this in your code.
  (I'm not even sure what is the use case for this function;
  perhaps it is to give yet another source of entropy for a `Math.seedrandom()` style call,
  in case you're trying to do cryptography with this library?)

The first call makes `Math.random()` to behave predictably
and the second call makes it behave unpredictably again,
so we can use the first call to set up a portion of the code to use seeded RNG,
and then use the second call to "undo" this and resume the unpredictable generation of random numbers.
Orteil's code has plenty of examples of this type of usage.
For example,
`Game.Upgrades['Mice clicking mice'].descFunc.toString()` reads

```javascript
    Math.seedrandom(Game.seed+'-blasphemouse');
    if(Math.random()<0.3) {
        Math.seedrandom();
        return this.desc+'<q>Absolutely blasphemouse!</q>';
    } else {
        Math.seedrandom();
        return this.desc+'<q>Absolutely blasphemous!</q>';
    }
```

Because calling `Math.seedrandom` ovewrites `Math.random` with a brand new closure,
we could instead store the old RNG and restore it afterwards.
Something like
```javascript
    let oldRNG = Math.random;
    Math.seedrandom(Game.seed + '-blasphemouse');
    // ...use Math.random(), then at the end:
    Math.random = oldRNG;
```
This eliminates the need for another `Math.seedrandom` call.

Both usage patterns require the programmer to remember to restore `Math.random`,
either directly, as in the second example,
or by creating a brand-new unpredictable RNG, as in the first example.
Notoriously,
in the code that determines clone names
(which kicks in when hovering clones in the building display for the building "You")
the second call is missing,
which leaks the RNG state to other portions of the game.
Abusing this bug is known as CMUM ("Clone Missing Unseed Manipulation").

But you can avoid this problem by restoring `Math.random` immediately.
Specifically,
`Math.seedrandom` replaces `Math.random` with a closure that makes no reference to any global variable,
so we can simply pick that function and use it directly.
Something like
```javascript
    let currentRNG = Math.random;
    Math.seedrandom(Game.seed + '-blasphemouse');
    let myRNG = Math.random;
    Math.random = currentRNG;
    
    // Now we use Math.random
    if(myRNG() < 0.3) {
        return this.desc+'<q>Absolutely blasphemouse!</q>';
    } else {
        return this.desc+'<q>Absolutely blasphemous!</q>';
    }
    // No need to reset anything at the end
    // as we have already restored Math.random in the beginning of the code.
```
(Technically,
this usage is not explicitly documented in `seedrandom.js`,
but it works.)


Caveat
------

This trick creates a local random number generator without affecting the global `Math.random`.
If your code relies on some other code that calls `Math.random()`,
it will not be seeded anymore.
This is most noticeable when use the Cookie Clicker functions `choose` and `randomFloor`,
which do use `Math.random()` underneath.

In your own code,
you can solve this by adding the RNG as a parameter to the function.
For example,
`choose` could be reimplemented as
```javascript
    function choose(rng, arr) {
        return arr[Math.floor(rng() * arr.length)];
    }
```
Then you could call `choose(myRNG, someArray)` in your code if you want the RNG to be seeded,
and `choose(Math.random, someArray)` otherwise.
