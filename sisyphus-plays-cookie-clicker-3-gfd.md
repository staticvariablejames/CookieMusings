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


Bugs, Bugs, Bugs
----------------

    Programmer has a problem.
    "I know, I will use threads!"
    Programmer has two problems.

(Or, more likely, `twoProgrammer problems. has `)

Sisyphus immediately noticed that GFD has a 1-second delay between casting and resolving
(i.e. casting the spell chosen by GFD).
In detail,
GFD is split in two steps:
1. The **cast**,
   in which GFD picks a _target_ spell to cast,
   memorizes its cost,
   and charges the GFD "self cost" (3 magic + 5% of max magic).
2. The **resolution**,
   which happens one second later,
   in which GFD casts the target spell,
   and charges the target's spell cost.

This means that GFD's code is _asynchronous_:
parts of it happen in different points in time.
When the second part of GFD (the resolution) happens,
the underlying game state may have changed,
so the code has to account for that.
But Cookie Clicker's code does not account for everything.
Asynchronous code is notoriously difficult to get right,
and Orteil didn't.

The most prominent bug is "scrying".
After any spell is cast,
the spells cast counter is increased.
The resolution step of GFD,
however,
uses the _current_ counter and does not increase it afterwards.
This means that manually casting the same spell chosen by GFD
will use the same PRNG seed.
The most important use case of this is precisely when GFD picks FtHoF,
because the FtHoF outcome will be the same,
so we effectively _scry_ the buff obtained from the next FtHoF cast.
(Two caveats: the GFD cast has a higher chance of backfiring,
so the outcomes may differ if the GFD cast backfires and the normal cast does not;
and there are some game states that alter FtHoF behavior,
like the current season and whether there's an active Dragonflight buff or not.)
Sisyphus technically cannot make use of this bug anymore,
because his spells cast counter is permanently stuck at `2^53`,
so the current and past spells cast counters are all the same.

Cookie Clicker has no means of halting the resolution step of a GFD cast.
(In technical terms,
the `setTimeout` id is never stored anywhere.)
This means that if,
for example,
a different save file is loaded in-between the cast and resolution steps,
Cookie Clicker will still try to cast the target spell anyway,
using the cost calculated during the cast step.
So if the "donor save" (where GFD is cast) has very cheap spells due to a low max magic,
and the "receiving save" (where GFD resolves) has a large pool of magic,
the receiving save will be able to cast the target spell multiple times more than it should have,
not only due to paying a cheaper cost to cast the spell,
but also for not having to pay the GFD's "self cost".
Sisyphus is not able to abuse this bug because,
in the very beginning of this series of articles,
we forbade him from loading save files at all.

The game does make a half-baked effort in stopping this
by checking whether `Game.seed` changed between cast and resolution;
if it did, the resolution step is aborted.
This does prevent a cast from being transferred between two unrelated save files,
and (usually) prevents a cast from resolving after ascending;
but the player can always take two save files from the same lineage
(ensuring that `Game.seed` stays the same)
or roll again into the same `Game.seed` after ascending.
(Of course Sisyphus is unable to exploit this.)

Cookie Clicker also does not store anywhere in the save file
the information that a GFD cast is "in transit".
This means that if we save the game between the cast and the resolution,
loading that save again will have advanced the spells cast counter
without experiencing the effects of the resolution step.
This specific bug is more niche,
as it only advances the spells cast counter for cheap,
so Sisyphus is not able to exploit it.

But the one thing that Sisyphus will be able to exploit is the refund mechanic.
Besides succeeding and backfiring,
a spell can _fail to cast_.
Stretch Time refuses to do anything if there are no buffs to affect.
A successful Spontaneous Edifice cannot create buildings past 400,
and a backfired one cannot destroy buildings if there are none.
Resurrect Abomination does nothing outside of the grandmapocalypse;
a successful cast cannot create more wrinklers than there is space for,
and a backfired cast cannot kill wrinklers if there are none.
But most importantly,
if there is not enough magic for the target spell when GFD resolves,
the cast also fails.
In case of failure,
GFD refunds the magic spent on GFD during the casting step
(the self cost).
GFD itself can fail if there are no viable candidates for the target spell,
but GFD does not pick GFD as the target spell.

(The refund mechanism has a bug of its own.
Refunding simply adds the GFD self-cost to the current magic meter.
If this brings the current magic to above the max magic,
in the next `Game.Logic()` tick the issue is fixed and the additional magic is erased.
Hence,
until the next tick happens,
there will be a few frames where the current magic is higher than the max magic.)

Some possible exploit avenues were already blocked by Sisyphus condition.
For example,
when transferring a GFD cast between saves,
if the spells in the "donor save" are too expensive for the "receiving save",
the magic will be refunded in the receiving save,
thus essentially instantaneously giving magic to the receiving save.
Similarly,
if we ascend and stay in the same seed,
a refund from a cast made before ascending
allows us to start the next ascension with more magic than what would normally be possible.
But not being able to load saves nor ascend prevents Sisyphus from abusing these exploits.

However,
Sisyphus can combine the refund mechanic
with the fact that the cost of the target spell is decided during the GFD cast phase.
This will allow Sisyphus to cast FtHoF more times than normal,
as follows.


Buffs, Buffs, Buffs
-------------------

Sisyphus starts with 517 level 1 Wizard towers,
giving him 100 max magic.
GFD self-cost is 8 and FtHoF's cost is 70.
He quickly casts GFD 8 times in succession,
depleting 64 magic.
All these casts target FtHoF,
and will attempt casting it with a cost of 35.

Then,
before any of these GFD casts resolve,
Sisyphus sells down to 49 Wizard towers,
bringing him down to 36 max magic.
GFD self-cost is now 4, and FtHoF's cost is 31.
He starts quickly casting GFD once again,
always staying under 35 magic.
Initially he casts GFD 5 times, depleting 20 magic.
Next,
whenever one of the 8 initial GFD casts resolve,
it attempts casting FtHoF for 35 magic,
but there is not enough magic for it to be cast.
So all 8 casts fail,
and refund the GFD self-cost of 8 magic each.
Sisyphus quickly casts GFD again twice for each of the failed resolutions.
Now he has 21 GFD casts pending resolution,
each of them with a self-cost of 4 and attempting to cast FtHoF for 15.5 magic.

In the third stage,
Sisyphus sells down to just 6 Wizard towers.
Max magic is 11, GFD self-cost is 3, FtHoF costs 16.
This is the trickiest part of this combo execution:
Sisyphus casts GFD once,
and whenever one of the 21 pending casts resolve,
he cast GFD once more.
Each of the resolutions fail and refund the magic,
but by alternating between a resolution and a new GFD cast,
Sisyphus's magic will alternate between the max 11 (when the resolution refunds the 4 magic)
and 8 (after paying the self-cost of GFD with 11 max magic).
Now Sisyphus has 22 unresolved GFD casts,
all of them with a self-cost of 3 and attempting to cast FtHoF for 8 magic.

In the fourth stage,
Sisyphus can sit back and watch the spells resolve.
- In the last batch of 22 GFD casts,
  the first one resolves successfully,
  yielding a FtHoF cast and bringing the current magic down to 0.
- The next three GFD casts fail (each one refunding 3 magic)
  but the fifth succeeds,
  yielding another FtHoF and bringing current magic to 1.
- The 6th, 7th and 8th casts again fail; the 9th succeeds,
  yielding a third FtHoF and bringing current magic to 2.
- The 10th and 11th casts fail,
  bringing the current magic to 8.
  We are back where we started,
  so this cycle repeats,
  yielding three more FtHoF casts.

Of course,
Sisyphus has to be quick to do all of this,
but an eternity pushing a massive boulder up a hill over and over again
has honed his motor capabilities.

Sisyphus has wasted a bunch of magic in this process.
Between stages 2 and 3,
he had 20 magic but lowered down to 11 (wasting 9 magic),
and converting between the second batch of GFD casts to the third batch of GFD casts
exchanged 4 magic for 3 magic
(wasting another 21 magic).
So he knows that he could have extracted even more FtHoF casts from 100 magic.
Perhaps starting with more max magic, even?
But six casts are enough:
he can repeat this three times,
granting him 18 building specials.
Together with the two building specials obtained from naturally-spawned golden cookies,
he gets all 20 building specials in his combo.
[Sisyphus now has 2.907 quattuorquadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-1.cki).


Stopping Time
-------------

All of these cookies were achieved by overfeeding and popping wrinklers.
Their reservoir effect essentially gives them a multiplier of `2^53`
over what they would normally provide.
Click Frenzies are nowhere near powerful enough to make clicking more powerful than wrinklers,
even when accounting for the fact that wrinklers only take 1/30th of the CpS per tick.

But now that Sisyphus has mastered abusing GFD,
he can overcome wrinklers with an artificial reservoir of sorts,
by harnessing the power of Godzamok.

When Godzamok is worshipped, say, in the diamond slot,
selling buildings grants the buff "Devastation" for 10 seconds.
Each building sold increases the clicking power by 1%.
Multiple buildings can be sold to increase the power of Godzamok's buff,
but they will not increase the length of the buff.
Devastation is still just a regular buff, though,
and the Grimoire spell Stretch Time works on it.
Of course the strength of Devastation is stored as an IEEE754 floating-point variable,
so we want to sell buildings 9 quadrillion times to maximize it.
The goal is thus to somehow cast Stretch Time so many times
to make the duration of Devastation long enough to perform the 9 quadrillion building sales.

Each successful cast of Stretch Time increases the timer on the buff by 10%,
up to a cap of 5 additional minutes.
This increase is based on the max time,
so even a nearly depleted buff can be stretched back to life.
Starting with 100 magic,
if GFD always picks Stretch Time
we can cast it 7 times in a row,
nearly doubling the max time
(`1.1^7 = 1.9487171`),
and having a cap of 35 extra minutes.
Since the sugar lump refill cooldown for minigames is 15 minutes,
once a buff has a duration longer than 462 seconds
we can stretch it indefinitely.
(This does mean that Sisyphus will have to wipe his save one last time
to get an appropriate seed,
as the one used for the previous save had GFD targetting FtHoF instead of Stretch Time.)

Devastation has an initial duration of 10 seconds;
stretching it 41 times raises its max time to 497.85 seconds,
allowing it to be further stretched indefinitely.
Hence Sisyphus has to somehow cast Stretch Time 41 times in a row.
And he achieves that with more GFD abuse,
of course.

If Sisyphus has no buffs,
Stretch Time will fail to cast.
But while the unresolved GFD cast is waiting for its time,
the natural magic regeneration of one unit every dozens of seconds will still be active.
So Sisyphus can cast GFD a few times,
and whenever a GFD resolves (and refunds its cost),
cast GFD again.
Since the magic meter will never be full this way,
after many minutes juggling these GFDs Sisyphus will have enough magic to cast GFD one more time.
This means that,
over time,
Sisyphus stores more and more magic ready to be refunded in the unresolved casts.
Several hours later,
Sisyphus can sell some cursors to trigger Godzamok,
and let all the unresolved casts resolve.
Most of them will simply become refunds,
but a few of them will actually cast Stretch Time.
With e.g. 9 Wizard towers,
GFD cost 3 and Stretch Time cost 10,
so to pay for all the 41 Stretch Times needed Sisyphus has to juggle 69 GFD casts.
This does mean that Sisyphus has to sustain 69 clicks per second for a few hours;
good thing he honed his body lifting that massive boulder, right?

Sisyphus laments that he _almost_ had a much less stressful way of achieving this goal.
If he had been allowed to close his game in the beginning of his journey,
he could achieve the result of stretching a Devastation buff using caramelized sugar lumps.
Harvesting one of those clears lump cooldowns instantly,
but existing buffs only tick down while the game is open,
so by getting lucky with the lump types
and only playing a few instants per day
Sisyphus could stretch a single Devastation buff to beyond the 462 seconds threshold
in only a few years.
Then,
by ensuring that this buff is always the longest,
he can let other buffs run out before starting a new combo.
(Once he purchases more buildings,
he has to at least let the building specials run out,
in order to pick them up again but accounting for the higher building count.)

In any case,
that sufficed.
Sisyphus stretched a Devastation buff long enough to sell cursors 9 quadrillion times.
He can sell a bit over 2300 cursors at a time,
increasing the buff power by about 23 each time,
so the maximum power of the buff is `2^58`.
This "artificial reservoir" is finally enough to overcome the `2^53` factor from wrinklers,
just barely.
Sisyphus can, of course,
gather all 20 building specials, an Elder Frenzy, and a Click Frenzy
using the same method.
He can even get his hands on a few additional multipliers:
Sisyphus can finally complete the Grandmapocalypse,
netting him a few extra achievements and upgrades;
he can plant whiskerblooms to increase CpS,
rather than using the garden to get more golden cookies;
and he can get the three loans and stretch them too,
paying their cost only once this way.
Sisyphus leaves wrinklers behind,
and reaches [12.194 sexquadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-3.cki).


The Final Trick: Buildings as Ineffective Reservoirs
----------------------------------------------------

Sisyphus has employed pretty much every single multiplier he has access to;
the only exception is the Chocolate egg,
which he will use as the absolute last resort... which will happen in a few paragraphs.
He starts selling his buildings to extract the maximum out of the Chocolate egg,
but as he does so,
he notices that he is actually surpassing his previous limit of 3 sexquadragintillion cookies,
even if just by a little.
"I think I can squeeze a few more cookies past the IEEE754 floating-point limitations!"

So far,
Sisyphus has only purchased buildings.
Whenever he hits a new cookie limit,
he uses the cookies in bank to purchase buildings.
He then reaches the limit again,
and purchases buildings again.
The last few units of each building all have to be purchased one by one;
for example,
the 2318th Cursor costed Sisyphus 6.308 sexquadragintillion cookies,
which is more than half of the limit of 12.194 sexquadragintillion cookies
that Sisyphus has reached in the previous step.
Hence he can only purchase that one cursor,
before having to grind back to 12.194 sexquadragintillion cookies.
He then purchases another cursor,
and so on,
until he reaches the limit of 2322 cursors.
(The 2323rd cursor would cost 12.688 sexquadragintillion cookies,
which is more than what Sisyphus can have in bank.)

But selling all those cursors is worth 21.146 sexquadragintillion cookies.
Sure,
Sisyphus only gets 25% of what he paid back,
and the bulk of the sale comes from the last few cursors;
but those sales are still enough to push Sisyphus beyond the limit he previously had.
The cursors had behaved like a cookie reservoir,
even if much less powerful than the other reservoirs that Sisyphus has dealt with.

This brings Sisyphus to 36.512 sexquadragintillion cookies.
Of course he cannot use those cookies to purchase more cursors,
as he has just sold all of them to reach this number.
But Sisyphus can purchase more grandmas.
The 2310th grandma costs 13.747 sexquadragintillion cookies
and the 2311st grandma costs 15.809 sexquadragintillion cookies.
Then Sisyphus repeats the same process of grinding back to 2322 cursors and selling them all at once,
and purchases the 2312nd grandma;
and so on.
This process ends with 2316 grandmas,
as the 2317th would cost Sisyphus 36.568 sexquadragintillion cookies.

But now selling all those grandmas nets Sisyphus 60.947 sexquadragintillion cookies.
Together with the 36.512 sexquadragintillion cookies from cursors,
now Sisyphus can reach 106.601 sexquadragintillion cookies.
Now he can purchase more farms!

The entire process repeats all the way until You,
the last building.
Sisyphus actually has to start with Wizard towers, rather than Cursors,
because he has to frequently sell towers to lower the max magic.
These reservoirs are imperfect,
as they are filled from the same source that they will fill later;
for this last trick,
only the building at "the bottom" can be sold and purchased freely,
so it has to be Wizard towers.
Finally,
before purchasing the buildings,
Sisyphus can fill his garden with cheapcaps,
cast Crafty Pixies,
and worship Dotjeiess,
to get another building here and there.
This gives Sisyphus [109.692 novemquadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-4.cki).

And finally,
Sisyphus realizes that these higher building counts yields better building specials.
He started out by having "You"s as the last building in the chain,
but he can also have Chancemakers as the last building,
and so on.
Every time he performs this buildings-as-reservoirs trick,
he can grab a building special with the highest building and stretch it indefinitely.
So by performing this trick 18 more times
(one for each building besides Wizard towers)
he can extract a few more cookies by having slightly better building specials active.

Sisyphus is sure that changing the order at which he purchases buildings
could net a few further cookies,
but he has given up on meaningful increases to his limit.
He sells all buildings and finally purchases the Chocolate egg:
[231.662 novemquadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-3.cki)
(`2.3166211683437614e+152`,
a bit over `2^506`)
is his final answer.
He cannot get any more cookies without ascending.


94% of the achievements done!
-----------------------------

Sisyphus takes stock of what he achieved.

He has purchased 583 upgrades (which is 81% of the 717 available in the game);
he is only missing synergies, fortunes, dragon drops, cookies from the several heavenly upgrades,
and the Heavenly chip secret series of upgrades.
All of these are only unlocked by purchasing heavenly upgrades,
which Sisyphus has no access to.

He has purchased over 2000 units of all buildings.
He got 2062 Yous and 2483 Cursors,
although not at the same time
(he had to perform the buildings-as-reservoirs trick to get these numbers).

And he has achieved 590 achievements (which is 94% of the 622 available in the game).
He got all the cookie-related achievements and all building-related achievements,
plus many minigame and miscellaneous achievements.

He is missing, of course,
all achievements related to ascension numbers and ascending with certain amount of cookies baked.
Some other achievements require heavenly upgrades to unlock,
like "Here be dragon" (fully train Krumblor),
"No time like the present" (receiving gifted cookies),
"O fortuna" (own every fortune upgrade),
and "Debt evasion" (ascending with an active loan).
And the last two achievements for owning a certain number of upgrades
can only be achieved after he unlocks more upgrades.

Four omissions are strategic.
Sisyphus has not leveled up his Wizard towers,
in order to have more flexibility with max magic,
so he does not have the achievement for having leveled up them to 10.
And the shadow achievements Speed Baking I, II, and III
were given up in favor getting Neverclick and Hardcore at the same time.

But perhaps the most interesting omission from his list of achievements
is "Speed's the name of the game", the very last production achievement.
It is awarded for baking 100 septendecillion cookies per second,
and Sisyphus is limited to 30.524 septendecillion cookies per second.

Sisyphus has asked Asopus to subit a pledge on his behalf to the Olympus gods
to allow him to ascend in Cookie Clicker.
The gods will be reviewing Sisyphus's pledge in a council meeting,
to determine whether to allow Sisyphus to ascend,
and if so,
under which restrictions.
But the meeting will happen in the next article.


The Companion GitHub Repository
===============================

For this article,
the companion repository again has 5 save files.
- [Save 1: 2.644 quadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-1.cki),
  from using GFD the intended way.
- [Save 2: 2.907 quattuorquadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-2.cki),
  from abusing GFD's refund mechanic to get all 20 building specials.
- [Save 3: 12.194 sexquadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-3.cki),
  by using Stretch Time and a maxed-out Godzamok.
- [Save 4: 109.692 novemquadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-4.cki),
  by doing the buildings-as-reservoirs trick.
- [Save 5: 231.662 novemquadragintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus3-5.cki),
  by maxing out building specials and purchasing the Chocolate egg.
