The Cheapcap Curse
==================

This is a game design article,
talking about one specific problem regarding Cookie Clicker's ability to sell buildings.

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
   we now spend only `0.8793 * x` cookies instead.
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
5. So we spent about `0.8793 * x` cookies and got `0.9775 * x` cookies,
   a relative gain of 11.16%.
   If we spent all our bank on those initial `0.8793 * x` cookies,
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

