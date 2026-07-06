Sisyphus Plays Cookie Clicker II: Risk-free Stocks
==================================================

(This is the second of a series of articles investigating the limits of Cookie Clicker.
In this article we investigate how many cookies we can obtain without ascending
closing the game,
or abusing the Grimoire spell Gambler's Fever Dream.)

[In the previous article](./sisyphus-plays-cookie-clicker-1-ieee754.md),
Sisyphus was tasked with playing Cookie Clicker
and learnt how his worst enemy will be the IEEE754 floating-point specification.

We ended that article with Sisyphus frustrated he cannot simply click his way till `10^303` cookies,
so he must actually interact with other parts of the game.


Buildings and Upgrades
----------------------

Sisyphus purchases his first cursor.
Cursors give 0.1 cookies per second,
and Sisyphus knows that gaining 1 cookie or fewer won't make the counter go past `2^53`
(which is about 9.007 quadrillion cookies)
so he goes ahead and purchases ten more cursors.
As we saw in the previous article,
the first representable number after `2^53` is `2^53+2`,
so by getting 1.1 cookies the game rounds that amount to `2^53`.

But disappointingly Sisyphus is still stuck to `2^53`!
This is a quirk of Cookie Clicker.
The game increases the amount of cookies owned smoothly.
Right now Sisyphus is gaining 1.1 cookies per second (CpS),
but as the game runs at 30 frames per second,
it awards only 1.1/30 cookies per tick.
As we have `2^53` cookies,
in each frame the cookie amount again rounds down to `2^53`.

(In the game code,
this happens inside `Game.Logic`.
The game is hardcoded to run at 30 frames per second,
but there is experimental code to handle higher framerates,
which would further reduce the amount of cookies earned per frame.)

With the 9 quadrillion cookies earned just by clicking,
Sisyphus can purchase 229 cursors.
This brings down his cookie bank to just above 1 quadrillion,
so he can click the big cookie a few quadrilion times more
to again bring the total to 9 quadrillion.
Then he can purchase another 5 cursors,
and so on,
until the cost of Cursor 245 becomes 9.691 quadrillion cookies,
finally becoming unreachable by Sisyphus.
This process highlights how Sisyphus will go about his purchases:
if he can reach a certain number of cookies,
he will be able to reach that number of cookies again,
for he has all the time of an eternity to click cookies.
Hence **the number of each building that Sisyphus can reach
is entirely determined by the point at which a single unit is too expensive**,
and by "too expensive" we mean that it is more than the threshold of cookies that Sisyphus can reach.

Unfortunately for him,
244 cursors produce 24.4 cookies per second,
or 24.4/30 cookies per frame;
hence no more cookies are awarded.
Thankfully,
Sisyphus unlocks upgrades for the cursors.
"Renforced index finger", "Carpal tunnel prevention cream" and "Ambidextrous"
each double the productivity of each cursor,
so now Sisyphus' 244 cursors produce 195.2 cookies per second,
which amounts to about 6.5 cookies per frame.
Now Sisyphus can go beyond `2^53` cookies;
in fact,
we only get in trouble when we reach `2^56` cookies,
because the floating-point numbers between `2^56` and `2^57` are spaced by 16
(which is `2^56/2^52`).

In general,
if Sisyphus can get `c` cookies in a single go,
his cookie count only gets stuck once the spacing between representable numbers
is `2*c` or more.
Hence,
**to calculate how many cookies Sisyphus can accumulate,
multiply `c` by `2^53` and round up to the next power of 2**.
Then Sisyphus can purchase everything whose cost is below this threshold
(because he can simply grind back to this threshold).
This increases `c`,
which further increases the threshold,
and so on.

With `2^56` cookies,
Sisyphus can purchase 8 Chancemakers;
their CpS is enough to get us to `2^86` cookies (77 septillion),
which affords us Cortex Bakers,
and finally You, the last building in the game.
We continue repeating this task of purchasing more buildings,
which raises the CpS,
which brings us to the next threshold.
Upgrades helps us too:
building upgrades double the CpS of each individual building,
and cookie upgrades grant us simple CpS boosts.
Seasonal upgrades also help.

Using just buildings,
sisyphus easily gets to decillion cookies,
and starts the search for more multipliers.


Building Levels and the Birthday Cookie
---------------------------------------

Each building level grants Sisyphus +1% production for that building.
We have seen in the previous article that building levels are capped at `2^53`,
and since we need one lump for the first level, two for the second, three for the third and so on,
we need `(2^53+1)*2^53 / 2 = 2^105+2^52` lumps to fully upgrade a building.
That's 40.5 nonillion sugar lumps per building,
or 770 nonillion to fully upgrade all buildings.
(Sisyphus explicitly does not upgrade the Wizard Towers,
because leveling it up too much negatively affects the Grimoire.)
Even if we harvested one lump per second,
that'd still require septillions of years to complete.

The Birthday Cookie is a cookie upgrade that gives 1% CpS boost
per year that the game existed since the game was released.
For example,
at the moment that this article was written,
purchasing the birthday cookie gives a 12% boost in CpS.
However:
- The calculation of the boost given by the cookie is computed when the game starts up.
  Hence,
  if Sisyphus wants his Birthday cookie to give boosts beyond +12%,
  he has to close his game at some point.
- The calculation starts failing after midnight of September 13th of the year 275760.
  This is because the game uses JavaScript's `Date` objects
  to calculate how many years have passed since the birth day of Cookie Clicker,
  and `Date` objects do not handle dates past September 14th, 275760.
  - This is because `Date` objects internally track the number of milliseconds since
    January 1st, 1970 (the Unix epoch),
    and this limit date corresponds to `86400 * 10^11` milliseconds,
    i.e. 100 million days.
    This number (8.64 quadrillion) is close to the number 2^53 (about 9.007 quadrillion),
    after which millisecond precision becomes impossible.
- The calculation failure is _catastrophic_.
  `new Date(86400 * 10**11 + 1)` is an invalid date,
  and any calculation performed with it becomes `NaN`.
  So the boost given by the birthday cookie becomes `NaN`,
  which irreversibly corrupts the save file.

Hence,
if Sysiphus wants to purchase the birthday cookie,
then he absolutely must not close his game after the year 275760.
He must keep his computer continuously powered and functioning and without crashing
for, at the absolute minimum,
_septillions of years_.
Any save file with the birthday cookie purchased will corrupt the game
if loaded beyond the year 275760.

Sisyphus does have a lifeline:
worst comes to happen,
he can simply wipe his save file and start anew,
but this time without purchasing the birthday cookie at all.
So our hero decides to challenge fate and purchases the upgrade on Friday, September 12th, 275760,
getting the maximum boost of 273747%.
And nonillions of years later,
all buildings (except Wizard Towers) have `2^53` levels.
This is enough to get us to vigintillion cookies.


Wrinklers as Reservoirs
-----------------------

The next step in Sisyphus journey is the Grandmapocalypse,
and with it,
the wrinklers.
Wrinkler ambergris is quickly grabbed by our hero,
who now has to study how wrinklers suck cookies.

With a single wrinkler,
every game tick that wrinkler will suck 5% of what would be gained by Sisyphus.
These cookies are stored in the wrinkler,
which
(as every number in JavaScript)
is an IEEE754 floating-point number.
Every game tick that number increases by `0.05 * CpS/30`
(because Cookie Clicker's frame rate is 30),
so after quadrillions of years
this single wrinkler will converge to either 1/16 or 1/32 of what we have in bank.
(The wrinkler gains, per tick, 1/20 of what Sisyphus would gain;
because 1/20 is not a power of 2,
the wrinkler gain could round to 1/16 or 1/32,
depending on how far the normal gain is to the next power of two.)

But the important thing here is that,
once the wrinkler is popped,
**Sisyphus gains all those cookies at once.**
Even in the worst-case scenario where the cookies converged to `bank/32`,
Sisyphus can repeat this process over and over again
until `bank/32` becomes the precision limit.
This happens at `2^53` times the increase,
i.e. we increase our cookies by `2^48 * bank`.

In a sense,
wrinklers act as a cookie reservoir,
which pretty much multiply our cookie limit by `2^53`.
We will see this "reservoir effect" twice more in this series of articles.

There are a few more wrinkles (hah) to this calculation.
The first is what happens if there are multple wrinklers.
With two wrinklers,
instead of each wrinkler sucking 5% of the CpS (for a total of 10%),
actually each wrinkler sucks 10% of the CpS.
The actual reduction in CpS gain is still just 10%;
hence normal players effectively experience an increase of 10% of CpS just by having two wrinklers.
Sisyphus is operating at the extremes of IEEE754 floating-point numbers,
so he actually experiences a much larger boost:
withering 10% of CpS instead of just 5% doubles the cap
on how many cookies can be stored in a single wrinkler.
In general,
with `n` wrinklers,
each wrinkler suck `n/20` of the CpS;
for example,
with a ring of 10 wrinklers
(the maximum that Sisyphus can get, as he cannot ascend)
the wrinklers digest 50% of the CpS.
This digestion rate can be further boosted by the garden plant Wrinklegill,
increasing that number by 45% with a full garden.
- The dragon aura Dragon Guts also boosts the digestion rate,
  but again Sisyphus cannot train a dragon,
  for he cannot ascend to purchase the "How to bake your dragon" heavenly upgrade.
- Alternating rows of nursetulips and wrinklegills
  increases the digestion rate by about 64.026%,
  instead of just 45%,
  but the nursetulips pretty much halve the normal CpS,
  so how much each wrinkler digets per game tick is actually reduced.
- The maximum (reasonable) digestion rate achievable by Sisyphus is thus 72.5%.
  With Dragon Guts and the heavenly upgrade Elder spice,
  we do get to 100% CpS withered,
  but that's not accessible for Sisyphus.

The second wrinkler is the bonuses that are applied after the wrinkler is popped.
By default the game gives a 10% bonus to this number
(i.e. there's a bonus multiplier of 1.1).
The heavenly upgrade Sacrilegious corruption multiplies this by 1.05,
the Easter egg Wrinklerspawn multiplies this by another 1.05,
the dragon aura Dragon Guts multiplies this by 1.2 (or 1.22 if together with Reality Bending),
the patheon spirit Skruuia multiplies this by 1.15 (if worshipped in the diamond slot),
and a shiny wrinkler multiplies all of this by 3.
(Wrinklegill only affects the withering rate,
not the popping bonuses.)
Multiplying all of this together,
the bonus for popping the wrinkler maxes out at 5.10446475.
Sisyphus cannot train a dragon or get Sacrilegious corruption,
so he is limited to a bonus of 3.98475.

The fact that Sisyphus wrinkler popping bonus is just shy of 4
is particularly frustrating for our hero.
He only pops wrinklers when they cannot possibly hold more cookies inside them,
which must be a power of two
(due to the way of how IEEE754 floating-point numbers work,
as we have seen many times before).
Thus this bonus is applied to a "clean" power of two,
so they directly determine when we hit the precision limit
from popping wrinklers like this over and over again.
And this precision limit effectively rounds the bonus to 4
(the next power of two after it),
whereas even a bonus of 4.002
(achievable with a shiny wrinkler,
Skruuia on the ruby slot,
and the heavenly upgrade Sacrilegious corruption and the Easter egg Wrinklerspawn)
would round up to 8.

Alas,
Sisyphus loses that factor of two when popping wrinklers.
Our hero reaches the sexvegintillionth cookie,
earning all cookie-related achievements.
But we are still nowhere near Infinity,
there are more cookies to be gained.
