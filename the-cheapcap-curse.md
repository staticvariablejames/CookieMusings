The Cheapcap Curse
==================

This is a game design article,
talking about one specific problem regarding Cookie Clicker's ability to sell buildings.

Advice Summary
--------------

If your mod has some effect that change the prices of buildings,
those effects must be _permanent_
(i.e. only removable by ascending).

History: The Cheapcap Exploit
-----------------------------

Back in 2018,
Cookie Clicker introduced its third minigame,
the garden.
It went live on Cookie Clicker version 2.01,
from 2018-04-18,
and introduced a brand new "strategy":
[the cheapcap exploit](https://old.reddit.com/r/CookieClicker/comments/8g0mcg/the_most_broken_strategy_in_the_history_of/).

It works like this.
1. Slot the Earth Shatterer aura on Krumblor,
   plant a full 6x6 garden of alternating rows of nursetulips and cheapcaps,
   and set the soil to clay.
2. The plants give in total a 12.06% discount when purchasing buildings,
   so if we were to spend `x` cookies purchasing buildings,
   we now spend only `0.8794 * x` cookies instead.
   Purchase as many buildings as we can.
3. Now freeze the garden, thus disabling the cheapcaps.
   Now the buildings we got on step 2 would cost again `x` cookies.
4. Sell those buildings.
   With Earth Shatterer,
   we would normally get 85% of the sale price back.
   However,
   due to a bug in the game,
   the refund factor of Earth Shatterer applies to the cost of the _next_ building,
   instead of the current one.
   Since buildings get 15% more expensive with each additional unit,
   the game refunds us for `0.85 * 1.15 * x` cookies, rather than just `0.85 * x` cookies.
5. So we spent about `0.8794 * x` cookies and got `0.9775 * x` cookies,
   a relative gain of 11.16%.
   If we spent all our bank on those initial `0.8794 * x` cookies,
   we just increased our bank by 11%.
   I.e. we multiplied our bank by about `1.1`.
   _If we repeat these steps, this increase compounds_;
   repeating this process 8 times more than doubles the bank,
   and repeating this process 25 times adds a zero to your bank account.
   
These gains are uncapped,
meaning that we can keep doubling the bank to legitimately get to `10^303` cookies.
Past this point,
Cookie Clicker simply displays `Infinity cookies` below your bakery name.
In other words,
this is a legitimate way of getting infinity cookies,
something [not even Sisyphus was capable of doing](./sisyphus-plays-cookie-clicker-5-ascension.md#sisyphus-buckles-the-boulder-falls).

But alas,
this exploit is no more.
Cookie Clicker 2.0109,
which went live on 2018-08-01,
gave cheapcaps a chance of dying when the garden is frozen
(ensuring that the building discount disappears after a few freeze-unfreeze cycles)
and nerfed Earth Shatterer to only refund 50% of the purchase price of buildings.
(Accordingly, the refund without the aura went from 50% to only 25%.)
The bug of applying the refund factor to the _next_ building,
rather than the current,
was not patched,
and still exists on Cookie Clicker 2.058.

Current State of Affairs
------------------------

The fundamental problem is the ability to sell buildings at a higher price than they were bought for.
Let me introduce two variables.

- `refundFactor`: the fraction of the cookies that we are refunded when selling a building.
  Its default value is intended to be `0.25`,
  but it ends up being `0.2875` due to the bug of using the price of the next building instead.
  With Earth Shatterer and Reality Bending,
  it goes up to `0.60375`.
  - Reality Bending also decreases building prices by 0.2%
    (due to Fierce Hoarder),
    so in practice we don't get exactly this 60.375% refund,
    but that's a close enough approximation.

- `dynamicPurchaseFactor`: i.e. `1 - dynamicBuildingDiscount`.
  This is the fraction of the number of cookies we must pay when purchasing a building,
  but only taking into account things that can be removed.
  For example,
  a full garden with nursetulips and cheapcaps brings this down to about `0.8794`,
  but purchasing the Faberge egg ("buildings and upgrades are 1% cheaper")
  keeps this value at `0.8794` because this discount cannot be removed
  (i.e. it is not _dynamic_).

I believe that the highest discount that cheapcaps can give is when planting a 6x6 garden with clay soil
and alternating rows of cheapcaps and nursetulips.
The exact value of `Game.effs.buildingCost` is `0.8793644627500736` for this case.
If the ghost nursetulips bug is used
(harvesting the nursetulips and planting bud cheapcaps,
which keeps the plant-boosting effect of the just-harvested nursetulips until the next game tick)
we get the slightly lower value of `0.8736114716248926`.

The other dynamic building discounts are
1. the Fierce Hoarder aura (buildings are 2% cheaper, or 2.2% with Reality Bending)
2. the Everything Must Go effect (buildings are 5% cheaper, obtainable from golden cookies during April fools)
3. the Summon Crafty Pixes spell from the Grimoire (buildings are 2% cheaper)
4. the Dotjeiess spirit from the Pantheon (up to 7% discount, when worshipped in the diamond slot).

Together,
these reduce the `dynamicPurchaseFactor` to `0.7446297698616725` (i.e. 74.46% of the full price),
or `0.7397582420264871` if we use the ghost nursetulips bug.

_And these discounts are pitiful_.

Recall that buildings get 15% more expensive with each purchased unit.
This means that, if we are only paying a factor of `r` of the buildings,
then our cookies are "worth" `1/r` more,
and we get `log_1.15(1/r)` additional units of that building.
And `log_1.15(1/0.7397)` is just over 2.15,
meaning that we have to jump through all the hoops described above
(cheapcaps + nursetulips, Fierce Hoarder, Everything Must Go, Summon Crafty Pixies, Dotjeiess)
to get _two additional units_ of each building.

Even when directly considering the equivalent CpS boost of `1/r`,
all these buffs combined correspond to around a 35% CpS increase.
This pales in comparison to other CpS boosts,
like Frenzy,
Click Frenzy,
Sugar Frenzy,
Building Specials,
Golden Switch,
Loans,
and so on.

Cheapcaps also provide the same discount for upgrades,
and there are other discounts as well.
They are also very weak,
but here we can easily fix that by making these boost stronger.
Nothing would _really_ break if cheapcaps gave -2% upgrade cost, rather than just -0.2%,
because _upgrades cannot be sold_.

**This is what I call "the cheapcap curse"**:
because `dynamicPurchaseFactor` must always remain strictly greater than `refundFactor`,
dynamic building discounts are fated to forever be pitiful,
compared to other boosts in the game.

The cheapcap exploit got named after cheapcaps
because it was easy to just freeze the garden to increase `dynamicPurchaseFactor`.
Strictly speaking,
the nerf to cheapcaps
(making them have a 15% of chance of dying when the garden is frozen)
does _not_ patch the exploit,
it just make it take longer.
Players could still replace the "freeze/unfreeze" cycle with a "harvest/replant" cycle,
which would take longer but the net effect on the bank is the same.
What really patched the cheapcap exploit was the nerf to Earth Shatterer.

In fact,
because the old Earth Shatterer made the `refundFactor` be `0.9775`
(i.e. `0.85 * 1.15`, due to the bug about applying the refund factor to the _next_ building),
when Everything Must Go was introduced in Cookie Clicker 2.002 in 2016-06-28,
[players noticed](https://www.reddit.com/r/CookieClicker/comments/514pem/with_everything_must_go_is_profit_possible_by/)
that it was possible to sell buildings for a higher price than they were purchased for.
So cheapcap-like exploits already existed two years prior to the introduction of cheapcaps.
- Exploiting that exploit in 2016 would have been significantly slower than in 2018,
  because we'd need to wait for like half an hour to increase the bank by just `0.9775/0.95 = 1.0289`,
  instead of simply freezing/unfreezing the garden to increase the bank by 11%.
  But if we were to mod the game to buff these pitiful effects,
  we could be waiting half an hour to increase the bank by 100%, or even more;
  then,
  even though each cycle of the exploit takes much longer,
  the gains on each cycle would be much bigger.

Historical Notes
----------------

Some older versions of Cookie Clicker are available,
but as far as I can tell,
by and large most of Cookie Clicker history is effectively lost media.

So, to write the introduction in the beginning of the article,
I based myself on other sources.
Dates and version numbers were taken from
[the version history on the Cookie Clicker wiki](https://cookieclicker.wiki.gg/wiki/Version_History).
I was able to tell from [these](https://www.reddit.com/r/CookieClicker/comments/93luxy/comment/e3ee2br/)
[two](https://www.reddit.com/r/CookieClicker/comments/9e3awg/comment/e5n1d96/)
Reddit comments that the price reduction from Cheapcaps is still the same today as it was back then,
and [this comment](https://www.reddit.com/r/CookieClicker/comments/93luxy/comment/e3g2zzd/)
confirms that the bug of selling based on the next building rather than the current building
already existed back then.

(Specifically this bug can also be seen in Cookie Clicker 1.0466,
from 2014,
[playable here](https://orteil.dashnet.org/cookieclicker/v10466/);
but that post witnesses the fact that the bug was not, say,
patched on Cookie Clicker 2.0,
and then reintroduced later.)

