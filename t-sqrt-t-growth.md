Postgame Growth Rate in Cookie Clicker is O(t\*sqrt(t))
=======================================================

...if you ignore a few details.

We frequently say that Cookie Clicker has exponential growth.
That is,
if `f(t)` is the number of cookies you have after `t` seconds,
then `f(t)` looks like `a^t` for some number `a`.
And this feels true in the beginning,
with building upgrades doubling their production,
and heavenly upgrades and minigames providing a steady source of multipliers.

But then,
several weeks later,
we hit the endgame wall.
The number of upgrades and minigames is finite,
and after we purchase all of them and achieve every achievement,
the only thing propelling us forward are the prestige levels.
At this point the magic of exponential growth disappears,
and we are left with a much slower growth rate.
But what is this growth rate, exactly?

We will first start with a simplified version of the problem.

What if I pretend that prestige gain is instantaneous?
------------------------------------------------------

Most upgrades provide a fixed boost to the CpS.
The main exceptions are the the "synergy-likes"
(all the synergies plus the "-illion fingers" series of upgrades)
and the birthday cookie,
which we will ignore for now.

In the aggregate,
all these upgrades and all buildings gives us a certain base production rate `R`.
In terms of our function `f`,
this `R` is our CpS,
i.e. `f'(t) = R`.

There are upgrades that give a variable boost;
for example, the heart biscuits from Valentine's Day can be further boosted by Selebrak,
and kitten upgrades get stronger the more achievements you have and the more milk is boosted.
But these variations by themselves are still limited,
so we can simply make our constant `R` a bit bigger to account for that.

So the only thing that can increase the cookie production is prestige.
At any point in time we can choose to reduce `f(t)` to zero
to gain a certain number `P` of prestige levels,
and increase our CpS by a factor of `1+P`.

But let us pretend that every cookie produced is instantaneously accounted for the prestige.
In game terms,
this would be the equivalent of continuously transferring cookies
from the "cookies baked this ascension" counter
to the "cookies baked all time" counter.
This is certainly faster than what we can achieve in-game,
so whatever upper bound we find in this setting also applies in-game.

The prestige level is the cube root of the total number of cookies sacrificed when ascending,
divided by 10000.
(Or you can first divide by one trillion (1e12) and then take the cube root.)
In our case,
since we are continuously sacrificing cookies,
the total number of sacrificed cookies is `f(t)`,
so the prestige level at time `t` is `cbrt(f(t)/1e12)`.
This means that the production rate `f'(t)` at time `t` is `(1+cbrt(f(t)/1e12)) * R`.
If we ignore the constants
(which do not affect the growth rate),
this gives us the differential equation `f'(t) = cbrt(f(t))`.
This equation can be solved using separation of variables,
or you can simply guess that it probably has a solution of the form `f(t) = a * t^b`
and calculate what `a` and `b` must be.
Either way,
all solutions have the form `f(t) = (t + C)^{3/2} * (2/3)^{3/2}` for some arbitrary constant C.

(A quick way of proving that all solutions have this form
is to define the function `g(t) = f(t)^{2/3}`.
Then `g'(t) = 2/3 * f(t)^{-1/3} f'(t) = 2/3`;
i.e. `g'` is a constant,
so `g(t)` must be of the form `2/3 t + C`,
whence `f(t) = g(t)^{3/2} = (2/3 t + C)^{3/2}`.)

If we ignore the constant terms,
we get `f(t) = t^{3/2} = t sqrt(t)`.

But in the actual game, prestige gain is not instantaneous!
-----------------------------------------------------------

Because the growth of `f` is polynomial
(in particular, it does not exhibit exponential growth)
we can work around that.

The key observation is that `f(t)` and `f(t/2)` have the same growth rate
(in fancy terms,
`f(t)` is big Theta of `f(t/2)`),
so we will find an ascension strategy that accumulates `g(t)` cookies
and show that it is sandwiched between `f(t)` and `f(t/2)`.

The strategy is simple: we double the time spent on each ascension.
If,
say,
we spent one day on the first ascension,
we will spend two days on the second ascension,
four days on the third ascension, and so on.

Let's say that we have last ascended at time `s`.
(For ease of calculation,
let us pretend that there is a "zeroth ascension" in which we also spent 1 day.)
Then the next ascension will happen at time `2s`;
let us compare the behavior of `g` between `s` and `2s`
with the behavior of `f` between `s/2` and `s`.
By our hypothesis,
the value of `g(s)` is at least `f(s/2)`.
The derivative of `f` between `s/2` and `s` increases by a factor of `sqrt(2)`.
Even though the derivative of `g(t)` between `s` and `2s` is stuck at `f'(s/2)`,
the interval between `s` and `2s` is twice as long as the interval between `s/2` and `s`,
so how much `g` increases between `s` and `2s`
is more than how much `f` increases between `s/2` and `s`.

In (quasi-)LaTeX notation,

    \int_s^{2s} g'(t) dt = \int_s^{2s} f'(s/2) dt
                         = 2 \int_{s/2}^s f'(s/2) dt
                        >= 2 \int_{s/2}^s f'(t)/sqrt(2)
                        >= \int_{s/2}^s f'(t) dt.

This means that `g(2s)` is at least `f(s)`.
And of course `g(t) <= f(t)`,
which means that `g(t)` also has a growth rate of `t sqrt(t)`.

What about combos?
------------------

Combos are fundamentally constant multipliers on `f'(t)`,
which does not affect the rate of growth.

More concretely,
let's say that we have a combo that achieves a multiplier of `M` on top of the "normal" CpS.
This means that the differential equation satisfied by `f` is now

    f'(t) = M cbrt(f(t))

If we apply the same procedure as we had before,
the solutions to this differential equation are of the form
`f(t) = (2M/3 * t + C)^{3/2}`,
which
(because the multiplier `M` is a constant)
has the same growth rate as `t sqrt(t)`.

It is interesting to note,
however,
that the constant `M` is inside the exponent in the general solution.
Moving it to outside the exponent gives (essentially)
the form `f(t) = M^{3/2} t^{3/2}`;
this means that,
**if we increase this multiplier by a factor of `N`,
the value of `f` will increase by a factor of `N^{3/2}`.**
As a concrete example,
if you learn to incorporate Dragon Harvests in your combos,
instead of just the multiplier of 17x gained from the buff itself
(after purchasing the Dragon fang, dropped by petting the dragon),
the compound effect of increasing prestige actually nets you a factor of `17^(3/2) =~ 70.09`.

What about the Birthday cookie?
-------------------------------

Ah, you got me.
The birthday cookie is unique in that it increases in power linearly.
This means that the differential equation actually looks like

    f'(t) = A * t * cbrt(f(t))

for some appropriate constant A.
If we ignore this constant,
the solutions to this differential equation are of the form
`f(t) = (2/3 * t^2 + C)^{3/2}`.
Again ignoring the constant this looks like `f(t) = t^3`,
which is much faster than the `t * sqrt(t)` that I promised above.

However,
even though this model is more accurate long-term,
the constant `A` above is very small,
as the birthday cookie only increases 1% _per year_.
So,
[unless you are an incredibly long-lived being that can appreciate the effect of years piling up](./README.md#sisyphus-plays-cookie-clicker),
the rate of growth you will experience is best modeled pretending that the Birthday cookie is constant,
which brings us back to `t sqrt(t)`.

But what about purchasing buildings?
------------------------------------

You got me again!... kinda.
With more cookies we can purchase more buildings,
which increases the CpS.
However the price of buildings increase exponentially;
conversely,
the CpS boost we get from them thus increase only logarithmically.
That is
(ignoring the Birthday cookie)
the differential equation on `f` looks like

    f'(t) = log(f(t)) cbrt(f(t))

...and unfortunately this differential equation has no solutions in elementary terms.
(It has solutions, but they cannot be expressed in terms of elementary functions.)
We can still get bounds on `f`:
for any constant `d > 0`,
we know that `log(x)` is smaller than `x^d`,
so a function `h` satisfying the differential equation

    h'(t) = h(t)^d cbrt(h(t)) = h(t)^{1/3 + d}

will grow faster than `f`.
The general solution to the differential equation above is

    h(t) = (t + C)^{1/(2/3 - d)} * (2/3-d)^{1/(2/3-d)}.

The crucial point is that,
if we make `d` small,
then the general solution above will get progressively closer to `t^{3/2}`.
Intuitively,
the rate of growth of `f` is just a tiny bit bigger than `t sqrt(t)`.
In formal terms,

    f(t) = t^{3/2 + o(1)}.

What about synergy upgrades? And building specials?
---------------------------------------------------

Ha, this time I got you!
For synergy upgrades,
the same reasoning applies as before,
the rate of growth just gains another `log(f(t))` factor.
And similarly for building specials;
the strength of each building special is proportional to `log(f(t))`,
and there can be up to 20 of them.
So,
the differential equation looks like

    f'(t) = log(f(t))^22 * cbrt(f(t)),

and exactly the same trick applies as before.
Hence,
albeit slightly wrong,
it is more fruitful to think of these multipliers as constants,
just like the other pieces of a combo.

I have a very clever strategy for purchasing buildings!
-------------------------------------------------------

That does not actually matter.
Your heuristic will be (at most) a small constant factor better
than the simple strategy of splitting all resources evenly.

Say you invested a fraction `r` of the cookie income in purchasing "You"s.
This is at most 20 times more than the simply strategy
of spending 1/20 of the cookies purchasing each building.
Building prices scale exponentially, gaining a factor of 1.15 each time;
this means that in `log_1.15(20) =~ 21.43` purchases
the price increases by more than a factor of 20.
That is,
your strategy purchases at most 22 more "You"s than evenly splitting resources.
When we have 100 "You"s,
that's a 22% CpS increase,
but the more "You"s we have,
the proportional CpS increase gets progressively smaller.

In fancy terms,
any building purchase strategy is within `1 + o(1)` of the strategy of evenly splitting resources.

But I'm an immortal being eternally playing Cookie Clicker and can witness `log(n)` growing!!!
----------------------------------------------------------------------------------------------

In this case,
your limitation will actually be the IEEE754 floating-point standard,
rather than imprecisions from treating building specials as constant multipliers.
[Luckily for you, I have written a survival manual precisely for this situation!](./sisyphus-plays-cookie-clicker-1-ieee754.md)

Exercise for the reader
=======================

In the `f'(t) = cbrt(f(t))` model,
gaining a factor of `N` in the cookie production
yields (after accounting for the compound effect of prestige levels)
an increase of a factor of `N^{3/2}` in the total number of cookies.
What is the increase in the `f'(t) = t cbrt(f(t))` model
(i.e. accounting for the Birthday cookie)?
