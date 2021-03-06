# Exponential and logarithmic functions

```julia; echo=false; results="hidden"
using CalculusWithJulia
using CalculusWithJulia.WeaveSupport
using Plots
nothing
```

The family of exponential functions is used to model growth and
decay. The family of logarithmic functions is defined here as the
inverse of the exponential functions, but have reach far outside of that.


## Exponential functions

The family of exponential functions is defined by
$f(x) = a^x, -\infty< x < \infty$ and $a > 0$.
For $0 < a < 1$ these functions decay or decrease, for $a > 1$ the
functions grow or increase, and if $a=1$ the function is constantly $1$.

For a given $a$, defining $a^n$ for positive integers is
straightforward, as it means multiplying $n$ copies of $a$. From this,
the key properties of exponents: $a^x \cdot a^y = a^{x+y}$, and
$(a^x)^y = a^{x \cdot y}$ are immediate consequences. For $a \neq 0$,
$a^0$ is defined to be $1$. For positive, integer values of $n$, we
have $a^{-n} = 1/a^n$. For $n$ a positive integer, we can
define $a^{1/n}$ to be the unique positive solution to $x^n=a$. And
using the key properties of exponents extend this to a definition of
$a^x$ for any rational $x$.

Defining $a^x$ for any real number requires some more sophisticated
mathematics. One method is to use a
[theorem](http://tinyurl.com/zk86c8r) that says a *bounded*
monotonically increasing sequence will converge. Then for $a > 1$ we
have if $q_n$ is a sequence of rational numbers increasing to $x$,
then $a^{q_n}$ will be a bounded sequence of increasing numbers, so
will converge to a number defined to be $a^x$. Something similar is
possible for the $0 < a < 1$ case.

This definition can be done to ensure the rules of exponents hold for
$a > 0$:

$$~
a^{x + y} = a^x \cdot a^y, \quad (a^x)^y = a^{x \cdot y}.
~$$


In `Julia` these functions are implemented using `^` or for a base of
$e$ through `exp(x)`. Here are some representative graphs:

```julia;
using CalculusWithJulia
using Plots
f1(x) = (1/2)^x
f2(x) = 1^x
f3(x) = 2^x
f4(x) = exp(x)
a,b = -2, 2
p = plot(f1, a, b, legend=false)
plot!(f2, a, b); plot!(f3, a, b); plot!(f4, a, b)
```

We see examples of some general properties:

* The domain is all real $x$ and the range is all *positive* $y$
  (provided $a \neq 1$).
* For $0 < a < 1$ the functions are monotonically decreasing.
* For $a > 1$ the functions are monotonically increasing.
* If $1 < a < b$ the and $x > 0$ we have $a^x < b^x$.


##### Example

[Continuously](http://tinyurl.com/gsy939y) compounded interest allows
an initial amount $P_0$ to grow over time according to
$P(t)=P_0e^{rt}$. Investigate the difference between investing $1,000$
dollars in an account which earns $2$% as opposed to an account which
earns $8$% over $20$ years.

The  $r$ in the formula is the interest rate, so $r=0.02$ or
$r=0.08$. To compare the differences we have:

```julia;
r2, r8 = 0.02, 0.08
P0 = 1000
t = 20
P0 * exp(r2*t), P0 * exp(r8*t)
```

As can be seen, there is quite a bit of difference.

In 1494, [Pacioli](http://tinyurl.com/gsy939y) gave the "Rule of 72",
stating that to find the number of years it takes an investment to
double when continuously compounded one should divide the interest rate
into $72$.

This formula is not quite precise, but a rule of thumb, the number is
closer to $69$, but $72$ has many divisors which makes this an easy to
compute approximation. Let's see how accurate it is:

```julia;
t2, t8 = 72/2, 72/8
exp(r2*t2), exp(r8*t8)
```
So fairly close - after $72/r$ years the amount is $2.05...$ times more
than the initial amount.

##### Example

[Bacterial growth](https://en.wikipedia.org/wiki/Bacterial_growth) (to
Wikipedia) is the asexual reproduction, or cell division, of a
bacterium into two daughter cells, in a process called binary
fission. During the log phase "the number of new bacteria appearing per
unit time is proportional to the present population." The article
states that "Under controlled conditions, *cyanobacteria* can double
their population four times a day..."

Suppose an initial population of $P_0$ bacteria, a formula for the
number after $n$ *hours* is $P(n) = P_0 2^{n/6}$ where $6 = 24/4$.

After two days what multiple of the initial amount is present if
conditions are appropriate?

```julia;
n = 2 * 24
2^(n/6)
```

That would be an enormous growth.  Don't worry: "Exponential growth
cannot continue indefinitely, however, because the medium is soon
depleted of nutrients and enriched with wastes."

##### Example

The famous [Fibonacci](https://en.wikipedia.org/wiki/Fibonacci_number)
numbers are $1,1,2,3,5,8,13,\dots$, where $F_{n+1}=F_n+F_{n-1}$. These numbers increase. To see how fast, if we *guess* that
the growth is evenually exponential and assume $F_n \approx c \cdot a^n$, then
our equation is approximately $a^{n+1} = a^n + a^{n-1}$ which has solutions $a$
satisfying $a^2 - a -1 = 0$. The positve solution is
$(1 + \sqrt{5})/2\approx 1.618$

That is evidence that the $F_n \approx 1.618^n$. (See
[Relation to golden ratio](https://en.wikipedia.org/wiki/Fibonacci_number#Relation_to_the_golden_ratio)
for a related, but more explicit exact formula.

##### Example

In the previous example, the exponential family of functions is used
to describe growth. Polynomial functions also increase. Could these be
used instead? If so that would be great, as they are easier to reason
about.

The key fact is that exponential growth is much greater than
polynomial growth. That is for large enough $x$ and for any fixed
$a>1$ and positive integer $n$ it is true that $a^x \gg x^n$.

Later we will see an easy way to certify this statement.



## Logarithmic functions: the inverse of exponential functions

As the exponential functions are strictly *decreasing* when $0 < a <
1$ and strictly *increasing* when $a>1$, in both cases an inverse
function will exist. (When $a=1$ the function is a constant and is not
one-to-one.) The domain of an exponential function is all real $x$ and
the range is all *positive* $x$, so these are switched around for the
inverse function.

The inverse function will solve for $x$ in the equation $a^x = y$. The
answer, formally, is the logarithm base $a$, written $\log_a(x)$.

That is $a^{\log_a(x)} = x$ and $\log_a(a^x) = x$ when defined.


To see how a logarithm is mathematically defined will have to wait,
though the family of functions - one for each $a>0$ - are implemented in `Julia` through the function
`log(a,x)`. There are special cases requiring just one argument: `log(x)` will compute the natural
log, base $e$ - the inverse of $f(x) = e^x$; `log2(x)` will compute the
log base $2$ - the inverse of $f(x) = 2^x$; and `log10(x)` will compute
the log base $10$ - the inverse of $f(x)=10^x$.

To see this in an example, we plot for base $2$ the exponential
function $f(x)=2^x$, its inverse, and the logarithm function with base
$2$:

```julia;
f(x) = 2^x
xs = range(-2, stop=2, length=100)
ys = f.(xs)
plot(xs, ys,  color=:blue, legend=false)  # plot f
plot!(ys, xs, color=:red)                 # plot f^(-1)
xs = range(1/4, stop=4, length=100)
plot!(xs, log2.(xs), color=:green)        # plot log2
```

Though we made three graphs, only two are seen, as the graph of `log2`
matches that of the inverse function.

Note that we needed a bit of care to plot the inverse function
directly, as the domain of $f$ is *not* the domain of $f^{-1}$. Again, in
this case the domain of $f$ is all $x$, but the domain of $f^{-1}$ is only all *positive* $x$ values.

Knowing that `log2` implements an inverse function allows us to solve
many problems involving doubling.


##### Example

An [old](https://en.wikipedia.org/wiki/Wheat_and_chessboard_problem) story about doubling is couched in terms of doubling grains of wheat. To simplify the story, suppose each day an amount of grain is doubled. How many days of doubling will it take 1 grain to become 1 million grains?

The number of grains after one day is $2$, two days is $4$, three days
is $8$ and so after $n$ days the number of grains is $2^n$. To answer
the question, we need to solve $2^x = 1,000,000$. The logarithm
function yields 20 days (after rounding up):

```julia;
log2(1_000_000)
```

##### Example

The half-life of a radioactive material is the time it takes for half the material to decay. Different materials have quite different half lives with some quite long, and others quite short. See [half lives](https://en.wikipedia.org/wiki/List_of_radioactive_isotopes_by_half-life) for some details.

The carbon 14 isotope is a naturally occurring isotope on Earth,
appearing in trace amounts. Unlike Carbon 12 and 13 it decays, in this
case with a half life of 5730 years (plus or minus 40 years). In a
[technique](https://en.wikipedia.org/wiki/Radiocarbon_dating) due to
Libby, measuring the amount of Carbon 14 present in an organic item
can indicate the time since death. The amount of Carbon 14 at death is
essentially that of the atmosphere, and this amount decays over
time. If roughly half the carbon 14 remains, then the death occurred
about 5730 years ago.


A formula for the amount of carbon 14 remaining $t$ years after death would be $P(t) = P_0 \cdot 2^{-t/5730}$.

If $1/10$ of the original carbon 14 remains, how old is the item? This amounts to solving $2^{-t/5730} = 1/10$. We have: $-t/5730 = \log_2(1/10)$ or:

```julia;
-5730 * log2(1/10)
```

```julia; echo=false
note("""
(Historically) Libby and James Arnold proceeded to test the radiocarbon dating theory by analyzing samples with known ages. For example, two samples taken from the tombs of two Egyptian kings, Zoser and Sneferu, independently dated to 2625 BC plus or minus 75 years, were dated by radiocarbon measurement to an average of 2800 BC plus or minus 250 years. These results were published in Science in 1949. Within 11 years of their announcement, more than 20 radiocarbon dating laboratories had been set up worldwide. Source: [Wikipedia](http://tinyurl.com/p5msnh6).
""")
```

### Properties of logarithms


The basic graphs of logarithms ($a > 1$) are all similar, though as we see larger
bases lead to slower growing functions, though all satisfy $\log_a(1)
= 0$:

```julia;
plot(log2, 1/2, 10)           # base 2
plot!(log, 1/2, 10)           # base e
plot!(log10, 1/2, 10)         # base 10
```


Now, what do the properties of exponents imply about logarithms?

Consider the sum $\log_a(u) + \log_a(v)$. If we raise $a$ to this
power, we have using the powers of exponents and the inverse nature of
$a^x$ and $\log_a(x)$ that:

$$~
a^{\log_a(u) + \log_a(v)} = a^{\log_a(u)} \cdot a^{\log_a(v)} = u \cdot v.
~$$

Taking $\log_a$ of *both* sides yields $\log_a(u) + \log_a(v)=\log_a(u\cdot v)$. That is logarithms turn products into sums (of logs).

Similarly, the relation $(a^{x})^y =a^{x \cdot y}, a > 0$s can be used to see that
$\log_a(b^x) = x \cdot\log_a(b)$. This follows, as applying $a^x$ to each side yields the
same answer.

Due to inverse relationship between $a^x$ and $\log_a(x)$ we have:

$$~
a^{\log_a(b^x)} = b^x.
~$$

Due to the rules of exponents, we have:

$$~
a^{x \log_a(b)} = a^{\log_a(b) \cdot x} = (a^{\log_a(b)})^x = b^x.
~$$

Finally, since $a^x$ is one-to-one (when $a>0$ and $a \neq 1$), if  $a^{\log_a(b^x)}=a^{x \log_a(b)}$ it must be that $\log_a(b^x) = x \log_a(b)$. That is, logarithms turn powers into products.

Finally, we use the inverse property of logarithms and powers to show that logarithms can be defined for any base. Say $a, b > 0$. Then $\log_a(x) = \log_b(x)/\log_b(a)$. Again, to verify this we apply $a^x$ to both sides to see we get the same answer:

$$~
a^{\log_a(x)} = x,
~$$

this by the inverse property. Whereas, by expressing $a=b^{\log_b(a)}$ we have:

$$~
a^{(\log_b(x)/\log_b(b))} = (b^{\log_b(a)})^{(\log_b(x)/\log_b(a))} =
b^{\log_b(a) \cdot \log_b(x)/\log_b(a) } = b^{\log_b(x)} = x.
~$$

In short we have these three properties of logarithmic functions:

> if $a,b$ are positive bases; $u,v$ are positive numbers; and $x$ is any real number then: 1) $\log_a(uv) = \log_a(u) + \log_a(v)$, 2) $\log_a(u^x) = x \log_a(u)$, and  3) $\log_a(u) = \log_b(u)/\log_b(a)$.

##### Example

Before the ubiquity of electonic calculating devices, the need to
compute was still present. Ancient civilizations had abacuses to make
addition easier. For multiplication and powers a [slide
rule](https://en.wikipedia.org/wiki/Slide_rule) rule could be used.
It is easy to represent addition physically with two straight pieces
of wood - just represent a number with a distance and align the two
pieces so that the distances are sequentially arranged. To multiply
then was as easy: represent the logarithm of a number with a distance
then add the logarithms. The sum of the logarithms is the logarithm of
the *product* of the original two values. Converting back to a number
answers the question. The conversion back and forth is done by simply
labeling the wood using a logartithmic scale. The slide rule was
[invented](http://tinyurl.com/qytxo3e) soon after Napier's initial publication
on the logarithm in 1614.


##### Example

Returning to the Rule of 72, what should the exact number be?

The amount of time to double an investment that grows according to
$P_0 e^{rt}$ solves $P_0 e^{rt} = 2P_0$ or $rt = \log_2(2)$. So we get
$t=\log_2(2)/r$. As $\log_2(2)$ is

```julia;
log(2, 2)
```

We get the actual rule should be the "Rule of $69.314...$".

## Questions

###### Question

Suppose ever $4$ days, a population doubles. If the population starts
with $2$ individuals, what is its size after $4$ weeks?

```julia; echo=false
n = 4*7/4
val = 2 * 2^n
numericq(val)
```

###### Question

A bouncing ball rebounds to a height of $5/6$ of the previous peak
height. If the ball is droppet at a height of $3$ feet, how high will
it bounce after $5$ bounces?

```julia; echo=false
val = 3 * (5/6)^5
numericq(val)
```

###### Question

Which is bigger $e^2$ or $2^e$?

```julia; echo=false
choices = [L"e^2", L"2^e"]
ans = e^2 - 2^e > 0 ? 1 : 2
radioq(choices, ans)
```


###### Question

Which is bigger $\log_8(9)$ or $\log_9(10)$?

```julia; echo=false
choices = [L"\log_8(9)", L"\log_9(10)"]
ans = log(8,9) > log(9,10) ? 1 : 2
radioq(choices, ans)
```

###### Question

If $x$, $y$, and $z$ satisfy $2^x = 3^y$ and $4^y = 5^z$, what is the
ratio $x/z$?

```julia; echo=false
choices = [
L"\frac{\log(2)\log(3)}{\log(5)\log(4)}",
L"2/5",
L"\frac{\log(5)\log(4)}{\log(3)\log(2)}"
]
ans = 1
radioq(choices, ans)
```

###### Question

Does $12$ satisfy $\log_2(x) + \log_3(x) = \log_4(x)$?

```julia; echo=false
ans = log(2,12) + log(3,12) == log(4, 12)
yesnoq(ans)
```


###### Question

The [Richter](https://en.wikipedia.org/wiki/Richter_magnitude_scale)
magnitude is determined from the logarithm of the amplitude of waves
recorded by seismographs (Wikipedia). The formula is $M=\log(A) -
\log(A_0)$ where $A_0$ depends on the epicenter distance. Suppose an
event has $A=100$ and $A_0=1/100$. What is $M$?

```julia; echo=false
A, A0 = 100, 1/100
val = M = log(A) - log(A0)
numericq(val)
```

If the magnitude of one earthquake is $9$ and the magnitude of another
earthquake is $7$, how many times stronger is $A$ if $A_0$ is the same
for each?

```julia; echo=false
choices = ["1000 times", "100 times", "10 times", "the same"]
ans = 2
radioq(choices, ans, keep_order=true)
```

###### Question

The [Loudest band](https://en.wikipedia.org/wiki/Loudest_band) can
possibly be measured in [decibels](https://en.wikipedia.org/wiki/Decibel). In 1976 the Who recorded $126$
db and in 1986 Motorhead recorded $130$ db. Suppose both measurements
record power through the formula $db = 10 \log_10(P)$. What is the
ratio of the Motorhead $P$ to the $P$ for the Who?

```julia; echo=false
db_who, db_motorhead = 126, 130
db2P(db) = 10^(db/10)
P_who, P_motorhead = db2P.((db_who, db_motorhead))
val = P_motorhead / P_who
numericq(val)
```

###### Question

Based on this graph:

```julia;
plot(log, 1/4, 4)
f(x) = x - 1
plot!(f, 1/4, 4)
```

Which statement appears to be true?

```julia; echo=false
choices = [L"x \geq 1 + \log(x)", L"x \leq 1 + \log(x)"]
ans = 1
radioq(choices, ans)
```


###### Question

Consider this graph:

```julia;
f(x) = log(1-x)
g(x) = -x - x^2/2
plot(f, -3, 3/4)
plot!(g, -3, 3/4)
```

What statement appears to be true?

```julia; echo=false
choices = [
L"\log(1-x) \geq -x - x^2/2",
L"\log(1-x) \leq -x - x^2/2"
]
ans = 1
radioq(choices, ans)
```

###### Question

Suppose $a > 1$. If $\log_a(x) = y$ what is $\log_{1/a}(x)$? (The
reciprocal property of exponents, $a^{-x} = (1/a)^x$, is at play here.)

```julia; echo=false
choices = [L"-y", L"1/y", L"-1/y"]
ans = 1
radioq(choices, ans)
```

Based on this, the graph of $\log_{1/a}(x)$ is the graph of
$\log_a(x)$ under which transformation?

```julia; echo=false
choices = [
L"Flipped over the $x$ axis",
L"Flipped over the $y$ axis",
L"Flipped over the line $y=x$"
]
ans = 1
radioq(choices, ans)
```


###### Question

Suppose $x < y$. Then for $a > 0$, $a^y - a^x$ is equal to:

```julia; echo=false
choices = [L"a^x \cdot (a^{y-x} - 1)",
L"a^{y-x}",
L"a^{y-x} \cdot (a^x - 1)"]
ans = 1
radioq(choices, ans)
```

Using $a > 1$ we have:

```julia; echo=false
choices = [
L"as $a^{y-x} > 1$ and $y-x > 0$, $a^y > a^x$",
L"as $a^x  > 1$, $a^y > a^x$",
L"a^{y-x} > 0"
]
ans=1
radioq(choices, ans)
```

If $a < 1$ then:

```julia; echo=false
choices = [
L"as $a^{y-x} < 1$ as $y-x > 0$, $a^y < a^x$",
L"as $a^x   < 1$, $a^y < a^x$",
L"a^{y-x} < 0"
]
ans = 1
radioq(choices, ans)
```
