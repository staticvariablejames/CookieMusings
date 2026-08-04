Sisyphus Plays Cookie Clicker II: Sisyphus the Quindecillionaire
================================================================

(This is the second of a [series of articles](./README.md#sisyphus-plays-cookie-clicker)
investigating the limits of Cookie Clicker.
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

But disappointingly Sisyphus is still stuck at `2^53` cookies!
This is due to a quirk of Cookie Clicker.
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
(which is `2^56/2^52`),
and adding 6.5 to a number in this range still keeps us closer to the original number than to the next.

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

Using just buildings,
Sisyphus easily gets to [87.112 duodecillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-1.cki),
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
because leveling it up too much will negatively affect the Grimoire.)
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
  - `Date` objects internally track the number of milliseconds since
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
This is enough to get us to [842.498 vigintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-2.cki).


Wrinklers as Reservoirs
-----------------------

The next step in Sisyphus journey is the Grandmapocalypse,
and with it,
the wrinklers.
Wrinkler ambergris is quickly grabbed by our hero,
who now has to study how wrinklers suck cookies.

With a single wrinkler,
every game tick it will suck 5% of what would be gained by Sisyphus.
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
We will see this "**reservoir effect**" twice more in this series of articles.

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
increasing that number by 45% (i.e. withering 72.5% of the CpS) with a full garden.
- The dragon aura Dragon Guts also boosts the digestion rate,
  but again Sisyphus cannot train a dragon,
  for he cannot ascend to purchase the "How to bake your dragon" heavenly upgrade.
- Alternating rows of nursetulips and wrinklegills
  increases the digestion rate by about 64.026%,
  instead of just 45%,
  but the nursetulips pretty much halve the normal CpS,
  so how much each wrinkler digests per game tick is actually reduced.
- The maximum (reasonable) digestion rate achievable by Sisyphus is thus 72.5%.
  With Dragon Guts and the heavenly upgrade Elder spice,
  we do get to 100% CpS withered,
  but that's not accessible for Sisyphus.

The second wrinkle in the calculation is the bonuses that are applied after the wrinkler is popped.
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
and this limit must be a power of two
(due to the way of how IEEE754 floating-point numbers work,
as we have seen many times before).
Thus this bonus is applied to a "clean" power of two,
so they directly determine the next precision limit.
And this precision limit effectively rounds the bonus to 4
(the next power of two after it),
whereas even a bonus of 4.002
(achievable with a shiny wrinkler,
Skruuia on the ruby slot,
and the heavenly upgrade Sacrilegious corruption and the Easter egg Wrinklerspawn)
would round up to 8.

Alas,
Sisyphus loses that factor of two when popping wrinklers.
Our hero reaches the [121.417 sexvigintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-3.cki),
earning all cookie-related achievements.
But there are more cookies to be gained,
we are still nowhere near Infinity.


Sisyphus the Quindecillionaire
------------------------------

The next thing that catches Sisyphus's eyes is the stock market.
Each $1 in the stock market corresponds to a "1 **$**econd of production".
By selling high-valued stocks,
Sisyphus gains several seconds of production at once;
the hope is that these sales outpace wrinklers.

[Sisyphus has read my article on the limits of stock market stocks](./stock-market-hard-cap.md),
and knows that,
with low-level banks,
goods can never go beyond $65k of value.
By redoing the math in that article with high-level banks,
we can see that the goods values
will always fluctuate up to $415k around the resting value of the good
(i.e. the goods values will be between `restingValue-415k` and `restingValue+415k`).
The resting value is `bankLevel + 9 + 10*id`;
the `id` is a number between 0 and 17,
and Sisyphus's bank levels are 2^53.
The bank level dominates the goods values in Sisyphus's stock market,
as the theoretical minimum value of 2^53-415k is only 0.0000000046% smaller
than the bank level of 2^53.

This is already enough to be sligthly better than wrinklers.
Under optimal conditions,
popping a wrinkler gives the equivalent of `2^51` times the current CpS,
whereas selling a single unit of the stock market gives `2^53 * CpS` cookies.
But the stock market also has a reservoir effect of its own:
Sisyphus can use whatever means to purchase individual units of each good,
then sell all of its assets at once.

And Sisyphus warehouses are _big_.
The storage capacity for each good is its highest amount of the corresponding building,
plus a bonus from the office level,
plus 10 times its level.
The first two terms sum to less than 4 digits (they're smaller than 10000)
but the last term is,
of course, `10 * 2^53`,
i.e. around 90 quadrillion.
Each good unit sells for around `2^53` CpS,
giving Sisyphus `10 * 2^106 * CpS` at once.
And,
again,
Sisyphus repeats this process `2^53` times,
yielding `10 * 2^159 * CpS`,
rounded up to the next power of two.
The profits in the stock market are exactly `16 * 2^159`,
i.e. 2^175 = 11.962 quindecillion.
Sisyphus is now a quindecillionaire.

But before Sisyphus's profits skyrocket beyond the Solar System
and crash the market so hard the 1929 crisis becomes literally a rounding error,
he has to solve the problem of actually filling his warehouses with hundreds of quadrillions of goods.
With wrinklers,
his buildings generated cookies,
which were then sucked by the wrinklers.
But now he has to spend his hard-earned cookies directly,
so we are adding to the reservoir from the same source we will dump into later.

The IEEE754 floating-point specification,
unexpectedly,
offers our hero an olive branch.
The first sale took a while,
because Sisyphus had to slowly gather the $9 quadrillion for each good,
then purchase a single unit of that good,
and repeat this proces 90 quadrillion times.
But when he sold all the goods at once for the first time,
his bank became 90 quadrillion times higher than the cost of a single unit of each good.
This is more than 2^54 (18 quadrillion),
so when we purchase a single unit of that good and subtract that number from the cookie bank,
the IEEE754 floating-point specification dictates that
the result must round back to the same value of the cookie bank.
Each singular good literally becomes a rounding error,
whence Sisyphus can purchase units of each stock literally for free.
- Technical explanation:
  if the `c` cookies that Sisyphus has is between `2^e` and `2^(e+1)`,
  then the `2^52` floating-point numbers represented in this interval
  are spaced `2^(e-52)` apart.
  If we subtract a value `v` from `c` which is smaller than `2^(e-53)`,
  then the closest representable value to `c-v` in this interval is still `c`.
  The exception is if `c` is exactly a power of two,
  in which case `c - v` would fall in the previous range `[2^(e-1), 2^e)`
  where the representable numbers are spaced `2^(e-53)` apart.
  For this exception,
  we must guarantee that `v` is at most `2^(e-54)`
  (i.e. 2^54 times smaller than `c`)
  to ensure that `c-v` rounds to `c`.

With this,
the quindecillionaire Sisyphus reaches [279.968 duotrigintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-4.cki).
But the truce with IEEE754 is short-lived,
for the stock market has one fatal flaw:
its profits are measured in seconds of the highest _raw_ cookies per second this ascension.
When Sisyphus learns about combos,
wrinklers shall be kings once more.


Achievements and Combos
-----------------------

Sisyphus noted that the most impactful upgrades are usually kittens.
These upgrades are based on "milk",
which is just the number of achievements divided by 25.
New kittens are unlocked on increments of 1
(meaning one new kitten every 25 achievements),
and how much each kitten boosts production also depends on milk.
For example,
Kitten workers increase production by `milk * 12.5%`
(i.e. CpS is multiplied by `1 + milk * 0.125`).
The other kittens work in the same way,
but with different constants.
- Exception:
  The first kitten uppgrade,
  "Kitten helpers",
  unlocks at 13 achievements owned, rather than the usual 25.

Hence,
Sisyphus starts achievement hunting.
Some achievements can only be achieved on new files,
so begrudgingly Sisyphus hard-wipes his save file to start anew.
(These achievements are Speed Baking I, II, III; Neverclick, True Neverclick; and Hardcore.
They can also be achieved on the "Born again" ascension mode,
but because we are in the Greek Underworld,
no ascensions of any kind for Sisyphus.)
Shadow achievements don't count for milk,
so although Sisyphus will grab them if the chance happens,
Speed Baking I, II, and III will be forfeited in favor of Neverclick and Hardcore.

Sisyphus immediately gets the several easy-to-get achievements
(like "Stifling the press" and "Cookie-dunker").
Seasons bring several other achievements and upgrades.
Sisyphus does not have the "Season switcher" heavenly upgrade,
so he has to go through the seasons naturally;
and the code that naturally advances seasons has the same issue as the Birthday cookie
(it stops working after the year 275760)
but that is still plenty for Sisyphus to grab all achievements and upgrades.
The Chocolate egg is a one-time purchase that multiplies the bank by 1.05
(which bypasses the IEEE754 precision limitations we've been dealing with so far),
so Sisyphus saves it as the last resort.

The hunt for achievements has Sisyphus clicking reindeer,
conjuring baked goods,
harvesting duketaters,
and of course clicking golden cookies.
This is Sisyphus's first foray into combos.

On their own,
all these actions are simply too small to threaten the power of our hero's $11 quindecillion.
For example,
clicking a reindeer gives one minute of CpS,
meaning that Sisyphus gets `60 * CpS` cookies at once.
The Grimoire spell Conjure Baked Goods gives Sisyphus `1800 * CpS` cookies,
and harvesting a mature Duketater gives `7200 * CpS` cookies.
This is nothing compared to the $9 quadrillion
obtained by selling a single unit of anything from Sisyphus's warehouses.

But the goods from Sisyphus's warehouses are priced according to raw CpS.
Clicking golden cookies sometimes gives us buffs,
namely Frenzy, Building Specials, and (during the grandmapocalypse) Elder Frenzies.
For example,
harvesting a mature Duketater while having both Frenzy and Elder Frenzy
gives Sisyphus `7 * 666 * 7200 * CpS = 33 566 400 * CpS` cookies.
This is,
of course,
not enough to outpace the stock market.
We need to stack more buffs,
i.e. we need a better combo.

Without any modifiers,
golden cookies take beween 5 and 15 minutes to spawn
(the spawn time is random, and somewhat skewed towards the average).
Using all upgrades that Sisyphus has available
(Lucky day, Serendipity, Golden goose egg, Green yeast digestives, Sugar blessing)
shrinks that range to between 63.5 and 113.73 seconds.
Using the garden shrinks this even further:
a garden full of mature Golden Clovers and Nursetulips speeds golden cookie times by about 192%
(meaning that these numbers are divided by 2.92)
so the range is now between 27.76 and 65.23 seconds.
(Selebrak cannot be used here,
as the code that handle natural seasons stopped working in the year 275760.)
With the upgrades,
building specials last for just over a minute.
This means that Sisyphus can get three Building Specials,
he just needs to be lucky to get two golden cookies in a row spawning within 30 second of each other.

The power of building specials is the number of buildings divided by 10.
For example,
with the 280 duotrigintillion cookies that Sisyphus got from the stock market,
he was able to purchase 1652 cursors, 1639 grandmas and 1622 farms.
The three building specials corresponding to these buildings
thus multiply the CpS by 166.2, 164.9 and 163.2, respectively.
Together with a Frenzy,
this means that a hypothetical Duketater harvest
would provide `7 * 166.2 * 164.9 * 163.2 * 7200 * CpS = 2.25e11 * CpS` cookies,
which is still lower than the $9e15 of the cheapest good in Sisyphus's warehouses.

Sisyphus can go further by summoning golden cookies.
Force the Hand of Fate summons a golden (or wrath) cookie,
independently of the spawning cycle of natural golden cookies.
He can cast that spell six times in a row:
first cast it with 321 wizard towers,
sell down to 21 (making the spell cheaper),
cast it again,
purchase wizard towers up to 321,
use a sugar lump to immediately refill the magic meter,
repeat the cycle (cast, sell, cast, rebuy),
harvest a caramelized sugar lump to clear the sugar lump cooldown,
and repeat the cycle again.
(Sisyphus cannot afford a seventh cast;
Without Krumblor's Supreme Intellect,
he would need over 3700 wizard towers for that.)

These nine building specials gives Sisyphus a CpS boost of 6.52e19,
so now the hypothetical duketater harvest yields `7 * 6.52e19 * 7200 * CpS = 3.28e24 * CpS` cookies,
which is more than the goods in his warehouses!
But before he decides whether to sacrifice one nursetulip or one golden clover
to plant the lone duketater,
he realizes that `3.28e24 * CpS` is not enough.
Although each good in his warehouses are worth `9e15 * CpS`,
he does not sell them one-by-one;
he sells them wholesale,
90 quadrillion units at a time.
That corresponds to `9e15 * 90e15 * CpS = 8.1e32 * CpS` in a single sale,
and what matters for handling IEEE754 floating-point precision limits
is how many cookies we can get at once.
In other words,
_this combo is not strong enough to outpace the stock market's reservoir effect_.

So we have to resort to a different reservoir effect,
and go back to overfeeding wrinklers.
We do lose the 7200 factor from the duketater,
and gain a 1/30 factor from the game adding to wrinklers that fraction of the CpS every tick,
but now we are feeding a reservoir instead of gaining cookies directly.
Cashing in the reservoir is thus effectively a `2^53 = 9 quadrillion` multiplier,
which does outpace the stock market.

The effect from golden clovers only apply to golden cookies,
not to wrath cookies.
So Sisyphus cannot be at the latest stage of the grandmapocalypse
if he wants to stack three building specials naturally.
Letting the pledge go is not good enough,
because over time the game progresses all the way through till the last stage,
and we must remain in the grandmapocalypse for a very long time while we fatten our wrinklers.
So,
begrudgingly,
Sisyphus wipes his save once more,
this time making sure not to purchase "Elder Pact"
and remain in the second-to-latest stage of the grandmapocalypse.
But if he must be in the second stage of the grandmapocalypse,
he may as well take advantage of that:
the last building special gained from a natural golden cookie
can be replaced with an Elder Frenzy from a natural wrath cookie.
So he replaces a 150.3x multiplier with a 666x multiplier.

Sisyphus marshalls the last few multipliers he can get his hands on.
Worshipping Mokalsium in the diamond slot nearly quadruples the CpS.
Holobore is slotted in the ruby slot after clicking all golden cookies.
A well-timed Cyclius grants another 15% CpS.
And finally,
once the second naturally-spawning golden cookie is clicked,
Sisyphus plants Whiskerbloom buds to get a slight boost to milk.
Quadrillions of wrinklers fattened up and popped later,
Sisyphus' bakery reaches [73.392 quattuortrigintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-5.cki),
i.e. 2^355 cookies.

Interestingly,
the loans from the stock market minigame do not help Sisyphus at all.
Together they do boost CpS by a factor of 3.6,
but each requires a downpayment corresponding to a percentage of the current bank
(20%, 40%, and 50%, respectively).
This means that, after the fifth wrinkler is popped,
simply taking the first loan once erases the equivalent of one wrinkler from the bank.
And Sisyphus pops overfed wrinklers 9 quadrillion times,
so he has to let go of that 3.6 factor.

Sisyphus has ignored the Grimoire spell Gambler's Fever Dream;
this is the last trick up his sleeve,
and we will analyze it in the next article.


The Companion GitHub Repository
===============================

For this article,
the companion repository has 5 save files,
one for each explicitly-mentioned cookie threshold.
- [Save 1: 87.112 duodecillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-1.cki),
  from just using buildings.
- [Save 2: 842.498 vigintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-2.cki),
  from leveling up buildings and purchasing the Birthday cookie.
- [Save 3: 121.417 sexvigintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-3.cki),
  from wrinklers.
- [Save 4: 279.968 duotrigintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-4.cki),
  from the stock market.
- [Save 5: 73.392 quattuortrigintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus2-5.cki),
  from achievements, combos, and wrinklers again.
