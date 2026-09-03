Sisyphus Plays Cookie Clicker V: Reach for the Heavens
======================================================

(This is the third of a [series of articles](./README.md#sisyphus-plays-cookie-clicker)
investigating the limits of Cookie Clicker.
In this article we reach the limit of cookies
obtainable under the rules imposed by the previous article.)

[In the previous article](./sisyphus-plays-cookie-clicker-4-rules-change.md),
the gods have finally allowed Sisyphus to ascend in Cookie Clicker.
This came at a hefty cost:
the Olympus gods significantly curtailed the strenght of Gambler's Fever Dream,
so Sisyphus will have to adopt new strategies.


Power gained, power lost
------------------------

The heavenly upgrades are powerful.
We get more cookie upgrades from upgrades like "Tin of British Tea Biscuits",
two tiers of synergy upgrades,
fortune upgrades,
various simple multipliers (like "Aura Gloves" making clicking more powerful),
unshackling upgrades,
and of course Krumblor.

Sisyphus uses the small bit of time-travel magic to get the upgrades from petting the dragon,
and places them in the permanent upgrade slots.
(This is the only use of the permanent upgrade slots for Sisyphus.)
Hermes's help
([from the previous article](./sisyphus-plays-cookie-clicker-4-rules-change.md))
means that the Heralds upgrade is always capped at a +100% CpS boost.

And ascending granted Sisyphus the last few missing achievements.
The only omission is Motormouth (reach level 10 Wizard towers);
Sisyphus deliberately keeps his Wizard towers at level 1
in ordor to maximize the value extracted from the Grimoire.

But the restrictions placed by the gods on GFD are brutal,
and most strategies from [the third article of the series](./sisyphus-plays-cookie-clicker-3-gfd.md)
no longer apply.
Sisyphus can still get the combo with 15 building specials,
but he again has to use that to overfeed wrinklers.
And the buildings-as-reservoirs trick still works,
albeit it is a bit weaker,
as Sisyphus cannot use Wizard Towers as one of the reservoirs
(because he must frequently sell and purchase Wizard Towers for the Grimoire).

There are strategies enabled by heavenly upgrades that Sisyphus can adopt,
but for now,
let us just focus on how far Sisyphus can go with just prestige levels.


The power of prestige
---------------------

Whenever Sisyphus ascends,
all of the cookies baked this ascension gets added to the "cookies forfeited by ascending" statistic.
The prestige level is just the cube root of this number,
divided by 10000 and rounded down.
For example,
the 231.662 novemquadragintillion cookies obtained by Sisyphus prior to ascending
grants him 61.416 quattuordecillion prestige levels.

Each prestige level gained also nets one heavenly chip,
the currency needed to purchase heavenly upgrades.
The heavenly chips earned by Sisyphus are more than enough to purchase all heavenly upgrades,
so they can safely be ignored from this point ownwards.

The key aspect of prestige levels is the five upgrades that "unlock their potential":
Heavenly chip secret,
Heavenly cookie stand,
Heavenly bakery,
Heavenly confectionery,
and Heavenly key,
which must be purchased in order
and unlock 5%, 25%, 50%, 75%, and 100% of the prestige level potential.
respectively.
With Heavenly key,
each prestige level grants Sisyphus +1% of CpS
(proportionally less if only some of the upgrades are owned),
meaning that Sisyphus immediately has access to a 614.164 tredecillion multiplier for his CpS.
Heavenly chip secret alone is a 30.708 tredecillion multiplier;
the other four upgrades combined are actually just a 20x multiplier on top of it.

(The upgrades Lucky number, Lucky digit, and Lucky payout,
which requires Sisyphus to have one, two, and four "7"s in his prestige level to purchase,
increase the prestige level effect on CpS by 1%;
i.e. instead of granting +1% of CpS,
each prestige level grants +1.01% instead.
Although these upgrades are multiplicative,
the overall increase is very small,
amounting to just over +3% CpS.)

This very large multiplier means that Sisyphus gets past quinquagintillion cookies
even without the complicated strategies abusing GFD glitches that he needed before.
And these extra cookies can of course be sacrificed,
further increasing the prestige level.
If Cookie Clicker operated at infinite precision,
the compound effect from using the higher prestige level to get even more cookies
[roughly raises the cookie amount to the power of 3/2](./t-sqrt-t-growth.md).
Starting at quadragintillions (which we got without abusing GFD),
Sisyphus thus expects to go past sexagintillions.

But there is one extra trick here:
the cookies baked this ascension counter
(`Game.cookiesEarned` in the game code)
gets added all at once to the "cookies forfeited by ascending" counter
(`Game.cookiesReset` in the game code).
This means that `Game.cookiesReset` works as a reservoir,
so by ascending `2^53` times,
Sisyphus effectively gains a factor of `2^53` when sacrificing cookies.
This essentially compounds into a `2^79.5` factor for the number of cookies baked all time,
whence a factor of `2^26.5` for the CpS,
which makes Sisyphus reach [1.559 unseptuagintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-1.cki),
with a total of 3.032 quinseptuagintillion cookies baked all time.


Tricky Multipliers
------------------

Ascension upgrades also grants Sisyphus some additional multipliers,
namely the Golden Switch,
the Shimmering Veil,
and Sugar Frenzy.

Sugar Frenzy is the easiest one to obtain.
Its base duration is 1 hour,
so Stretch Time can be used to lengthen this buff indefinitely.

Shimmering Veil and Golden Switch are trickier.
Sisyphus is stuck fattening and slaughtering wrinklers,
so in theory he could use his bank to activate these two switches every once in a while,
and have the increased cookie production feed the wrinklers further.
But Sisyphus wants to repeat the cycle of fattening and slaughtering wrinklers `2^53` times,
and after the first time he has to store these cookies directly.
But each time he pops wrinklers,
he gains around `2^50` times his CpS,
and after doing this cycle `2^53` times,
Sisyphus has roughly `2^103` times his CpS in bank.
Anything that costs less than `2^49` times his CpS
can be obtained for free,
as it "rounds to zero" when purchased.
- This is the same trick that allowed Sisyphus to purchase stock market goods for free
  [in the second article of this series](./sisyphus-plays-cookie-clicker-2-stock-market.md#sisyphus-the-quindecillionaire).

But the main source of multipliers come from having more golden cookies.


More Golden Cookies
-------------------

There are many heavenly upgrades that increase the frequency of golden cookies
and the duration of their effects.

The most important one is Distilled Essence of Redoubled Luck.
This heavenly upgrade makes so that whenever a golden cookie spawns naturally,
there's a 1% chance for the spawn to be doubled (i.e. two golden cookies spawn).
(Also applies to reindeer.)
The very end of Sisyphus's combo happens with three naturally spawned golden cookies,
giving two Building Specials and one Elder Frenzy;
if all three of these naturally spawned golden cookies generate a second golden cookie,
Sisyphus can stack three more Building Specials to his combo.
(This does mean that Sisyphus has to hit the 1% probability three times in a row,
which on its own lengthens the average time between successful combos by 1 million,
but that's par for the course for Sisyphus at this point.)

Other golden cookie upgrades are (individually) less impactful,
but they are still strong enough together.
Lasting Fortune, Lucky Digit, Lucky Number, and Lucky Payout
increase the duration of Building Specials from 61 seconds to 70 seconds,
and Heavenly Luck and Startrade shortens the minimum golden cookie spawn time
from 24.1 to 21.8 seconds.
This means that,
_barely_,
Sisyphus can get four golden cookies in the time it takes for a Building Special to run out
(eight with Distilled Essence of Redoubled Luck),
meaning that Sisyphus can get 7 Building Specials (and the Elder Frenzy)
using only naturally-spawned golden cookies.

And the Fortune Cookies upgrade adds news messages that can be clicked to unlock the upgrades,
plus two special messages:
one of them gives 1 hour of CpS,
and the other spaws a golden cookie when clicked.
Unfortunately the latter cannot be used by Sisyphus at all;
the combos performed by Sisyphus have to be repeated at a minimum of 9 quadrillion times
(in reality multiple times more),
and the CpS increase from the fortune golden cookie could only apply to one of them.


Krumblor enters the chat
------------------------

Krumblor the Cookie Dragon is a powerful source of multipliers.
The cost of fully training Krumblor is basically a rounding error for Sisyphus,
and once done so he can slot up to two auras at a time,
which confers several benefits.

- Radiant Appetite is the simplest aura:
  it doubles cookie production.
  Sisyphus will use this aura for a simple CpS multiplier once he's done with his combos.

- Dragon Harvest is more interesting.
  While slotted,
  the golden cookies clicked by Sisyphus have 5-10% chance of yielding the buff Dragon Harvest,
  which (because Sisyphus owns Dragon fang) gives a 17x multiplier to cookie production.
  This buff lasts twice as long as Building specials,
  so Sisyphus can slot in this aura to get the buff,
  and then slot another aura to get a CpS increase.

- Earth Shatterer doubles how much cookies we gain back from selling buildings.
  Normally this aura is basically useless,
  because selling buildings is such a small fraction of the cookie production.
  However,
  this aura makes the buildings-as-reservoirs trick significantly more powerful;
  each step of the trick is now roughly twice as powerful,
  so with 18 buildings being used in the trick,
  it roughly confers a `2^18` multiplier to the number of cookies that Sisyphus can amass.

- Reality Bending gives 10% of every other dragon aura at once.
  The main use case is for when we want to boost a specific dragon aura to 110% of its power.
  In most cases, slotting another aura instead gives a bigger boost to CpS.

- Supreme Intellect has several effects on minigames.
  For Sisyphus,
  the benefit is that it makes Grimoire spells slightly cheaper,
  so Sisyphus is able to cast 14 `GFD->FtHoF` in a row now.

- Dragon's Fortune multiplies CpS by 2.23 per on-screen golden cookie.
  Normally,
  on-screen golden cookie should always be clicked to (hopefully) obtain a building special,
  which is a much higher multiplier;
  but the 14 FtHoF casts and the 7 building specials from natural golden cookies
  already saturate the limit of 20 building specials.
  Hence the very last golden cookie summoned by FtHoF can be left on-screen
  for a 2.23x multiplier.

The other auras are largely useless, though.
Honorable mentions:

- Breath of Milk it boosts milk by 5%,
  which essentially makes each kitten 5% stronger.
  (The actual effect is a bit smaller, but that's a good approximation.)
  This comes close to doubling the CpS,
  but not quite;
  so this one loses to Radiant Appetite.

- Elder Battalion acts as a very powerful Synergy upgrade,
  and roughly at vigintillion cookies
  it makes Grandmas be responsible for half of the player's cookie production.
  But eventually the other Synergy upgrades catch up,
  and beyond like sexagintillions this aura is always overshadowed by even Breath of Milk.

- Dragon Guts further increase how much wrinklers digest each tick,
  and make them explode into 20% more cookies.
  (The additional wrinklers are only relevant for increasing how much the wrinklers digest.)
  Recall from the second article in the series
  (near the end of the section [Wrinklers as Reservoirs](./sisyphus-plays-cookie-clicker-2-stock-market.md#wrinklers-as-reservoirs))
  that Sisyphus can now already get a multiplier of over 4 for exploding wrinklers,
  and the buff for cookie digestion
  is overshadowed by e.g. Breath of Milk simply increasing cookie production.

- Arcane Aura shortens the spawn time of golden cookies by 5%
  and Epoch Manipulator lengthens their buffs by 5%.
  This makes it easier
  for Sisyphus to get the 7 building specials from naturally-spawned golden cookies,
  but they are not nearly enough to get an 8th building special.

The only downside of Krumblor is that each aura switch costs 1 "You"
(or the highest building, if the player has no "You"s).
This means that "You" cannot be part of the buildings-as-reservoirs trick anymore.
A priori,
Sisyphus could simply choose to slot in Supreme Intellect and Earth Shatterer
(the two most impactful auras right now)
and not change it afterwards,
but that locks Sisyphus out of the buff from Dragon harvest
and the multipliers from Dragon's Fortune and Radiand Appetite;
and these multipliers increase cookie production
by more than the effect of the additional building.

Sisyphus reaches [31.853 unoctogintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-2.cki),
with 123.003 quinoctogintillion cookies baked all time,
using all these multipliers.


The sacrifice needed to get rid of wrinklers
--------------------------------------------

While Sisyphus fattens and slaughters wrinklers over and over again,
Godzamok sits there,
ever tempting our hero.

"Come on, worship me," Sisyphus imagines Godzamok whispering.
"I'm even more powerful now,
thanks to the 1223x boost from Dragonflight.
I'm sure you can make use of my powers."

And this is true:
Sisyphus can stop overfeeding wrinklers and switch to clicking,
but doing so comes at a steep price.
Sisyphus heart sinks as he realizes what he has to do.

Godzamok can only reach its full potential by sacrificing buildings over and over again,
so Sisyphus has to repeatedly cast Stretch Time to get the buff "Devastation" to stick forever.
Previously Sisyphus had abused the GFD refund mechanic to do so,
but now his only hope is to get a streak of caramelized sugar lumps
and only load the save a few instants every day.
The gods,
however,
have forbidden Sisyphus from importing save files.
The only way of achieving this is thus by closing the game,
and having the browser do the save game loading for Sisyphus.

And what brings tears to Sisyphus eyes is the Birthday Cookie.
We are long past the year 275760,
the last year in which the boost from the Birthday Cookie can be computed correctly.
If Sisyphus closes and opens the game now,
the game code returns `NaN` (Not-a-Number) for the CpS boost given by the Birthday Cookie.
And `NaN` is violently destructive:
any mathematical operation involving `NaN` returns another `NaN`,
which would irreversibly corrupt Sisyphus's save file.
Closing and opening the game _will mangle the Birthday Cookie beyond recognizability_,
and Sisyphus would have to get rid of it to save his save game.

"Please, gods, no!"
The Birthday Cookie is giving Sisyphus a CpS boost of 273747%.
It is the third most powerful upgrade that Sisyphus has access to,
behind only the Heavenly Chip Secret
(which unlocks the multiplier from prestige levels)
and A Crumbly Egg
(which unlocks Krumblor).
But in contrast with the top 2,
which are only available after ascending,
the Birthday Cookie has been a loyal companion of Sisyphus
basically since the beginning.

Could Sisyphus have reached this stage before the year 275760 and saved the Birthday Cookie?
No, of course not.
That is not nearly enough time to level up all buildings,
or to max out Godzamok.
Sisyphus cannot have simultaneously maxed-out building levels,
maxed-out Godzamok,
and maxed-out Birthday Cookie.
_One of these three have to go_.

Tears roll down his eyes as he stares the Birthday Cookie.
The resolve in Sisyphus's heart speaks louder, just barely.
He ascends,
and for the first time ever,
the Birthday Cookie is not purchased.

Sisyphus gently places his companion in the vault,
and,
with the last bit of his resolve,
he closes the game.
The Birthday Cookie,
the third most powerful upgrade in Sisyphus's arsenal,
is permanently lost to the sands of time.

The corpse of the upgrade,
a "Click to purchase" button in the vault,
will forever stare at Sisyphus's soul,
reminding him of this sacrifice.
One must wonder if Sisyphus can ever be happy again.


Clicking the damn cookie
------------------------

The show must continue.

The cookie shall be clicked.

Sisyphus gets multiple caramelized sugar lumps in a row,
and stretches Godzamok to eternity.
Frenzy, Click Frenzy, Elder Frenzy, Sugar Frenzy, Dragonflight, Dragon Harvest, Building Specials.
Even the bank loans are stretched to permanency.

Sisyphus still uses the Grimoire to cast `GFD->FtHoF`,
but this time all 14 golden (and wrath) cookies from Force the Hand of Fate are left on-screen.
With the two naturally-spawned golden cookies,
Sisyphus can get 16 on-screen golden cookies for Dragon's Fortune;
slotting Reality Bending is already more powerful than simply using Radiant Appetite.

"Sacrificing the Birthday Cookie was worth it. I think. I hope."
Sisyphus reaches [80.019 septenoctogintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-3.cki),
with 8.863 unnonagintillion cookies baked all time.


Distilling luck to get more golden cookies
------------------------------------------

Sisyphus has stacked every conceivable buff he could possibly get,
and all additional multipliers
(like the Golden Switch, Shimmering Veil, slotted Temple spirits and so on)
have already been activated.
The only thing left is to somehow get more golden cookies on-screen,
to power Dragon's Fortune.

Sisyphus can get 14 golden cookies from the Grimoire.
The natural golden cookie spawn gives him another one,
and if Distilled Essence of Redoubled Luck (DEoRL) triggers,
Sisyphus gets a 16th on-screen golden cookie.
But he can go further.

DEoRL works as follows.
Every game tick (which happens 30 times per second)
the game has a chance of spawning a golden cookie.
(In the code, this is the responsibility of the function `Game.updateShimmers()`.)
This chance increases with time,
being 0 below the minimum GC spawn time
and growing all the way to 1 (100%) when it reaches the maximum GC spawn time.
If that chance triggers,
the game spawns a golden cookie, and marks it as a "spawnlead";
then if the player has DEoRL,
there is a 1% chance of the game spawning an additional golden cookie.
DEoRL may only trigger during the normal spawning cycle,
so there is no risk of the DEoRL-spawned golden cookie triggering DEoRL again.
When the player clicks the spawnlead,
or it despawns naturally after its lifetime has passed,
the game restarts the algorithm.

This means that,
if DEoRL triggers and Sisyphus clicks just the spawnlead,
Cookie Clicker will start trying to spawn another golden cookie with the redoubled GC still on-screen.
If the next GC spawn also triggers DEoRL,
Sisyphus now has three golden cookies on-screen.
He may then click on the spawnlead,
which starts the countdown again.
This is only limited by the lifetime of golden cookies and how fast they can spawn.

The initial lifetime of golden cookies (how long they stay onscreen)
is 13 seconds.
Lucky day and Serendipity each doubles it,
and Decisive fate, Lucky digit, Lucky number, Lucky payout slightly lengthens that further;
this brings the duration to 56.26 seconds.
The garden plant Green Rot could also be used;
a garden full of nursetulips and greenrots multiplies that by about 32%,
reaching 74.26 seconds.
(If the "ghost nursetulip" glitch is used,
we can replace mature nursetulips with bud green rots
to get these numbers to 33.6% and 75.2 seconds, respectively.)
Each on-screen golden cookie multiplies their lifetime by 0.95,
but as long as the spawn cycle is longer than 3.76 seconds,
the bottleneck will be the lifetime of the very first golden cookie.

The minimum spawn time for golden cookies starts out at 5 minutes.
Just with upgrades
(Lucky day, Serendipity, Golden goose egg, Heavenly luck, and Green yeast digestives),
this number goes down to 67 seconds.
A garden full of nursetulips and golden clovers divides this number by 2.92,
bringing this number down to 22.94 seconds.
Finally,
Sugar Blessing (from golden lumps) reduces this by 10%,
and Krumblor's Arcane Aura multiplies this by 0.95, or 0.945 if accompanied by Reality Bending.
The fastest GC spawning time is thus 19.53 seconds.
(Using the "gost nursetulip" glitch,
the factor from the garden becomes 3.02 instead of 2.92,
slightly shortening this to 18.9 seconds.)
With a lifetime of 56.26 seconds,
Sisyphus can squeeze 2 additional spawn cycles before the first golden cookie despawns,
**totaling 4 on-screen golden cookies**.
- The first golden cookie actually has a lifespan of 56.45 seconds,
  because it only spawned after the spawnlead had already been spawned.
- Some seasons have heavenly upgrades that also decrease golden cookie spawn times;
  the strongest one is Startrade,
  which multiplies them by 0.95 during April fools.
  Selebrak further reduces spawn times as well,
  but again only during seasons.
  This would reduce the minimum GC spawning time to 17.73 seconds,
  which would allow Sisyphus to get 5 on-screen golden cookies.
  The problem is that the code for naturally-occuring seasons has long stopped working,
  so the only way of entering a season is through the Season Switcher biscuits.
  But each time that the season sitcher is used,
  its price increases by 50% (multiplicatively),
  so after a few thousand season switches it is impossible for Sisyphus to further change seasons.
  (Sisyphus could sell all his buildings and not purchase the upgrade "egg",
  but that's not compatible with the buildings-as-reservoirs trick.)

Sisyphus has investigated a potential avenue for shortening golden cookie spawn times further:
cookie chains.
Each golden cookie has 5-10% chance of starting a cookie chain,
and during cookie chains,
the game forces the golden cookie spawn time to be exaclty 3 seconds.
It also shortens the lifetime of golden cookies to below 2 seconds
(the first 5 golden cookies in a chain will last 2 seconds;
the 6th lasts 10/6 seconds, the 7th lasts 10/7 seconds, and so on),
so abusing cookie chains is tricky.
Sisyphus could do so as follows.

First, have Distilled Essence of Redoubled Luck trigger,
click the spawnlead,
and have it be a chain cookie.
Since the spawnlead was clicked,
the countdown to the next golden cookie starts,
and it is fixed at 3 seconds due to it being a cookie chain.
Have Distilled Essence of Redoubled Luck trigger again,
spawning two more golden cookies.
These GCs are short-lived;
we click the spawnlead (so that the timer to the next GC is still 3 seconds)
but let the second one despawn (forcefully ending the cookie chain).
A non-spawnlead despawning does not update the GC timers,
so the next spawn cycle happens 3 seconds later;
but the cookie chain has ended,
so the lifetime of the next golden cookies will be normal.
We then repeat the cycle again,
gaining one extra on-screen golden cookie every 6 seconds.

This can be further shortened by cookie storms.
While a cookie chain is active,
clicking any golden cookie with a predefined effect
(like the ones from Force the Hand of Fate, or the ones from a cookie storm),
or letting any golden cookie despawn,
also forcefully ends the chain.
So we don't have to wait until the next spawn cycle to break the chain,
we can simply click a cookie storm GC;
this gives us one new on-screen golden cookie every 3 seconds.

With the maximum lifetime of 74.26 seconds,
we can repeat this cycle 24 times,
for a total of 26 on-screen golden cookies
(the initial non-spawnlead GC,
23 non-spawnlead GCs from the cycle repetitions,
plus 2 GCs from the last cycle).
With Dragon's Fortune and Reality Bending,
these additional 22 GCs (compared to not abusing cookie chains)
would give Sisyphus a multiplier of `2.353^22`,
which is about `1.5e8`.

Unfortunately,
this "cookie chain storm abuse" has a fatal flaw:
the chain breaking in the first golden cookie.
Normally,
each golden cookie in a cookie chain gives the player 10 times more cookies than the previous one.
The chain ends if the _next_ golden cookie would give the player
more than 6 hours worth of CpS, or more than 50% of the bank
(whichever is lower).
(There is also a 1% chance of it ending at any point, but this is not relevant for us.)
Cookie chains starts giving cookies corresponding to roughly the current bank divided by `10^10`.
But Sisyphus banks are _massive_.
With a maxed-out Godzamok, and Click Frenzy and Dragonflight,
a single click gives Sisyphus `2.65e23` times his CpS,
placing him way past the 6-hour-cap.
Hence cookie chains will always break in the first golden cookie,
_meaning that this trick only works once_.
This trick only gives sisyphus a multiplier of `6.37e7`,
which is much smaller than the `9e15` factor that Sisyphus gets
simply by executing the same combo over and over again.
(This is the same reason why Sisyphus cannot make use of
the golden cookie spawned by the Fortune Cookies upgrade.)

Hence Sisyphus can only get an additional 4 on-screen golden cookies.
Together with the 14 golden cookies spawned by the Grimoire,
they only get Sisyphus to 18 on-screens.
Resorting once more to the buildings-as-reservoirs trick
(including the variant to maximize the power of building specials),
Sisyphus reaches [1.309 octooctogintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-4.cki)
(`1.3089779436036037e267`, about `2^887.34`),
with 18.588 trenonagintillion cookies baked all time
(`1.858771135597229e283`, which is `2^941`).

Had Sisyphus been able to add the Birthday Cookie to his army of upgrades,
he would have gotten 5 orders of mangitude further,
reaching [664.553 novemoctogintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-5.cki)
(`6.645525395128818e272`, about `2^906.3`),
with 9.745 quinnonagintillion cookies baked all time
(`9.7453140114e288`, which is `2^960`).


Sisyphus buckles; the boulder falls
-----------------------------------

At long last,
Sisyphus meets his ultimate fate.
It took him vigintillions of years to reach septenoctogintillion cookies;
and this is a milestone he cannot surpass.

Sisyphus can try as much as he want.
After every ascension,
he starts climbing the mountain again,
making progressively more cookies,
but as he reaches septoctogintillion cookies yet again,
his multipliers fail him;
his buffs go past diminishing returns and fully stagnate;
the "cookies baked all time" counter is fully saturated,
whence his prestige level is unchanging.

Sisyphus ascends.
The metaphorical boulder rolls back again.
Our hero fruitlessly repeats the task,
unable to progress any further.
The gods have won;
Sisyphus has merely replaced the boulder with a cookie.

One must wonder if Sisyphus is truly happy.


The Companion GitHub Repository
===============================

One last time,
the companion repository has 5 save files.
- [Save 1: 1.559 unseptuagintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-1.cki),
  just by using prestige levels.
- [Save 2: 31.853 unoctogintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-2.cki),
  by stacking multipliers from the Shimmering Veil, Golden Switch, Sugar Frenzy,
  and using Distilled Essence of Redoubled Luck and Krumblor to get more golden cookies.
- [Save 3: 80.019 septenoctogintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-3.cki),
  after letting go of the Birthday Cookie and using Stretch Time to go back to using Godzamok.
- [Save 4: 1.309 octooctogintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-4.cki),
  by using Distilled Essence of Redoubled Luck to get 3 extra on-screen golden cookies,
  and squeezing a bit more strength from Building Specials
  by doing the variant of the buildings-as-reservoirs trick.
- [Save 5: 664.553 novemoctogintillion cookies](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus5-5.cki),
  which is what Sisyphus would get if he could also use the Birthday Cookie.
