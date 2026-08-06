Sisyphus Plays Cookie Clicker III: Sisyphus Goes Gambling
=========================================================

(This is the second of a [series of articles](./README.md#sisyphus-plays-cookie-clicker)
investigating the limits of Cookie Clicker.
In this article we investigate how many cookies we can obtain without ascending
closing the game,
or abusing the Grimoire spell Gambler's Fever Dream.)

[In the previous article](./sisyphus-plays-cookie-clicker-2-stock-market.md),
Sisyphus used almost all tools available to him
to reach quattuortrigintillion cookies without ascending.
He even managed to score a truce with IEEE754 to get stock market goods for free,
but it was short-lived,
and he settled for overfeeding wrinklers instead.
Now he will reach out to the last resource at his disposal:
the Grimoire spell Gambler's Fever Dream (GFD).
(And,
of course,
purchasing the Chocolate egg afterwards.)

GFD is arguably the worst-coded portion of Cookie Clicker,
housing many bugs and exploits;
and the patches are not always successful.
Let us however start simple,
and cast the spell as it is intended to be cast.


Half the price, double the fun!
===============================

The cost of spells in the Grimoire is a bit peculiar,
in that they increase with the available max magic.
For example,
Force the Hand of Fate (FtHoF) has a base cost of 10,
plus a percentage cost of 60%.
That is,
if the current max magic is, say, 81,
then casting FtHoF costs `floor(10 + 0.6*81) = 58` magic to cast.
If we started with a full magic meter,
we will be left with 23 magic.
We can then sell Wizard towers until the max magic becomes 23;
this lowers the cost of FtHoF to `floor(10 + 0.6*23) = 23`,
meaning we have just enough magic to cast FtHoF a second time in succession.
(Starting with 81 max magic is, in fact,
the absolute minimum max magic we need in order to cast FtHoF twice in succession this way.)

As mentioned before,
Sisyphus can then buy Wizard towers until the max magic becomes 81 again,
use a sugar lump to refill the magic,
and double-cast FtHoF again.
Sugar lump refills have a cooldown,
and can only be executed once every 15 minutes.
This is too long for our purposes,
as Building Specials only last for one minute.
But there are caramelized sugar lumps:
when harvested,
these sugar lumps clear the 15-minute cooldown,
so (up to once per day) Sisyphus can refill magic twice in succession.
This gives Sisyphus six casts of FtHoF.
He does not have access to Reality Bending or Supreme Intellect,
which would reduce the cost of spells;
without those,
getting a seventh cast would require Sisyphus to start with 226 max magic,
which is only achievable with 3738 level 1 wizard towers.
Purchasing the 3738th Wizard tower costs 21.54 septenseptuagintillion cookies (`2.154 * 10^235`),
which is far beyond Sisyphus's reach.

But Sisyphus can cast Gambler's Fever Dream (GFD).
GFD itself is very cheap
(costs 3 + 5% of max magic);
when cast,
it randomly chooses one of the other eight spells,
and casts it at half its regular cost
(with the caveat of it having a higher chance of backfiring).

With the lower cost,
Sisyphus can stack more casts of FtHoF than before.
If Sisyphus gets GFD choosing FtHoF several times in a row,
with 1556 Wizard towers or more he can get up to 13 new building specials.
Starting with 156 magic,
GFD costs 10 magic and FtHoF costs 103 magic,
so a `GFD->FtHoF` cast costs 61.5 magic.
Then we sell down to 94 max magic (454 towers),
reducing the cost of a `GFD->FtHoF` cast to 40 magic.
Sell down to 54 max magic (121 towers), `GFD->FtHoF` costs 26;
sell down to 28 max magic (31 towers), `GFD->FtHoF` costs 17;
sell down to 11 max magic (6 towers), `GFD->FtHoF` costs 11.
Now we purchase 526 towers to get back to 100 max magic,
spend a lump,
and repeat this process to get 4 more `GFD->FtHoF` casts.
A caramelized sugar lump gets us the last 4 casts.

Getting lucky multiple times in a row is not a problem for Sisyphus,
for he has been playing on this save file for decillions of years,
and he can play a few decillion more.
Together with the two building specials and the elder frenzy from the garden,
overfeeding and popping wrinklers brings Sisyphus' bakery
to [2.644 quadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-1.cki).

Interestingly,
the cost halving from GFD preserves the decimal.
E.g. if we start with 23 max magic,
GFD costs `floor(3 + 0.05*23) = 4` and FtHoF costs `floor(10 + 0.6*23) = 23`,
but if GFD picks FtHoF the total cost is `4 + 23/2 = 15.5`.
Preserving this extra half-point of magic
could reduce the minimum number of towers needed to get 13 FtHoF casts,
but is not enough to get a 14th cast.


Threading the Needle
--------------------

Sisyphus has read my [primer on pseudorandom number generators](https://github.com/staticvariablejames/ChooseYourOwnLump#randomness-and-planners)
and knows that Grimoire spell outcomes are seeded
based on the `Game.seed` and on the total number of spells cast so far.
The number of spells cast so far is,
of course,
an IEEE754 floating-point number,
so it stops increasing once it reaches `2^53`.
When this happens,
all the outcomes from the grimoire share the same seed,
making them highly correlated.
This means that Sisyphus had to wipe his save a few times
until he landed on a `Game.seed` that gives the needed outcomes;
i.e. casting GFD lands on FtHoF,
and this FtHoF has a building special as the outcome.

In fact,
this extremely high correlation of seeds almost worked against Sisyphus here.
The outcome of a cast also depends on the current state of the game.
Specifically,
for GFD,
the game only considers the affordable spells when selecting the spell to be cast.
With 1556 towers and a full magic meter, all 8 spells can be afforded,
but if we only have 14 max magic and 12 current magic,
Spontaneous Edifice and Resurrect Abomination are not affordable anymore.

The problem is in how the game uses the PRNG to pick a candidate from a list.
The game first builds the list of affordable spells,
then it rolls a rational number `r` between 0 and 1,
multiplies `r` by the list length,
and chooses the item `floor(length * r)` of the list.
Because the seeds are the same,
the number `r` will always be the same,
so depending on how the list is constructed
it could very well be that GFD yields a different outcome,
despite starting from the same seed.
If, say, the spells were declared in a different order
in <https://orteil.dashnet.org/cookieclicker/minigameGrimoire.js>,
it could very well be that getting the 13 building specials this way would be impossible.

Thankfully for Sisyphus,
there are seeds where GFD picks FtHoF for all 13 casts.
(The companion repository
[has a seed-searching script](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/src/seed-search.ts).
In this case,
the lexicographically least seed satisfying this property is `aaadj`.)
Sisyphus merrily continues his journey,
unaware of this bit of luck he had.
