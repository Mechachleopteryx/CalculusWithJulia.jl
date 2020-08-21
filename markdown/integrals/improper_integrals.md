# Improper Integrals






A function $f(x)$ is Riemann integrable over an interval $[a,b]$ if
some limit involving Riemann sums exists. This limit will fail to exist if
$f(x) = \infty$ in $[a,b]$. As well, the Riemann sum idea is undefined if either $a$
or $b$ (or both) are infinite, so the limit won't exist in this case.

To define integrals  with either functions having singularities or infinite domains, the idea of an improper integral is
introduced with definitions to handle the two cases above.

````
CalculusWithJulia.WeaveSupport.ImageFile("/var/folders/k0/94d1r7xd2xlcw_jkg
qq4h57w0000gr/T/jl_D3vRVN.gif", L"
Area under $1/\sqrt{x}$ over $[a,b]$ increases as $a$ gets closer to $0$. W
ill it grow unbounded or have a limit?

")
````





## Infinite domains

Let $f(x)$ be a reasonable function, so reasonable that for any $a <
b$ the function is Riemann integrable, meaning $\int_a^b f(x)dx$
exists.

What needs to be the case so that we can discuss the limit over the entire real number line?

Clearly something. The function $f(x) = 1$ is reasonable by the idea
above. Clearly the integral over and $[a,b]$ is just $b-a$, but the
limit over an unbounded domain would be $\infty$. Even though limits
of infinity can be of interest in some cases, not so here. What will
ensure that the area is finite over an infinite region?


Or is that even the right question. Now consider $f(x) = \sin(\pi
x)$. Over every interval of the type $[-2n, 2n]$ the area is $0$, and
over any interval, $[a,b]$ the area never gets bigger than $2$. But
still this function does not have a well defined area on an infinite
domain.

The right question involves a limit. Fix a finite $a$. We define the definite integral over $[a,\infty)$ to be

$$~
\int_a^\infty f(x) dx = \lim_{M \rightarrow \infty} \int_a^M f(x) dx,
~$$

when the limit exists. Similarly, we define the definite integral over $(-\infty, a]$ through

$$~
\int_{-\infty}^a f(x) dx  = \lim_{M \rightarrow -\infty} \int_M^a f(x) dx.
~$$

For the interval $(-\infty, \infty)$ we have need *both* these limits to exist, and then:


$$~
\int_{-\infty}^\infty f(x) dx  = \lim_{M \rightarrow -\infty} \int_M^a f(x) dx + \lim_{M \rightarrow \infty} \int_a^M f(x) dx.
~$$

````
CalculusWithJulia.WeaveSupport.Alert("When the integral exists, it is said 
to *converge*. If it doesn't exist, it is said to *diverge*.\n", Dict{Any,A
ny}(:class => "info"))
````





##### Examples


* The function $f(x) = 1/x^2$ is integrable over $[1, \infty)$, as this limit exists:

$$~
\lim_{M \rightarrow \infty} \int_1^M \frac{1}{x^2}dx = \lim_{M \rightarrow \infty} -\frac{1}{x}\big|_1^M
= \lim_{M \rightarrow \infty} 1 - \frac{1}{M} = 1.
~$$

* The function $f(x) = 1/x^{1/2}$ is not integrable over $[1, \infty)$, as this limit fails to exist:

$$~
\lim_{M \rightarrow \infty} \int_1^M \frac{1}{x^{1/2}}dx = \lim_{M \rightarrow \infty} \frac{x^{1/2}}{1/2}\big|_1^M
= \lim_{M \rightarrow \infty} 2\sqrt{M} - 2 = \infty.
~$$

The limit is infinite, so does not exist except in an extended sense.

* The function $x^n e^{-x}$ for $n = 1, 2, \dots$ is integrable over $[0,\infty)$.

Before showing this, we recall the fundamental theorem of calculus. The limit existing is the same as saying the limit of $F(M) - F(a)$ exists for an antiderivative of $f(x)$.

For this particular problem, it can be shown by integration by parts that for positive, integer values of $n$ that an antiderivative exists of the form $F(x) = p(x)e^{-x}$, where $p(x)$ is a polynomial of degree $n$. But we've seen that for any $n>0$, $\lim_{x \rightarrow \infty} x^n e^{-x} = 0$, so the same is true for any polynomial. So, $\lim_{M \rightarrow \infty} F(M) - F(1) = -F(1)$.


* The function $e^x$ is integrable over $(-\infty, a]$ but not
$[a, \infty)$ for any finite $a$. This is because, $F(M) = e^x$ and this has a limit as $x$ goes to $-\infty$, but not $\infty$.


* Let $f(x) = x e^{-x^2}$. This function has an integral over $[0, \infty)$ and more generally $(-\infty, \infty)$. To see, we note that as it is an odd function, the area from $0$ to $M$ is the opposite sign of that from $-M$ to $0$. So $\lim_{M \rightarrow \infty} (F(M) - F(0)) = \lim_{M \rightarrow -\infty} (F(0) - (-F(\lvert M\lvert)))$. We only then need to investigate the one limit. But we can see by substitution with $u=x^2$, that an antiderivative is $F(x) = (-1/2) \cdot e^{-x^2}$. Clearly, $\lim_{M \rightarrow \infty}F(M) = 0$, so the answer is well defined, and the area from $0$ to $\infty$ is just $e/2$. From $-\infty$ to $0$ it is $-e/2$ and the total area is $0$, as the two sides "cancel" out.

* Let $f(x) = \sin(x)$. Even though $\lim_{M \rightarrow \infty} (F(M) - F(-M) ) = 0$, this function is not integrable. The fact is we need *both* the limit $F(M)$ and $F(-M)$ to exist as $M$ goes to $\infty$. In this case, even though the area cancels if $\infty$ is approached at the same rate, this isn't sufficient to guarantee the two limits exists independently.



* Will the function $f(x) = 1/(x\cdot(\log(x))^2)$ have an integral over $[e, \infty)$?

We first find an antiderivative using the $u$-substitution $u(x) = \log(x)$:

$$~
\int_e^M \frac{e}{x \log(x)^{2}} dx
= \int_{\log(e)}^{\log(M)} \frac{1}{u^{2}} du
= \frac{-1}{u} \big|_{1}^{\log(M)}
= \frac{-1}{\log(M)} - \frac{-1}{1}
= 1 - \frac{1}{M}.
~$$

As $M$ goes to $\infty$, this will converge to $1$.



* The sinc function $f(x) = \sin(\pi x)/(\pi x)$ does not have a nice antiderivative.  Seeing if the limit exists is a bit of a problem. However, this function is important enough that there is a built-in function, `Si`, that computes $\int_0^x \sin(u)/u\cdot du$. This function can be used through `sympy.Si(...)`:

````julia

using CalculusWithJulia    # loads `SymPy`, `QuadGK`
using Plots
@vars M
limit(sympy.Si(M), M => oo)
````


````
π
─
2
````





### Numeric integrals

The `quadgk` function (available through `QuadGK` which is loaded with `CalculusWithJulia`) is able to accept `Inf` and `-Inf` as endpoints of the interval. For example, this will integrate $e^{-x^2/2}$ over the real line:

````julia

f(x) = exp(-x^2/2)
quadgk(f, -Inf, Inf)
````


````
(2.506628274639168, 3.6084380708526996e-8)
````





(If may not be obvious, but this is $\sqrt{2\pi}$.)

## Singularities

Suppose $\lim_{x \rightarrow c}f(x) = \infty$ or $-\infty$. Then a Riemann sum that contains an interval including $c$ will not be finite if the point chosen in the interval is $c$. Though we could choose another point, this is not enough as the definition must hold for any choice of the $c_i$.

However, if $c$ is isolated, we can get close to $c$ and see how the area changes.

Suppose $a < c$, we define $\int_a^c f(x) dx = \lim_{M \rightarrow c-} \int_a^c f(x) dx$. If this limit exists, the definite integral with $c$ is well defined. Similarly, the integral from $c$ to $b$, where $b > c$, can be defined by a right limit going to $c$. The integral from $a$ to $b$ will exist if both the limits are finite.

##### Examples

* Consider the example of the initial illustration, $f(x) = 1/\sqrt{x}$ at $0$. Here $f(0)= \infty$, so the usual notion of a limit won't apply to $\int_0^1 f(x) dx$. However,

$$~
\lim_{M \rightarrow 0+} \int_M^1 \frac{1}{\sqrt{x}} dx
= \lim_{M \rightarrow 0+} \frac{\sqrt{x}}{1/2} \big|_M^1
= \lim_{M \rightarrow 0+} 2(1) - 2\sqrt{M} = 2.
~$$

````
CalculusWithJulia.WeaveSupport.Alert(L"
The cases $f(x) = x^{-n}$ for $n > 0$ are tricky. For $n > 1$, the function
s can be integrated over $[1,\infty)$, but not $(0,1]$. For $0 < n < 1$, th
e functions can be integrated over $(0,1]$ but not $[1, \infty)$.

", Dict{Any,Any}(:class => "info"))
````





* Now consider $f(x) = 1/x$. Is this integral  $\int_0^1 1/x \cdot dx$ defined? It will be *if* this limit exists:

$$~
\lim_{M \rightarrow 0+} \int_M^1 \frac{1}{x} dx
= \lim_{M \rightarrow 0+} \log(x) \big|_M^1
= \lim_{M \rightarrow 0+} \log(1) - \log(M) = \infty.
~$$

As the limit does not exist, the function is not integrable around $0$.

* `SymPy` may give answers which do not coincide with our definitions, as it uses complex numbers as a default assumption. In this case it returns `NaN` for an integral with no answer:

````julia

@vars x
integrate(1/x, (x, -1, 1))
````


````
nan
````





* Suppose you know $\int_1^\infty x^2 f(x) dx$ exists. Does this imply $\int_0^1 f(1/x) dx$ exists?

We need to consider the limit of $\int_M^1 f(1/x) dx$. We try the $u$-substitution $u(x) = 1/x$. This gives $du = -(1/x^2)dx = -u^2 dx$. So, the substitution becomes:

$$~
\int_M^1 f(1/x) dx = \int_{1/M}^{1/1} f(u) (-u^2) du = \int_1^{1/M} u^2 f(u) du.
~$$

But the limit as $M \rightarrow 0$ of $1/M$ is the same going to $\infty$, so the right side will converge by the assumption. Thus we get $f(1/x)$ is integrable over $(0,1]$.

### Numeric integration

So far our use of the `quadgk` function specified the region to
integrate via `a`, `b`, as in `quadgk(f, a, b)`. In fact, it can
specify values in between for which the function should not be
sampled. For example, were we to integrate $1/\sqrt{\lvert x\rvert}$
over $[-1,1]$, we would want to avoid $0$ as a point to sample. Here
is how:

````julia

f(x) = 1 / sqrt(abs(x))
quadgk(f, -1, 0, 1)
````


````
(3.999999962817228, 5.736423067171012e-8)
````





Just trying `quadgk(f, -1, 1)` leads to a `DomainError`, as `0` will
be one of the points sampled. The general call is like `quadgk(f, a, b, c, d,...)`
which integrates over $(a,b)$ and $(b,c)$ and $(c,d)$,
$\dots$. The algorithm is not supposed to evaluate the function at the
endpoints of the intervals.

## Probability applications

A probability density is a function $f(x) \geq 0$ which is integrable
on $(-\infty, \infty)$ and for which $\int_{-\infty}^\infty f(x) dx =1$. The cumulative distribution function is defined by $F(x)=\int_{-\infty}^x f(u) du$.

Probability densities are good example of using improper integrals.

* Show that $f(x) = (1/\pi) 1/(1 + x^2)$ is a probability density function.

We need to show that the integral exists and is $1$. For this, we use the fact that $(1/\pi) \cdot \tan^{-1}(x)$ is an antiderivative. Then we have:

$\lim_{M \rightarrow \infty} F(M) = (1/\pi) \cdot \pi/2$ and as
$\tan^{-1}(x)$ is odd, we must have $F(-\infty) = \lim_{M \rightarrow
-\infty} f(M) = -(1/\pi) \cdot \pi/2$. All told, $F(\infty) -
F(-\infty) = 1/2 - (-1/2) = 1$.

* Show that $f(x) = 1/(b-a)$ for $a \leq x \leq b$ and $0$ otherwise is a probability density.

The integral for $-\infty$ to $a$ of $f(x)$ is just an integral of the constant $0$, so will be $0$. (This is the only constant with finite area over an infinite domain.) Similarly, the integral from $b$ to $\infty$ will be $0$. This means:

$$~
\int_{-\infty}^\infty f(x) dx = \int_a^b \frac{1}{b-a} dx = 1.
~$$

* Show that if $f(x)$ is a probability density then so is $f(x-c)$ for any $c$.

We have by the $u$-substitution

$$~
\int_{-\infty}^\infty f(x-c)dx = \int_{u(-\infty)}^{u(\infty)} f(u) du = \int_{-\infty}^\infty f(u) du = 1.
~$$

The key is that we can use the regular $u$-substitution formula
provided $\lim_{M \rightarrow \infty} u(M) = u(\infty)$ is
defined. (The *informal* notation $u(\infty)$ is defined by that
limit.)

* If $f(x)$ is a probability density, then so is $(1/h) f((x-c)/h)$ for any $c, h > 0$.

Again, by a $u$ substitution with, now, $u(x) = (x-c)/h$, we have $du = (1/h) \cdot dx$ and the result follows just as before:


$$~
\int_{-\infty}^\infty \frac{1}{h}f(\frac{x-c}{h})dx = \int_{u(-\infty)}^{u(\infty)} f(u) du = \int_{-\infty}^\infty f(u) du = 1.
~$$


* If $F(x) = 1 - e^{-x}$, for $x \geq 0$, and $0$ otherwise,  find $f(x)$.

We want to just say $F'(x)= e^{-x}$ so $f(x) = e^{-x}$. But some care
is needed. First, that isn't right. The derivative for $x<0$ of $F(x)$
is $0$, so $f(x) = 0$ if $x < 0$. What about for $x>0$? The derivative
is $e^{-x}$, but is that the right answer? $F(x) = \int_{-\infty}^x
f(u) du$, so we have to at least discuss if the $-\infty$ affects
things. In this case, and in general the answer is *no*. For any $x$
we can find $M < x$ so that we have $F(x) = \int_{-\infty}^M f(u) du + \int_M^x f(u) du$. The first part
is a constant, so will have derivative $0$, the second will have
derivative $f(x)$, if the derivative exists (and it will exist at $x$
if the derivative is continuous in a neighborhood of $x$).

Finally, at $x=0$ we have an issue, as $F'(0)$ does not exist. The
left limit of the secant line approximation is $0$, the right limit of
the secant line approximation is $1$. So, we can take $f(x) = e^{-x}$
for $x > 0$ and $0$ otherwise, noting that redefining $f(x)$ at a
point will not effect the integral as long as the point is finite.

## Questions


###### Question

Is $f(x) = 1/x^{100}$ integrable around $0$?

````
CalculusWithJulia.WeaveSupport.Radioq(["Yes", "No"], 2, "", nothing, [1, 2]
, ["Yes", "No"], "", false)
````





###### Question

Is $f(x) = 1/x^{1/3}$ integrable around $0$?

````
CalculusWithJulia.WeaveSupport.Radioq(["Yes", "No"], 1, "", nothing, [1, 2]
, ["Yes", "No"], "", false)
````





###### Question

Is $f(x) = x\cdot\log(x)$ integrable on $[1,\infty)$?

````
CalculusWithJulia.WeaveSupport.Radioq(["Yes", "No"], 2, "", nothing, [1, 2]
, ["Yes", "No"], "", false)
````





###### Question

Is $f(x) = \log(x)/ x$ integrable on $[1,\infty)$?

````
CalculusWithJulia.WeaveSupport.Radioq(["Yes", "No"], 2, "", nothing, [1, 2]
, ["Yes", "No"], "", false)
````





###### Question

Is $f(x) = \log(x)$ integrable on $[1,\infty)$?

````
CalculusWithJulia.WeaveSupport.Radioq(["Yes", "No"], 2, "", nothing, [1, 2]
, ["Yes", "No"], "", false)
````





###### Question

Compute the integral $\int_0^\infty 1/(1+x^2) dx$.

````
CalculusWithJulia.WeaveSupport.Numericq(1.5707963267948966, 0.001, "", "[1.
5698, 1.5718]", 1.5697963267948967, 1.5717963267948964, "", "")
````





###### Question

Compute the the integral $\int_1^\infty \log(x)/x^2 dx$.

````
CalculusWithJulia.WeaveSupport.Numericq(0.999999998385741, 0.001, "", "[0.9
99, 1.001]", 0.998999998385741, 1.000999998385741, "", "")
````





###### Question

Compute the integral $\int_0^2 (x-1)^{2/3} dx$.

````
CalculusWithJulia.WeaveSupport.Numericq(1.2000000004723115, 0.001, "", "[1.
199, 1.201]", 1.1990000004723116, 1.2010000004723114, "", "")
````





###### Question

From the relationship that if $0 \leq f(x) \leq g(x)$ then $\int_a^b f(x) dx \leq \int_a^b g(x) dx$ it can be deduced that

* if $\int_a^\infty f(x) dx$ diverges, then so does $\int_a^\infty g(x) dx$.
* if $\int_a^\infty g(x) dx$ converges, then so does $\int_a^\infty f(x) dx$.

Let $f(x) = \lvert \sin(x)/x^2 \rvert$.

What can you say about $\int_1^\infty f(x) dx$, as $f(x) \leq 1/x^2$ on $[1, \infty)$?



````
CalculusWithJulia.WeaveSupport.Radioq(["It is convergent", "It is divergent
", "Can't say"], 1, "", nothing, [1, 2, 3], ["It is convergent", "It is div
ergent", "Can't say"], "", false)
````






----

Let $f(x) = \lvert \sin(x) \rvert / x$.

What can you say about $\int_1^\infty f(x) dx$, as $f(x) \leq 1/x$ on $[1, \infty)$?

````
CalculusWithJulia.WeaveSupport.Radioq(["It is convergent", "It is divergent
", "Can't say"], 3, "", nothing, [1, 2, 3], ["It is convergent", "It is div
ergent", "Can't say"], "", false)
````





----

Let $f(x) = 1/\sqrt{x^2 - 1}$.
What can you say about $\int_1^\infty f(x) dx$, as $f(x) \geq 1/x$ on $[1, \infty)$?




````
CalculusWithJulia.WeaveSupport.Radioq(["It is convergent", "It is divergent
", "Can't say"], 2, "", nothing, [1, 2, 3], ["It is convergent", "It is div
ergent", "Can't say"], "", false)
````





----


Let $f(x) = 1 + 4x^2$.
What can you say about $\int_1^\infty f(x) dx$, as $f(x) \leq 1/x^2$ on $[1, \infty)$?

````
CalculusWithJulia.WeaveSupport.Radioq(["It is convergent", "It is divergent
", "Can't say"], 2, "", nothing, [1, 2, 3], ["It is convergent", "It is div
ergent", "Can't say"], "", false)
````





----


Let $f(x) = \lvert \sin(x)^{10}\rvert/e^x$.
What can you say about $\int_1^\infty f(x) dx$, as $f(x) \leq e^{-x}$ on $[1, \infty)$?

````
CalculusWithJulia.WeaveSupport.Radioq(["It is convergent", "It is divergent
", "Can't say"], 1, "", nothing, [1, 2, 3], ["It is convergent", "It is div
ergent", "Can't say"], "", false)
````





###### Question

The difference between "blowing up" at $0$ versus being integrable at
$\infty$ can be seen to be related through the $u$-substitution
$u=1/x$. With this $u$-substitution, what becomes of $\int_0^1 x^{-2/3} dx$?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$\int_0^1 
u^{2/3} \cdot du$", L"$\int_1^\infty u^{2/3}/u^2 \cdot du$", L"$\int_0^\inf
ty 1/u \cdot du$"], 2, "", nothing, [1, 2, 3], LaTeXStrings.LaTeXString[L"$
\int_0^1 u^{2/3} \cdot du$", L"$\int_1^\infty u^{2/3}/u^2 \cdot du$", L"$\i
nt_0^\infty 1/u \cdot du$"], "", false)
````





###### Question

The antiderivative of $f(x) = 1/\pi \cdot 1/\sqrt{x(1-x)}$ is $F(x)=(2/\pi)\cdot \sin^{-1}(\sqrt{x})$.

Find $\int_0^1 f(x) dx$.

````
CalculusWithJulia.WeaveSupport.Numericq(0.9999999921866226, 0.001, "", "[0.
999, 1.001]", 0.9989999921866226, 1.0009999921866226, "", "")
````

