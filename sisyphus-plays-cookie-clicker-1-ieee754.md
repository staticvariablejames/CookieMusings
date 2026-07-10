Sisyphus Plays Cookie Clicker I: Face to Face with IEEE754
==========================================================

(This is the first of a [series of articles](./README.md#sisyphus-plays-cookie-clicker)
investigating the limits of Cookie Clicker.
This first article is mostly technical;
we need to discuss how IEEE754 works and how it limits many counters in the game.)

[Sisyphus](https://en.wikipedia.org/wiki/Sisyphus) is a character in Greek mythology
who attracted the wrath of the Greek gods by deceitfully outsmarting them
(including cheating death).
He was condemned to push an enormous boulder up a hill,
which would inevitably fall down whenever Sisyphus neared the top,
forcing him to start again,
for all eternity.

The Cookie Clicker God Orteil has struck a deal with the Olympus gods to change Sisyphus's punishment.
Instead of rolling a boulder,
Sisyphus now has to play Cookie Clicker for all eternity.
Once Sisyphus' bakery reaches Infinity cookies,
Sisyphus will be released from his punishment.

Sisyphus is serving his sentence in the Greek Underworld,
so this is where we install a gaming computer for him,
complete with a very good gaming chair.
We disabled the browser's console,
and Sisyphus cannot rename his bakery (otherwise it will cease to be "Sisyphus' bakery"),
so he cannot simply open Sesame and get infinity cookies.
But Sisyphus could still edit his save to say "Infinity cookies",
so we also prevent that by simply forbidding Sisyphus from importing a save file.

Feigning annoyance,
Sisyphus starts clicking.
Cookie Clicker will display "Infinity cookies" once we hit one centillion cookies (10^303 cookies).
"Clicking 10^303 times will take a while",
thinks Sisyphus,
"but compared to literal eternity, this will be quick!"

One cookie turns into a thousand and turns into a million.
After an eternity rolling up a boulder,
Sisyphus is content in just clicking the big cookie,
ignoring other elements of the game (like buildings and golden cookies).
The billionth click brings up the popup about sugar lumps,
which is also promptly dismissed by our hero.

The trillionth click brings the trillionth cookie and a tiny "+1" underneath the "legacy" button.
Clicking that button would allow Sisyphus to _ascend_.
But we are in the Greek Underworld,
and Sisyphus is still serving his sentence,
so he's not allowed ascensions of any kind.
Hence that "+1" is also ignored.

The quadrillionth click passes by without fanfare,
but then an oddity happens:
instead of marching forward towards the quintillionth cookie,
Sisyphus' bakery gets stuck at 9.007 quadrillion cookies.
It does not matter how many times Sisyphus clicks,
the number does not budge.
What is going on?

This is Sisyphus's first encounter with his biggest enemy in the journey towards infinity cookies:
the [IEEE Standard for Floating Point Arithmetic](https://en.wikipedia.org/wiki/IEEE_754),
also known as IEEE754.


How many sugar lumps can we get?
--------------------------------

Cookie Clicker is implemented in JavaScript.
This programming language is peculiar in that its numbers are all 64-bit floating-point numbers
following the IEEE754 specification.
(Except for [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
but Cookie Clicker does not use those.)
The definitive source for IEEE754 floating-point
[is the standard itself](https://ieeexplore.ieee.org/document/8766229),
though you can find more readable sources on the internet.
This section is a summary of the parts of this standard which are relevant for Cookie Clicker.
Our guiding question is "how many sugar lumps can we store?"

Since we know that JavaScript numbers are stored in 64 bits,
and there are `2^64` different 64-bit strings,
we know that there are at most `2^64` different numbers with a floating-point representation.
(In reality we only get `2^64-2^53` different numbers due to technicalities,
but this is close to the theoretical maximum of `2^64`.)
However the representable numbers are not evenly spaced,
so we will have to learn how a string of 64 bits gets interpreted as a number.

Floating-point numbers are essentially numbers represented in the scientific notation,
except that the number base is 2 instead of 10.
More explicitly,
a **64-bit floating-point number** is either the number 0, or a number of the form

    ± r * 2^e,

where `e`, the **exponent**, is an integer,
and `r`, the **significand** (also sometimes called the mantissa),
is a rational number between 1 and 2.
The bit string representation of these numbers looks like

    seeeeeeeeeeerrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrr.

- `s` is the sign bit (`0` for positive and `1` for negative).

- The next 11 bits encode the exponent.
  The bit strings `00000000000` and `11111111111` are reserved and have a special meaning;
  we ignore those and simply encode the numbers between -1022 and 1023 in order.
  **This means that the exponent is constrained to be an integer between -1022 and 1023**.
  - The bit string `00000000001` encodes the number -1022;
  - The bit string `00000000010` encodes the number -1021;
  - The bit string `00000000011` encodes the number -1020;
  - ...
  - The bit string `11111111110` encodes the number 1023.
  - In fancy terms,
    the exponent is encoded
    in an [offset binary](https://en.wikipedia.org/wiki/Offset_binary) representation.
    If you are a hardware designer,
    each bit in the representation has a specific meaning (as a specific power of two),
    but for our purposes it suffices to know that
    we represent all 2046 integers between -1022 and 1023.

- The last 52 bits encode the significand.
  Here, we represent numbers between 1 and 2 whose denominator is a power of 2.
  More specifically:
  - The bit string `000...0000` encodes the number `1 = 1 + 0 * 2^(-52)`.
  - The bit string `000...0001` encodes the number `1 + 1 * 2^(-52)`.
  - The bit string `000...0010` encodes the number `1 + 2 * 2^(-52)`.
  - ...
  - The bit string `111...1111` encodes the number `1 + (2^52-1) * 2^(-52) = 2 - 2^(-52)`.
  - In fancy terms,
    we encode the rational number `a/b`
    by first multiplying both `a` and `b` to ensure `b = 2^52`,
    and then encode `a` as a binary number.
    Because `1 <= a/b < 2`, the resulting `a` would need 53 bits to encode,
    but because the first bit is always `1`,
    we simply omit it from our representation.
    Alternatively,
    we can also write `a/b` in binary, like `1.d_1 d_2 d_3 ... d_52`,
    and store the 52 bits `d_1 d_2 ... d_52` in the significant's place.
    If you are a hardware designer,
    again each bit has a specific meaning (as a specific negative power of two),
    but the key takeaway for us is that
    **the significands are multiples of `2^(-52)` between 1 and 2**
    (including 1 but excluding 2).

This means that the collection of numbers that can be represented as 64-bit floating-point numbers
has a very well-behaved "shape".
We split the real line in half-open intervals of the form `[2^e, 2^(e+1))`
(i.e. all numbers between `2^e` and `2^(e+1)`, including `2^e` but excluding `2^(e+1)`).
There are 2046 such intervals,
one for each possible value of the exponent `e`.
In each of these intervals,
we represent `2^52` equally-spaced numbers.
Each of these numbers correspond to one value of the significand `s`.
- For example,
  In the half-open interval `[1, 2)`,
  the `2^52` numbers which are represented are spaced apart exactly by `2^(-52)`.
- In the half-open interval `[2, 4)`
  the `2^52` numbers which are represented are spaced apart exactly by `2^(-51)`.
- In the half-open interval `[4, 8)`
  the `2^52` numbers which are represented are spaced apart exactly by `2^(-50)`.
  and so on.
- The same thing happens for negative exponents.
  For example,
  in the half-open interval `[0.5, 1)` we also represent `2^52` numbers,
  and they are spaced apart exactly by `2^(-53)`.
  In the half-open interval `[0.25, 0.5)` we represent `2^52` numbers spaced apart by `2^(-54)`,
  and so on.

Since we are interested in the number of sugar lumps we can accumulate,
we are only interested in the integers which can be represented by floating-point numbers.
If the exponent `e` is at most 52,
then all the integers in the half-open interval `[2^e, 2^(e+1))` will be representable.
(In fact, for `e = 52`,
the representable numbers in the interval `[2^52, 2^53)` are exactly the integers.)
This means that all integers up to `2^53` can be represented,
and since the count of sugar lumps typically increase by 1 each time,
we are able to go through all lumps counts between 0 and `2^53`.

However,
once `e` gets to 53,
we face trouble.
- In the half-open interval `[2^53, 2^54)`,
  we represent `2^52` numbers, spacing them uniformly;
  this means that the numbers are spaced `2` apart,
  i.e. we can only represent the even numbers in this interval.
- In the half-open interval `[2^54, 2^55)`
  we again represent `2^52` numbers,
  so the representable numbers here are the multiples of 4.
- In the half-open interval `[2^55, 5^56)` we represent only the multiples of 8, and so on.

This explains why Sisyphus stopped at 9 quadrillion cookies.
Once he got the `2^53`th cookie,
the next cookie amount would be `2^53 + 1`,
which is not representable as a floating-point number.
Hence JavaScript _rounds that number down_ to `2^53`.
The next click also stays at `2^53` for the same reason.
And the next one and the next one.
Hence Sisyphus is stuck at `2^53` cookies,
i.e. 9007199254740992---about 9.007 quadrillion.

Therefore,
Sisyphus must actually pay attention to other parts of the game,
and the first thing that Sisyphus notices are sugar lumps.
They also go up in integer increments,
so Sisyphus expected its number to also get stuck at `2^53`,
but there are different lump types.
With a bit of luck,
a bifurcated sugar lump yields two lumps at once,
so the number of lumps becomes `2^53+2`,
which is representable as a IEEE754 64-bit floating-point number.
But then something weird happens:
the next lump Sisyphus harvests is a normal one,
but the lump counter goes to `2^53+4` lumps.
The next few lumps are all normal,
and the lump counter still does not move from `2^53+4`.
What is going on?

### Rounding in IEEE754

Whenever a mathematical operation is performed with IEEE754 floating-point numbers,
the standard mandates that the operation is to first be performed with infinite precision,
and then (once the mathematical result is established)
the result is rounded to the nearest representable number.

For example,
say we have `2^53` cookies and we get `1.5` more.
The mathematical result `2^53+1.5` is not representable;
the two representable numbers surrounding it are `2^53` and `2^53+2`,
and the result is closer to the latter,
so the resulting number of cookies is `2^53+2`.
(If you are reading this in a web browser,
you can check this yourself:
press F12 to open the console,
and type `2**53 + 1.5` and then `2**53 + 2` and see that they yield the same result.)

But what if the mathematical result sits exactly in the middle?
In this case,
we look at the significand of the two numbers surrounding the mathematical result.
For example,
when we are at `2^53` sugar lumps and we get another one,
the two candidates are `2^53` and `2^53+2`,
whose significands `1` and `1 + 2^(-52)`
are encoded by the bit strings `000...0000` and `000...0001`,
respectively.
Look at the rightmost bit of the significand:
it will always be the case that one of the candidates has a significand whose rightmost bit is `0`,
and the other candidate has a significant whose rightmost bit is `1`.
JavaScript specifies that we pick the one with bit `0` in case of ties,
so the tie is broken in favor of `2^53`.

If we had `2^53+2` lumps and we got another one,
then the significands of the two candidates `2^53+2` and `2^53+4`
are represented by `000...0001` and `000...0010`, respectively.
This this time we pick the second one,
and we end up with `2^53+4` lumps.

If we interpret the significand as a rational number of the form `a/2^52`,
then the rightmost bit is `0` precisely when `a` is an even number,
which is why this "rounding mode" is called `roundTiesToEven`
(even though, for exponents larger than 53,
the two candidates are both even numbers).

(The IEEE754 standard also specifies four other "rounding modes",
[but JavaScript always uses the `roundTiesToEven` mode](https://tc39.es/ecma262/multipage/ecmascript-data-types-and-values.html#sec-ecmascript-language-types-number-type).)

Bifurcated and caramelized lumps have a chance of giving us +2 and +3 lumps at once,
so we slowly increase our sugar lump count from `2^53` all the way up to `2^54`.
(Meaty sugar lumps also would work,
but Sisyphus hasn't started the grandmapocalypse yet,
so he can't get them.)
Between `2^54` and `2^55` lumps the representable integers are spaced 4 apart,
so getting a +2 has the same effect as a +1 had between `2^53` and `2^54`
(half of the times it increases the count by 4, and half of the times it does not increase at all).
But the +3 of caramelized lumps always puts us closer to the highest of the two numbers,
so we always increase it by 4.
Between `2^55` and `2^56`,
the representable numbers are spaced 8 apart,
so we need golden lumps.
The +5, +6, and +7 of getting a golden lump will always place us closer to the highest end,
in practice rounding it up to a +8,
and +4's work again half of the time.
So just harvesting coalescing sugar lumps,
we cap off at `2^56`.

But there is still one more way of getting multiple sugar lumps at once:
sacrificing the garden seed log grants us 10 sugar lumps in a single go.
So we can proceed through one last power of two.
Between `2^56` and `2^57`,
the `2^52` representable numbers are spaced 16 apart.
The first representable number after `2^56` is `2^56+16`,
hence when we get +10 lumps at once by sacrificing the garden
the closest candidate to `2^56+10` is `2^56+16`.
The next garden sacrifice gets us to `2^56+32` and so on,
all the way up to `2^57`.
Hence,
once Sisyphus starts paying attention to the rest of the game,
he can sacrifice the garden `2^52` times to go from `2^56` sugar lumps to `2^57`.

(There is yet one last way of getting multiple sugar lumps:
fully offline lumps
---that is, sugar lumps which started growing offline and are auto-harvested offline---
are awarded in a single go,
so if Sisyphus logs off for 20 days,
he will get 19 lumps in a single batch.
This would essentially make the number of sugar lumps unlimited.
but because Sisyphus is serving a sentence
we will not simply allow him to log off whenever he pleases.
His previous punishment was rolling a boulder for all eternity,
so we will force him to play Cookie Clicker for all eternity.
Hence for our purposes the cap on the number of sugar lumps is `2^57`.
We will talk more about this technicality in a later article.)

### Zeros, Subnormals, Infinities, NaNs

We finish this section by talking about the reserved values for the exponent.
Recall that the exponent is represented as an 11-bit string,
and that the strings `00000000000` and `11111111111` have a special meaning
rather than encoding an exponent.

When writing numbers in base-10 scientific notation,

    ± r * 10^e,

we _normalize_ the significand `r` by placing it in the half-open interval `[1, 10)`;
i.e. we "float" the decimal place to make sure that `r` is between 1 and 10
(including 1 and excluding 10).
Analogously,
in base-2 scientific notation

    ± r * 10^e,

the significand is normalized to be in the interval `[1, 2)`
(between 1 and 2, including 1 and excluding 2).
This normalization step makes it impossible to represent the number 0.
This is where the reserved exponents show up.

- If the exponent is the bit string `00000000000`
  and the bit string of the significand is also all zeros,
  this encodes the number zero.
  - Pleasantly,
    this means that the all-zeros bit string represents the number zero.
  - Unpleasantly,
    the sign bit can be negative.
    IEEE754 has the number `-0`, which is distinct from the number `+0` (i.e. `0`).
    There are operations with IEEE754 floating-point numbers
    that produce this "negative zero",
    but this will not happen in Cookie Clicker.

- If the exponent is the bit string `00000000000`
  and the bit string of the significand is not all zeros,
  we encode a _subnormal number_.
  Here the significand `r` is a number between 0 and 1 (instead of between 1 and 2),
  again a fraction whose denominator is a multiple of `2^(-52)`,
  and it encodes the number `r * 10^(-1022)`.
  - These numbers are called _subnormal_
    because the significand is smaller than it would be if it were to be normalized.
    These numbers extend a bit the range of numbers available
    and reduce rounding errors when calculating with very small numbers,
    but much like negative zero,
    they will also not show up in Cookie Clicker.

- If the exponent is the bit string `11111111111`
  and the bit string of the significand is all zeros,
  this number represents infinity.
  - Infinity shows up in computations when the result is larger than any representable number.
    For example,
    the largest representable number
    (with the highest possible exponent of 1023 and highest possible significand of `2-2^(-52)`)
    is `2^1204-2^971`,
    which is about `1.7977 * 10^308`.
    If we add `2^971` to this number,
    the result is too large to represent as a 64-bit floating point number,
    so we get the IEEE754 `+Infinity` instead.
  - Cookie Clicker will display "Infinity" slightly earlier than `10^308`.
    The largest named exponent is "novemnonagintillion",
    corresponding to `10^(3*99 + 3) = 10^300`.
    The next named exponent, 10^303, would be "centillion",
    but Cookie Clicker gives up and just say "infinity" for any values larger than 10^303.
    The range lost is just a few powers of ten,
    but this does mean that Cookie Clicker says "infinity"
    a few exponents before reaching the IEEE754 `+Infinity`.

- If the exponent is the bit string `11111111111`
  and the bit string of the significand is not all zeros,
  then this is a special value called "Not a Number" (`NaN`).
  - `NaN`s show up when we attempt to perform a "meaningless operation",
    like calculating `Infinity-Infinity` or `0/0`.
    Note that some mathematically meaningless operations
    do have meaningful IEEE754 results as infinities;
    for example,
    division by zero results in Infinity,
    unless the numerator itself is zero or `NaN`.
  - `NaN`s are destructive.
    Any operation involving a `NaN` returns another `NaN`,
    so we will have to avoid the rare cases where a calculation would yield `NaN`.
  - Unfortunately for us,
    in many cases `NaN`s are displayed as "Infinity" by Cookie Clicker,
    so we will have to be careful to know when an infinite value is indeed `+Infinity`
    or just an incorrectly-rendered `NaN`.

Other than zero,
we will rarely meet these numbers in Cookie Clicker.


Effect on Counters
------------------

We have seen how sugar lumps stop increasing normally when reaching `2^53` lumps
(i.e. 9.007 quadrillion lumps)
and only increase further if we can get several lumps in a single go.
Recall that JavaScript does not have alternative number formats,
so _all_ numbers in Cookie Clicker behave like this.
This is particularly important for counters,
which are only ever incremented in steps of 1:
as discussed above,
these counters will be capped off at `2^53`.

- Many statistics are simple counters;
  the number of cookie clicks,
  of golden cookies and reindeer clicked,
  ascensions,
  wrinklers popped,
  plant harvested,
  and even hidden statistics
  (the number of golden cookies missed and the number of garden sacrifices)
  are all capped at `2^53`.

- Building levels are counters:
  they increment by 1 whenever you level up the building,
  and can only gain one level at a time.
  Hence building levels are capped at `2^53`.

- The counter for the number of spells cast in the Grimoire also stops at `2^53`.
  At that point the outcomes of the Grimoire will be "stuck" too;
  we will discuss more about this in a later article.

Most of this article was spent understanding how IEEE754 works.
In the next article,
Sisyphus will purchase buildings and upgrades and perform combos!


The Companion GitHub Repository
===============================

If you'd like to see Sisyphus's progress yourself,
I have created a GitHub repository containing several save games from our hero's journey:
<https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/>.
You will need [Cookie Connoisseur](https://github.com/staticvariablejames/cookie-connoisseur)
to time-travel far enough into the future and witness the game as Sisyphus saw it,
but you should be able to get a glimpse of our hero's progress.

[The save file from this first article is available here.](https://github.com/staticvariablejames/SisyphusPlaysCookieClicker/blob/master/saves/sisyphus1.cki)
