How high can the stock market go?
=================================

The minigame for banks is the stock market,
where you can trade goods whose price fluctuate over time.
(So it is more like a commodity market than stock market.)
Goods' prices follow a stochastic process.
Every minute,
the game runs `Game.Objects['Bank'].minigame.tick()`,
whose code can be read
in [`minigameMarket.js`](https://orteil.dashnet.org/cookieclicker/minigameMarket.js).
Near the end of this function,
there is the code
```js
    me.val=Math.max(me.val,1);
```
which sets the value of the good to 1
if it happened to fall below this threshold during this tick.
In other words,
goods' values absolutely cannot go below $1.

However, there is no explicit maximum value in the code.
But it seems clear that the goods' values cannot go up forever.
For example,
[in my thousand-year simulation](https://github.com/staticvariablejames/InsugarTrading#highlights),
the goods' values seem to never exceed $400,
with the exception of PBL who once reached $400.6.
But what if we are _impossibly lucky_?
Can we go beyond, say, $1 million?

The answer is "no",
and the rest of this text will mathematically analyze the `tick()` stochastic process
to calculate the theoretical highest value a stock market good can have.

To follow this article,
you will need to know a bit about calculus,
and to have a copy of the `tick()` algorithm
(either [Orteil's code](https://orteil.dashnet.org/cookieclicker/minigameMarket.js),
[the wiki's description](https://cookieclicker.wiki.gg/wiki/Stock_Market#Market_Behavior),
or [my C++ reimplementation](https://github.com/staticvariablejames/CookieClickerCppTools/blob/master/stock.h#L141)).

Side-note: RNG vs PRNG
----------------------

The `tick()` algorithm uses `Math.random()` to generate a number between 0 and 1 (exclusive).
Cookie Clicker uses [David Bau's seedable PRNG](https://www.davidbau.com/archives/2010/01/30/random_seeds_coded_hints_and_quintillions.html),
which will have biases due to its nature as a _pseudo_-random number generator.
So the behavior discussed in this article might never actually take place.
I will ignore these concerns and assume that `Math.random()` is a true random sequence.

The Dragon Boost
----------------

[Krumblor's Supreme Intellect](https://cookieclicker.wiki.gg/wiki/Krumblor#Auras)
can influence the stock market,
and this influence is seen directly in the code as the variable `dragonBoost`.
This variable will be 0 if Supreme Intellect is not being used,
1 if it is being used,
0.1 if it is being used through Reality Bending alone
(which gives 10% of all other auras),
and 1.1 if both Supreme Intellect and Reality Bending are being used.

Essentially,
for the duration of this article,
we may assume `dragonBoost` is 1.1.

The Mode
--------

There are fundamentally four variables affecting the goods values:
the value itself (`me.val`),
the delta (`me.d`),
and the current mode and its duration (`me.mode` and `me.dur`, respectively).
We first analyze the mode.

There are six different modes,
represented by integers between 0 and 5:
- 0 is "stable";
- 1 is "slow rise";
- 2 is "slow fall";
- 3 is "fast rise";
- 4 is "fast fall";
- 5 is "chaotic".
The "normal behavior" is that the duration is decreased by 1 every tick,
and when it reaches 0,
a new mode is selected,
and the duration is set to a number between 10 and 699 (inclusive).
However,
due to the existence of the parameter `globD`,
every tick there is a chance that the duration will be set to 0,
so goods modes can (in theory) change how often we want.

`dragonBoost` skews the duration to be lower and the new mode to be "chaotic" more often,
but the duration is always at least 10,
and all modes can be selected as the new mode
(including the current mode),
so we can ignore Krumblor's effect
and assume our impossibly lucky good stays in the mode we want for as long as we want.

The Delta
---------

The delta affects the value,
but it is not directly affected by it,
so it is the next thing we will analyze.

Fundamentally,
the delta changes according to the following algorithm:
```js
    me.d *= 0.97 + 0.01*dragonBoost;
    me.d += someRandomValue();
    if(me.value > softCap() && me.d > 0) me.d *= 0.9;
```

The soft cap (called "market cap" in the wiki) is defined to be `97 + 3*bankLevel`.
That is,
with banks level 1,
the soft cap is $100,
and it increases by $3 for each bank level.
Since we are talking about impossibly lucky goods,
we will assume that the goods' values stay above the soft cap.

The `someRandomValue()` pseudocode summarizes several lines of code.
For "chaotic" and "fast rise" goods modes
(which are the only relevant modes for this discussion)
`someRandomValue()` depends only on the current goods mode
(i.e. it does not depend on the current delta).

For the "chaotic" mode,
the delta gains (in order) a number in the `(-0.15, 0.15)` range,
then a number in the `(-5, 5)` range,
then a number in the `(-0.1, 0.1)` range,
then a number in the `(-0.26, 0.26)` range,
then a number in the `(-4.3, 4.3)` range.
So `someRandomValue()` is at most 9.81.

For the "fast rise" mode,
the delta gains (in order) a number in the `(-0.015, 0.135)` range,
then a number in the `(-5, 5)` range,
then a number in the `(-0.1, 0.1)` range,
then a number in the `(-0.26, 0.26)` range,
then a number in the `(-0.05, 0.05)` range,
then a number in the `(-0.05, 0.05)` range.
So `someRandomValue()` is at most 5.595.

(Note that the last `(-0.05, 0.05)` range only happens if the goods mode switches to fast fall,
but as mentioned above,
due to the `globD` variable the goods mode can switch back to fast rise again
at the end of `tick()`.)

So, how high can the delta value go?

In chaotic mode,
if the random values were always the highest
(where `someRandomValue() == 9.81`)
then the update of `me.d` would be equivalent to
```js
    me.d = 0.981 * me.d;
    me.d = me.d + 9.81; // The fact that this is 10 * 0.981 is a coincidence
    me.d = me.d * 0.9;
```
which is equivalent to
```js
    me.d = 0.8829 * me.d + 8.829;
```

Mathematically speaking,
if we let the values of `me.d` be `d_0, d_1, d_2, ...`
then `d_{n+1} = 0.8829 * d_n + 8.829`,
and regardless of the initial value,
this sequence has limit
```
    8.829 / (1-0.8829) = 88290/1171 =~ 75.397.
```

Similarly,
in the fast rise mode,
getting the highest value of `someRandomValue()` is equivalent to
```js
    me.d = 0.8829 * me.d + 5.0355;
```
and this sequence has limit
```
    5.595 / (1-0.8829) =~ 47.779.
```

Mathematically speaking,
we have imposed an upper bound of 88290/1171 on the delta.
Furthermore,
this bound is tight,
in the sense that there are sequences of random numbers
which get us arbitrarily close to this bound.

The Value
---------

Now we play the same game,
but with `me.val` instead.
This time,
`tick()` can be distilled to the following.

```js
    if(me.mode == 'fast rise') {
        me.val += 5 * Math.random();
    }
    me.val = 0.99 * me.val + 0.01 * restingValue();
    me.val += someOtherRandomValue();
    me.val += me.d;
```

The resting value is defined as `10 * me.id + bankLevel + 9`,
where the id is 0 for CRL, 1 for CHC and so on.
The game tries to push `me.val` towards the resting value,
so the average value of the goods ends up being vaguely close to the resting value
but not equal to it.

For the "chaotic" mode,
the value increases (in order)
by a number in the `(-1-7*delta, 1+7*delta)` range,
then a number in the `(-8, 8)` range,
then a number in the `(-3, 3)` range,
then a number in the `(-1.5, 1.5)` range,
then a number in the `(-10.5, 10.5)` range,
then a number in the `(-5, 5)` range.
So `someOtherRandomValue()` is at most `29 + 7*delta`.

For the "fast rise" mode,
after the weighted averaging with `restingValue()`,
the value increases (in order)
by a number in the `(-1-7*delta, 1+7*delta)` range,
then a number in the `(-8, 8)` range,
then a number in the `(-3, 3)` range,
then a number in the `(-1.5, 1.5)` range,
then a number in the `(-10.5, 10.5)` range,
then a number in the `(-7, 3)` range,
then a number in the `(-3, 7)` range,
So `someOtherRandomValue()` is at most `34 + 7*delta`.

(As before, the last `(-3, 7)` range only happens if the goods mode switches to fast fall.)

We can again apply the same logic we did for calculating the highest value of delta.
Assuming we always get the highest possible value,
the code which updates `me.val` is equivalent to
```js
    me.val = 0.99*me.val + 0.01*restingValue() + 29 + 8*delta;
```
if the mode is "chaotic",
and for the "fast rise" mode is
```js
    me.val = 0.99*me.val + 0.01*restingValue() + 38.95 + 8*delta;
```
(included the value added before the weighted averaging with `restingValue()`.)

The highest of these two is, of course, the "fast rise" mode.
If we combine this with the upper bound of 75.397 for delta obtained above,
we get the following expression:
```js
    me.val = 0.99*me.val + 0.01*restingValue() + 642.126772,
```
and the same limit argument as before gives us the upper bound
```
    (0.01* restingValue() + 642.12677)/(1-0.99) =~ restingValue() + 64212.6772.
```

In other words,
for any reasonable bank levels
(below 500),
**it is mathematically impossible for a goods' value to go above $65k.**

The bound is not tight
----------------------

Notice the conundrum, though:
the highest expression of `me.val` is obtained in the "fast rise" mode,
but the upper bound of `me.d` of about 75.397 is obtained in the "chaotic" mode.
This means that the upper bound calculated above can not be obtained this way.

If we repeat the limit calculation with value update for the chaotic mode,
we get the value
```
    restingValue() + 63217.6772.
```
This value is the upper bound assuming the good is always in the chaotic mode,
and _this_ bound is tight.
So we know that,
in theory,
goods' values can go as close as we want to `restingValue() + 63217.6772`;
i.e. **it is theoretically possible for a good to reach the value $63.2k.**

Open Problems ("exercises for the reader")
==========================================

The upper bound of $64.2k is not attainable because the two "components"
(delta and value)
get the highest boosts in different goods modes.
This means that we can improve on that upper bound.

And when computing the upper bound for delta,
we made the assumption that the goods value would always be above the market soft cap,
which may not be true if the bank level is "too high".
For example,
if the bank level is 50k,
then the soft cap is 150 097,
which is clearly above the calculated upper bound of `restingValue() + 64.2k`.
(The resting value would be about $50k in this case.)
Under these conditions,
the delta may go higher,
and so does the goods values.
