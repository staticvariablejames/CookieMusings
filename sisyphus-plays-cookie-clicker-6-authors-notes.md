Sisyphus Plays Cookie Clicker VI: Author's Notes
================================================

This is the afterword of a [series of articles](./README.md#sisyphus-plays-cookie-clicker)
investigating the limits of Cookie Clicker.
I will provide some context, justification, historical notes, and general thoughts
concerning this series of articles.


The Concept of Hardcap
----------------------

A "cap" is an upper limit on something.
"Soft cap" is a point at which further growth is substantially diminished,
and "hard cap" is a point at which further growth is impossible.

For example,
let us imagine that we added an upgrade for Cookie Clicker that boosts CpS
based on the number `r` of reindeer clicked this ascension,
as follows:
- If `r` is smaller than 1 million, multiply CpS by `r`.
- If `r` is between 1 million and 1 trillion, multiply CpS by `sqrt(r) * 1000`.
- If `r` is bigger than 1 trillion, multiply CpS by 1 billion.

This upgrade has a clear hardcap of 1 billion,
and has a clear softcap of 1 million.
But what if we remove the third condition,
and let the second condition also hold beyond 1 trillion?
In this case,
this upgrade does not have an explicit hardcap anymore,
but because our programming language is JavaScript,
that upgrade is subject to its limitations.

There is the obvious limit of 1.8e308 for any number in JavaScript,
but (as Sisyphus figured out very early in his journey)
simple counters cannot go past `2^53`,
which means that the number of reindeer clicked will never count past this value.
Hence our upgrade above caps off at `sqrt(2^53)*1000`,
which is about 9.49e10.
And because this limit is theoretically reachable
(by clicking `2^53` reindeer in a single ascension)
this is the hardcap of this upgrade.

There is another kind of hardcap which is imposed by its mathematical properties.
For example,
we could have a bonus multiplier that starts at 1,
and each game tick it is divided by 2 and then increased by 1.
This multiplier will slowly increase over time,
getting progressively closer to 2,
but never surpassing it.
[This kind of hardcap happens in the stock market minigame.](./stock-market-hard-cap.md).

In my opinion,
these categories of hardcap are much more interesting than the ones directly imposed by the code.
These limits are not designed by the author,
but rather _imposed on_ by the nature of the system in consideration.
In a sense,
this series of articles essentially talks about the hardcap of Cookie Clicker,
including,
of course,
lengthy discussions about the behavior of the IEEE754 floating-point specification.

"Softcap" also admits the meaning of "point after which we experience diminishing returns".
For example,
if we remove the first condition in our hypothetical reindeer-based boost,
so that the upgrade always boosts CpS by `sqrt(r) * 1000`,
then there is no explicit softcap in the code anymore,
but players will still experience much slower growth at the 1 million reindeer mark
compared to when they just started.
But this multiplier's growth already started slowing down since the beginning,
so one may claim that the softcap happens earlier.
This notion is much more nuanced,
and I avoided discussing it in my series of articles.


A Brief History of the Cookie Clicker Hardcap Calculation
---------------------------------------------------------

Cookie Clicker was initially published in August 8th, 2013,
and its prestige system debuted in September 8th, 2013.

The earliest calculation I was able to find about the Cookie Clicker hardcap
was done by the user `MasterSparky`,
in [this 2014 forum post](https://web.archive.org/web/20190618180522/http://forum.dashnet.org/discussion/4701/there-is-a-theoretical-maximum-amount-of-cookies)
in the now-defunct <http://forum.dashnet.org/>.
Cookie Clicker was on version 1.0465 at that time;
it only had 11 buildings,
no minigames,
no Krumblor,
no heavenly upgrades as we know them today.
(The version 1.0466 (a small patch) is still playable today,
in <https://orteil.dashnet.org/cookieclicker/v10466/>.)
It does have wrinklers and golden cookies, though;
`MasterSparky` has estimated the hardcap to be at around vigintillion cookies for this version.

Prior to this series of articles,
the current calculation of Cookie Clicker's hardcap had been done by Discord user `lookas123`.
[In 2020 he had estimated it to be around `1e254`](https://discord.com/channels/412363381891137536/412371320232345624/751556516074487859)
(when Cookie Clicker was on version 2.029)
and a more detailed calculation of `2^922 = 3.545325e277`
[was posted in 2024](https://discord.com/channels/412363381891137536/412371320232345624/1240053792251318335)
(for Cookie Clicker 2.052).

And finally,
this series of articles,
posted in 2026,
reaches 3e280 cookies baked all time,
or 1e284 if using the Birthday Cookie.


Time Travel Troubles
--------------------

This series of articles stemmed from my work on [Cookie Connoisseur](https://github.com/staticvariablejames/cookie-connoisseur),
a framework for testing Cookie Clicker mods.
This framework uses [Playwright](https://playwright.dev/)
to launch browsers and play Cookie Clicker in a scripted/automated manner,
thus creating repeatable test scenarios for mods.
And a fundamental ability of Cookie Connoisseur
is to mock (simulate) the date at which the script runs;
for example,
by default,
all Cookie Connoisseur scripts run with the date set to September 13th, 2020.

In fancy words,
Cookie Connoisseur can time travel to any point in time,
including going all the way up to the year 275760
(the year past which `new Date()` stops working).
I can time travel this much into the future,
and automatically and repeatedly perform any action a player could perform in Cookie Clicker;
why can't I _simulate_ the hardcap,
instead of just estimating it?
And thus Sisyphus Plays Cookie Clicker was born.

In technical terms,
Cookie Connoisseur achieves time travel by overwriting the global `Date` object.
`Date.now` is replaced with a function that calls the original `Date.now`
and appropriately shifts its return value;
`new Date` calls the original `Date` constructor,
but passing the timestamp calculated by the overwritten `Date.now`.
This means that Cookie Connoisseur can travel to any timestamp
representable by a IEEE754 floating-point number,
going way past the year 275760.
It can even accurately simulate the centillion-years idling feared by Cronus
[in the fourth article of the series](./sisyphus-plays-cookie-clicker-4-rules-change.md).

However, there is a problem with that:
_it makes Cookie Connoisseur not standards-compliant_.
[The ECMAScript Standard has the concept of _time value_](https://tc39.es/ecma262/multipage/numbers-and-dates.html#sec-time-values-and-time-range);
they are integers representing the number of milliseconds that have passed
since the midnight in the beginning of January 1st, 1970.
IEEE754 floating-point numbers can represent all integers between -2^53 and 2^53
(about -9.007e15 to 9.007e15),
and beyond that range all representable numbers are integers;
but the ECMAScript standard restricts time values
to be between -8.64e15 and 8.64e15,
which is slightly shorter than the interval where all integers can be represented.
(8.64e15 milliseconds is 100 million days,
so it is a "round number" in this sense.)
`new Date(8.64e15)` is a valid `Date` object,
but `new Date(8.64e15+1)` is not.
I felt that the standard wording to be a bit confusing,
as it phrases this shorter range as if it were a consequence some earlier statement,
but it feels to me that the intention is that a time value must be between -8.64e15 and 8.64e15.
Since time values must not go over `8.64e15`,
and `Date.now()` returns a time value,
beyond the year 275760 the output of `Date.now()` must always be `NaN`,
making Cookie Connoisseur's overwriting non-standards-compliant.

Not being fully standards-compliant is mostly a [broken-window theory kind of problem](https://en.wikipedia.org/wiki/Broken_windows_theory).
If we are not being standards-compliant in this point,
what is preventing us from being non-standards-compliant in other aspects?
We could replace the 64-bit IEEE754 floating-point numbers with 128-bit IEEE754 floating-point numbers,
or with a number representation that increases in precision as time passes,
and thus the entire concept of a hardcap goes out of the window.
So I want to stay standards-compliant as much as possible.

Of course,
_Sisyphus Plays Cookie Clicker_ is a work of fiction,
so I can change some rules compared to reality.
For example,
computers can definitely last decades without turning off,
but there is no way a computer fabricated today
will still be running one hundred thousand years from now.
Yet Sisyphus's gaming PC is still going strong vigintillions of years later.
Still,
I strived to be as "close to reality as possible";
I wanted to keep Cookie Clicker as-is,
just with infinite time.

Hence I had to "patch" the ECMAScript standard
to allow `Date.now()` to keep working beyond the year 275760.
But if `Date` objects are allowed to go past this limit,
the Birthday Cookie will continue to increase forever,
making it very easy for Sisyphus to reach Infinity cookies.
This obviously makes for a very uninteresting series of articles;
so I have choosen to just allow `Date.now()` to work past the limit,
and keep `Date` objects fully standards-compliant.
("Eh the stardard is kinda imprecise anyway about `Date.now()`,
that function is not _really_ part of any `Date` object,
I can pretend I did not read all that,
it will be fine if I shove this detail under the rug.")
Furthermore,
essentially this is the behavior of Cookie Connoisseur,
so I don't need to do any further work here.

I could,
of course,
simply mandate that Sisyphus must achieve his goal before the year 275760.
This is not nearly enough time for any counter to reach `2^53`,
so _Sisyphus Plays Cookie Clicker_ would need a very different approach.
Instead of just asking the biggest possible combo,
Sisyphus would have to worry about how often the combos can be carried out.
For example,
DEoRL only triggers 1% of the time,
and getting an extra on-screen GC only multiplies the combo strength by a factor of 2.353;
Sisyphus would be better served by repeating a combo 100 times
than by getting DEoRL to trigger once.
Can Sisyphus use planners?
Using e.g. [CYOL](https://github.com/staticvariablejames/ChooseYourOwnLump)
speeds up the rate at which Sisyphus accumulates caramelized lumps by a factor of 50.
What about savescumming?
Ultimately,
this means I cannot use this series of articles
as an excuse to talk about IEEE754 floating-point numbers,
which I deemed uncacceptable :)


Reining in Sisyphus
-------------------

The Birthday Cookie was not the only threat:
there are two other ways of getting to Infinity cookies,
namely through idling,
or by abusing GFD to get unlimited on-screen golden cookies.
These two had to be "patched out of existence" for this series of articles to remain interesting.

The "GFD patch" is also inspired by a previous work of mine:
the mod [Spiced Cookies](https://github.com/staticvariablejames/SpicedCookies#patch-the-delay-in-gamblers-fever-dream-disabled-by-default)
has an option to remove the 1-second delay between the cast and resolution of GFD.
This patches every single bug mentioned [in the third article of the series](./sisyphus-plays-cookie-clicker-3-gfd.md#bugs-bugs-bugs),
because now GFD is not asynchronous anymore.
The closest thing in-game is what I adopted,
by forbidding Sisyphus from changing the game state between cast and resolution.
This also roughly corresponds to the "intended usage" described in the third article,
as I did not want to simply ban GFD altogether.

There were other ways of preventing a limitless amount of on-screen golden cookies,
like putting a cap on how many unresolved GFD casts Sisyphus could have at any given moment,
or limiting how much magic Sisyphus can regenerate before he's required to resolve all casts.
But these rules felt too contrived and unnatural,
so I opted to "pretend that GFD is synchronous".

I also had multiple ways of preventing idling.
The obvious alternative is to forbid Sisyphus from closing the game at all,
but since GFD was substantially nerfed,
this would prevent Sisyphus from stretching Godzamok to infinity.
(It would actually have killed Godzamok,
as even a TAS cannot sell more than a few million buildings in a second,
so clicking would have been weaker than wrinklers.)

The solution I adopted does have the downside that I cannot keep the Birthday Cookie.
Alternatively,
I could have allowed Sissyphus from exporting his save and manually importing afterwards;
but that'd mean I would need to make Sisyphus pinky-promise he won't modify the saves himself,
which felt way too janky.
In any case,
I computed how many cookies Sisyphus would have gotten were he able to keep the Birthday Cookie.


Leftover Multipliers, Future Updates
------------------------------------

Of course,
this series of articles does not compute the hardcap of Cookie Clicker;
not only I had to forbid some ways of playing the game,
fundamentally these articles only present a very large number that can be achieved,
and which I was unable to increase further.

I believe I have accounted for every single multiplier that Sisyphus is able to get in Cookie Clicker.
The only room for improvement I know exists
is in the [buildings-as-reservoirs trick](./sisyphus-plays-cookie-clicker-3-gfd.md#the-final-trick-buildings-as-reservoirs).
Say that we are now selling Grandmas to purchase Farms.
Each new farm costs 15% more than the previous one,
so it could be that we have 10% more cookies than the previous farm
and those additional cookies are "wasted".
But it could be that Mines are only 8% more expensive than the previous farm,
so it is worthwhile to go from Grandmas to Mines instead,
and come back to farms later.
This increases the cookies available in the reservoir by up to 15%;
with 20 buildings,
thats a factor of `1.15^19 =~ 14.23`
(or `1.15^17 =~ 10.76` if we account for Sisyphus not using Wizard towers or "You"s in the trick).
So if the order I have chosen is the worst possible one,
we could a priori make Sisyphus bake an order of magnitude more cookies.
There are `20! = 2.43e17` building orders,
though,
so checking all of them is unfeasible
and I did not bother trying to do something fancier like gradient descent.

[The companion GitHub repository](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/)
has the scripts that I used to produce the save files.
Of course,
I may have missed something,
or the scripts may be incorrect;
feel free to check for yourself and prove me wrong :)

Finally,
having a script means that it can easily be run again
when (if?) Orteil publishes the next Cookie Clicker update.
