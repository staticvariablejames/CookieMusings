Sisyphus Plays Cookie Clicker IV: The Olympus Council
=====================================================

This is the fourth of a [series of articles](./README.md#sisyphus-plays-cookie-clicker)
investigating the limits of Cookie Clicker.
[In the previous article](./sisyphus-plays-cookie-clicker-1-ieee754.md),
Sisyphus hit the absolute limit on the number of cookies obtainable without ascending.
In this article,
rules will change.

The Council Meeting
-------------------

"With this,
I declare started the 308th Council Meeting.
As a reminder,
Sisyphus's sentence was commuted to playing Cookie Clicker in the Underworld.
On his behalf,
Asopus has submitted an appeal to allow Sisyphus to 'ascend in Cookie Clicker'.
As the Underworld overseer,
I,
Hades,
am presiding the council today,
and this meeting's goal is to decide whether to allow Sisyphus to ascend,
and if so under which conditions."

"We cannot allow that", complained Zeus.
"One cannot simply _ascend_ from the Underworld.
He must stay as he is,
without being allowed to ascend."

"'Ascending' is just a term", Hera counterargued.
"Is a mechanic of the game.
Sisyphus will stay right where he is,
clicking cookies in the Underworld."

"No, no; no ascension of any kind!
He will get heavenly chips and heavenly upgrades,
and there is nothing 'heavenly' about people serving sentences in the Underworld!"

"These are just _terms_, Zeus.
In the game code,
ascending is even called 'reset' instead of 'ascend'.
That is a weird choice,
because it meant that an actual reset had to be renamed to 'wipe save',
but I digress.
Are we really forbidding Sisyphus from _ascending in Cookie Clicker_ just because?"

"No, it is, uh, well, ..."
Zeus was still clearly salty about Sisyphus outsmarting him that one time.
Apollo interrupted before he could whine any further.

"The problem is the dragon", started Apollo.
"When Sisyphus ascends in Cookie Clicker,
he will be allowed to purchase Heavenly upgrades.
Among them is How to bake your dragon,
which gives Sisyphus access to the cookie dragon Krumblor.
Among the auras Krumblor can have,
the truly troublesome one is Dragon's Fortune.
It multiplies CpS by 2.23 per on-screen golden cookie.
Do you all recall how Sisyphus abused Gambler's Fever Dream so hard
that time nearly stopped?
If he picks Force the Hand of Fate instead of Stretch Time,
Sisyphus will be able to amass a large quantity of on-screen golden cookies.
If we allow Sisyphus to ascend,
he will turn on Dragon's Fortune,
get hundreds of on-screen golden cookies,
and immediately reach Infinity."

"Yes! That's the problem!" Zeus felt vindicated.
"As soon as he hits a cookie storm,
he will get Infinity cookies,
and we will be forced to release him!"

Hades sighed and took the word back to himself.
"Dragon's Fortune does not work with cookie storms, Zeus.
It seems thus that the problem is not ascending per se,
but the interaction between Dragon's Fortune and Gambler's Fever Dream.
We could just ban one of those, for example."

Apollo's opinion was that
"we all know how badly coded is Gambler's Fever Dream.
I say we ban that spell."

Athena disagreed.
"I think we can engineer a more delicate restriction in this case.
We all have read [Static Variable James's analysis on Gambler's Fever Dream](./sisyphus-plays-cookie-clicker-3-gfd.md#bugs-bugs-bugs),
the problem of that spell is that its code is asynchronous.
Most of the bugs disappear if Sisyphus does not touch the game state between cast and resolution.
And the other major bug,
scrying,
will not affect him anyway due to the fact that he already cast 9 quadrillion spells."

"Uhh so we forbid Sisyphus from touching anything for 1 second after casting GFD?"

"Yes,
or even just forbidding him from changing the parts of the game state relevant to the Grimoire,
like casting other spells or selling Wizard towers."

"Sounds good", said Hades.
"This should cover all of our bases regarding the dragon."

"Not quite, but we need to discuss something else first."
Hermes started talking.
"I went to talk with grandfather Cronus before this meeting.
Grandpa has been much grumpier since the Renaissance.
You see, people keep lumping him together with Chronos,
and so now grandpa is also responsible for this whore _time_ thing.
But that's great to me,
because this means we can talk with someone that understands _time_
with just a quick trip to the Tartarus!"

"Please get to the point, Hermes."

"You know how there's some heavenly upgrades that generates cookies even offline?
Well,
these upgrades are very weak,
but they are not _capped_.
If Sisyphus waits for a year,
he will get a year's worth of resources.
If he waits a decillion years,
he will get a decillion years' worth of resources.
You see,
he can just idle for one centillion years,
and easily get over `1.8 * 10^308` cookies this way."

"Indeed,
we most definitely don't want Sisyphus winning by doing nothing.
What is the proposed solution?"

"Simply make him play once per day!
You see,
'time offline' is an uncapped multiplier that is not limited by IEEE754 precision loss issues,
so we can simply cap that ourselves.
Also,
Sisyphus will want to play at least once per day anyway,
to collect his sugar lump,
so demanding him to play once a week, for example, makes no sense."

"I don't think the sugar lump will matter", said Hades,
"because Sisyphus is already at the limit of how many sugar lumps he can collect.
But the once-per-day rule sounds reasonable."

Cronus actually thought that forbidding Sisyphus from closing the game would be simpler,
but he convinced Hermes that this once-per-day rule would make things "more interesting".
So he didn't even mention this alternative in the council.

"Hermes, you still had to say something about the dragon."

"Oh yes, I almost forgot!
You know how seasons don't work anymore in Cookie Clicker,
because the `Date` class stopped working in the year 275760?
Well,
the upgrades from petting the dragon have the same problem,
they also use the `Date` class,
so they don't work anymore.
So grandpa Cronus gave Sisyphus this tiny bit of time-travel magic,
which will allow him to get those upgrades once again."

"Won't Sisyphus lose these upgrades anyway once he ascends again?" asked Apollo.

"Not quite!" answered Hermes.
"There are only four of those upgrades.
Sisyphus can stuff them into the permanent upgrade slots,
and still have one to spare!"

"Fair enough" said Hades.
"OK, we have adressed the dragon and the idling.
I think we are done?"

"Hold on" interrupted Athena.
"I think you must be made aware of Hermes's shenanigans with heralds."

"You got me.
As a herald myself, I'm a fan of heralds,
of course I would do something with heralds!
You know,
even Cookie Clicker has heralds.
For every 100 people playing Cookie Clicker on Steam,
the game adds one herald;
and each herald gives +1% CpS multiplier.
But since the year 2333,
Valve has ceased to exist (the year had too many 3's in it),
so the number of heralds in Cookie Clicker has been stuck at 0.01 since then."

"Get to the point."

"Well, I got my hands on Valve's server infrastructure,
and got it working,
and added 10000 copies of Cookie Clicker always running!
So Cookie Clicker now always has max heralds."

Zeus had to whine one last time.
"Why are you helping Sisyphus? You can add more heralds now and he will get to Infinity cookies!"

But Hades was not impressed.
"I think we will be fine,
as heralds are limited to a +100% CpS bonus.
And since we debated everything,
I therefore conclude this meeting."

---

Hades summarized Sisyphus restrictions as follows.

1. After casting Gambler's Fever Dream,
   and before it resolves,
   Sisyphus may not alter the game state in any way that may interfere with the spell's resolution.

2. Sisyphus is required to open his game at least once per day.

3. Sisyphus is permitted to time-travel once,
   in order to acquire the four upgrades obtained from petting the dragon.

4. Hermes's shenanigans are in effect,
   fixing Heralds at a +100% CpS boost.

Under these restrictions,
Sisyphus is now allowed to ascend in Cookie Clicker.
This event will take place in the next article.
