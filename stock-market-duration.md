The Stock Market Duration is a Lie
==================================

Besides the stock value, mode and delta,
stock market goods have a duration attribute that ticks down every tick.
Once it reaches zero,
the stock mode of the good is changed,
and a new value for the duration is selected.

Before Cookie Clicker 2.048,
the new duration was chosen via
```js
    me.dur=Math.floor(10+Math.random()*(990-200*dragonBoost));
```
i.e. a random number between 10 and 999, if no dragon aura was present.
(`dragonBoost` is 0 without dragon auras,
1 with Supreme Intellect active,
0.1 with Reality Bending active,
and 1.1 with both auras active.)
This gives us an easy-to-calculate average of 504.5,
or roughly once every 8.5 hours.

But Cookie Clicker 2.047 (one of the beta versions for 2.048)
introduced the variables `globP` and `globD` to `Game.Objects['Bank'].minigame.tick`,
making "the bank minigame flow a little more exciting".
Since then,
the duration attribute has been 98.3% _a lie_.

`globP` and `globD`
-------------------

Since Cookie Clicker 2.048,
the function `Game.Objects['Bank'].minigame.tick` looks like this:
```javascript
    M.tick = function() {
        var dragonBoost=Game.auraMult('Supreme Intellect');
        var globD = 0;
        if(Math.random() < 0.1+0.1*dragonBoost) {
            globD=(Math.random()-0.5)*2;
        }
        var globP = Math.random();
        for(each good) {
            var me = current good;
            ... // Adjust me.d and me.val according to me.mode
            if(globD != 0 && Math.random() < globP) {
                ... // Adjust me.d and me.val independent of me.mode
                de.dur = 0; // <- the crux of the issue
            }
            ... // More adjustments
            me.dur--;
            if(me.dur <= 0) {
                me.dur = Math.floor(10+Math.random()*(690-200*dragonBoost));
                ... // choose new mode
            }
        }
    }
```

The big culprit here is the `me.dur = 0` line,
which forces the good to undergo a mode change.

What is the probability that it happens?
Let us first focus on the analysis without Supreme Intellect
(so that `dragonBoost == 0`).

We first need `globD` to be nonzero.
With 10% probability,
`globD` is set to a random number between -1 and 1.
The probability that this number is exactly 0 is negligible,
so we have 10% probability of passing this check.

Then we need `Math.random()` to be smaller than `globP`.
This variable is also set to `Math.random()`,
a random variable between 0 and 1.
Intuitively,
since these two random variables are continuous, independent, and identically distributed,
the first being bigger than the second has exactly the same probability
as the second being bigger than the first;
so this check will be passed with 50% probability.
(Formally,
on the real square `[0, 1) x [0, 1)`,
the measure of the points `(x, y)` where `x < y` is exactly 1/2,
which is the probability we want.)

Hence the probability that the stock mode will be forcibly changed by `globP` and `globD` is 5%,
which averages to once in every 20 minutes.
This is a much faster rate than the once-every-six-hours
which would happen from just decrementing the duration.

In fact,
it is _rare_ that decrementing the duration is the cause for a stock market mode change.
If the duration is `d`,
then the the probability that we fail this 5% probability in all ticks is `0.95^d`.
The average for the 690 values of `d` between 10 and 699
can be calculated (using the geometric progression formula)
to be exactly
```
    (0.95^10 - 0.95^700)/(1 - 0.95)/690 =~ 0.01735469389096749713.
```
In other words,
in around 98.3% of the time that a stock market changes mode,
it was due to `globP` and `globD`,
rather than from the duration attribute naturally ticking down once every minute.

If we use Supreme Intellect (so that `dragonBoost == 1.0`),
then the probability that `globD` is nonzero raises to 20%,
so now both checks pass with 10% probability.
Hence for duration `d`
the probability that the mode is not changed by `globP` and `globD` is `0.9^d`,
and the average over the 490 values of `d` beween 10 and 499 is
```
    (0.9^10 - 0.9^500)/(1 - 0.9)/490 =~ 0.00711588653265306122.
```
Hence 99.29% of the time the change is due to `globP` and `globD`.

If both Supreme Intellect and Reality Bending are used
(so that `dragonBoost = 1.1`)
then both checks pass with 10.5% probability,
so the chance of `globP` and `globD` changing the stock mode raises a bit, to 99.33%.
And if only Reality Bending is used
(so that `dragonBoost = 0.1`)
then both checks pass with 5.5% probability,
this chance is 98.46%.

Exact Average Duration
----------------------

If `globP` and `globD` were always zero,
the average duration would be determined by the lines
```js
    me.dur--;
    if(me.dur <= 0) {
        me.dur = Math.floor(10+Math.random()*(690-200*dragonBoost));
    }
```
which would be 354.5 ticks without dragon auras,
or (because the stock market ticks once per minute)
roughly once every 6 hours.

But as we have seen above,
98.3% of the time the stock mode changes due to `globP` and `globD` being nonzero.
If `globP` and `globD` were the sole reason for mode changes,
the durations would follow a [geometric distribution](https://en.wikipedia.org/wiki/Geometric_distribution)
whose average is `1/0.05 = 20`.
In fact,
this is a good approximation to the actual value,
because,
intuitively,
98.3% of the time this is the reason why the stock mode has changed.

To compute the exact value we can proceed as follows.
Let us say that the duration is `d`.
The expected number of ticks until switching modes is
```
    E[number of ticks until mode change]
        = E[number of ticks | globP and globD changed the mode] * P(globP and globD changed the mode)
          + E[number of ticks | globP and globD did not change the mode] * P(globP and globD did not change the mode)
```
The second `E[]` is exactly `d`, by definition;
and the second `P` is `0.95^d`.

The first `E[]` is the same as the expected value of a geometrically-distributed random variable
with probability of success `p = 0.05`,
conditioned on this success happening in the first `d` tries.
The geometric distribution "has no memory",
so the expectation of the complementary condition (first `d` tries are failures)
is just `d` plus the un-conditioned expected value.
In other words,
```
    E[number of tries until first success | first d tries are failures]
        = d + E[number of tries until first success]
        = d + 1/(1-0.95) = d + 20.
```
The expectation on the number of tries until first success also decomposes as
```
    20 = E[number of tries until first success]
       = E[number of tries until first success | succeed within d tries] * P(succeeed within d tries)
         + E[number of tries until first success | first d tries are failures] * P(first d tries are failures)
       = E[number of tries until first success | succeed within d tries] * (1 - 0.95^d) + (d+20) * 0.95^d.
```
Rearranging gives
```
    E[number of ticks until mode change | globP and globD change the mode]
        = E[number of tries until first success | succeed within d tries]
        = (20 - (d+20) * 0.95^d)/(1-0.95^d).
```
Plugging back in the first equation now gives
```
    E[number of ticks until mode change]
        = (20 - (d+20) * 0.95^d)/(1-0.95^d) * (1-0.95^d) + d*0.95^d
        = 20 - 20 * 0.95^d.
```
(Alternatively,
this can be calculated directly from the formula for the expected value
using the formula for the [arithmetico-geometric sequence](https://en.wikipedia.org/wiki/Arithmetico-geometric_sequence#Partial_sums).)

The average for the 690 values of `d` between 10 and 699 is thus
```
    20 - 20 * (0.95^10 - 0.95^700)/(1-0.95)/690 =~ 19.65290612218065005725,
```
which is a bit shorter than our estimate of 20 ticks but by less than 2%.

If we repeat this calculations with Supreme Intellect we get approximately 9.9288,
which is even closer to our estimate of 10.
(With Reality Bending the number is about 17.9,
and with both auras this number is about 9.46.)
