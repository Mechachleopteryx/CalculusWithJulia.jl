# Vector-valued functions, $f:R \rightarrow R^n$





We discuss functions of a single variable that return a vector in $R^n$. There are many parallels to univariate functions (when $n=1$) and differences.

Before beginning, we load our `CalculusWithJulia` package, which will provide a few functions used in the following.

````julia
using CalculusWithJulia
using Plots
````






## Definition

A function $\vec{f}: R \rightarrow R^n$, $n > 1$ is called a vector valued function. Some examples:

$$~
\vec{f}(t) = \langle \sin(t), 2\cos(t) \rangle, \quad
\vec{g}(t) = \langle \sin(t), \cos(t), t \rangle, \quad
\vec{h}(t) = \langle 2, 3 \rangle + t \cdot \langle 1, 2 \rangle.
~$$

The components themselves are also functions of $t$, in this case
univariate functions. Depending on the context, it can be useful to
view vector-valued functions as a function that returns a vector, or a
vector of the component functions.


The above example functions have $n$ equal $2$, $3$, and $2$ respectively. We
will see that many concepts of calculus for univariate functions
($n=1$) have direct counterparts.


## Representation in Julia

In `Julia`, the representation of a vector-valued function is straightforward: we define a function of a single variable that returns a vector. For example, the three functions above would be represented by:

````julia

f(t) = [sin(t), 2*cos(t)]
g(t) = [sin(t), cos(t), t]
h(t) = [2, 3] + t * [1, 2]
````


````
h (generic function with 1 method)
````





For a given `t`, these evaluate to a vector. For example:

````julia

h(2)
````


````
2-element Array{Int64,1}:
 4
 7
````





We can create a vector of functions, e.g., `F = [cos, sin, identify]`, but calling this object, as in `F(t)`, would require some work, such as `t = 1; [f(t) for f in F]`.




## Space curves

A vector-valued function is typically visualized as a
curve. That is, for some range, $a \leq t \leq b$ the set of points
$\{\vec{f}(t): a \leq t \leq b\}$ are plotted. If, say in $n=2$, we
have $x(t)$ and $y(t)$ as the component functions, then the graph
would also be the parametric plot of $x$ and $y$. The term *planar* curve is common for the $n=2$ case and *space* curve for the $n \geq 3$ case.


This plot represents the vectors with their tails at the origin.

There is a convention for plotting the component functions to yield a parametric plot within the `Plots` package (e.g., `plot(x, y, a, b)`). This can be used to make polar plots, where `x` is  `t -> r(t)*cos(t)` and `y` is `t -> r(t)*sin(t)`.

However,  we will use a different approach, as the component functions are not naturally produced from the vector-valued function.

In `Plots`, the command `plot(xs, ys)`, where, say, `xs=[x1, x2, ..., xn]` and `ys=[y1, y2, ..., yn]`, will make a connect-the-dot plot between corresponding pairs of points. This can be used as an alternative to plotting a function through `plot(f, a, b)`: first make a set of $x$ values, say `xs=range(a, b, length=100)`; then the corresponding $y$ values, say `ys = f.(xs)`; and then plotting through `plot(xs, ys)`. (If using `Julia 1.0` the second argument to `range` should be named, as in `stop=b`.)

Similarly, were a third vector, ` zs`, for $z$ components used, `plot(xs, ys, zs)` will make a 3-dimensional connect the dot plot

However, our representation of vector-valued functions naturally generates a vector of points: `[[x1,y1], [x2, y2], ..., [xn, yn]]`, as this comes from broadcasting `f` over some time values.
That is, for a collection of time values, `ts`, typically generated, as above, by `range`, the command `f.(ts)` will produce the vector of points.

To get the `xs` and `ys` from this, is conceptually easy: just iterate over all the points and extract the corresponding component. For example, to get `xs` we would have a command like `[p[1] for p in f.(ts)]`. Similarly, the `ys` would use `p[2]` in place of `p[1]`. The following  function, from the `CalculusWithJulia` package and employed  previously, does this for us, returning the vectors in a tuple:


````julia

unzip(vs) = Tuple(eltype(first(vs))[xyz[j] for xyz in vs] for j in eachindex(first(vs)))
````




The name comes from how the `zip` function in base `Julia` takes two vectors and returns a vector of the values paired off. This is the reverse.


It was noted in [vectors](./vectors.html) that the `unzip` function
turns a vector of points (or vectors) into a tuple of vectors
collecting the $x$ values, the $y$ values, and, if present, the $z$
values:

```
[[x1, y1, z1],         (⌈x1⌉,  ⌈y1⌉, ⌈z1⌉,
 [x2, y2, z2],          |x2|, |y2|, |z2|,
 [x3, y3, z3],   -->    |x3|, |y3|, |z3|,
     ⋮                         ⋮
 [xn, yn, zn]]          ⌊xn⌋,  ⌊yn⌋, ⌊zn⌋ )
```





To turn a tuple of vectors into separate arguments for a function, splatting (the `...`) is used.

----

Finally, with these definitions, we can visualize the three functions we have defined.

Here we show the plot of `f` over the values between $0$ and $2\pi$ and also add a vector anchored at the origin defined by `f(1)`.

````julia
ts = range(0, 2pi, length=200)
plot(unzip(f.(ts))...)
arrow!([0, 0], f(1))
````





The trace of the plot is an ellipse. If we describe the components as $\vec{f}(t) = \langle x(t), y(t) \rangle$, then we have $x(t)^2 + y(t)^2/4 = 1$. That is, for any value of $t$, the resulting point satisfies the equation $x^2 + y^2/4 =1$ for an ellipse.


The plot of $g$ needs 3-dimensions to render. For most plotting backends, the following should work with no differences, save the additional vector is anchored in 3 dimensions now:


````julia
ts = range(0, 6pi, length=200)
plot(unzip(g.(ts))...)
arrow!([0, 0, 0], g(2pi))
````






Here the graph is a helix; three turns are plotted. If we write $g(t) = \langle x(t), y(t), z(t) \rangle$, as the $x$ and $y$ values trace out a circle, the $z$ value increases. When the graph is viewed from above, as below, we see only $x$ and $y$ components, and the view is circular.

````julia

plot(unzip(g.(ts))..., camera=(0, 90))
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_11_1.png)




The graph of $h$ shows that this function parameterizes a line in space. The line segment for $-2 \leq t \leq 2$ is shown below:

````julia

ts = range(-2, 2, length=200)
plot(unzip(h.(ts))...)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_12_1.png)



### The `plot_parametric_curve` function

While the `unzip` function is easy to understand as a function that reshapes data from one format into one that `plot` can use, it's usage is a bit cumbersome.
The `CalculusWithJulia` package provides a function `plot_parametric_curve` which hides the use of `unzip` and the splatting within a function definition. It expects a vector-valued function and a range of $t$ values specified by two values. For example, the last plot can be produced with

````julia

plot_parametric_curve(h, -2, 2)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_13_1.png)

````
CalculusWithJulia.WeaveSupport.Alert("Defining plotting functions in `Julia
` for `Plots` is facilitated by the `RecipesBase` package. There are two co
mmon choices: creating a new function for plotting, as is done with `plot_p
arametric_curve` and `plot_polar`; or creating a new type so that `plot` ca
n dispatch to an appropriate plotting method. The latter would also be a re
asonable choice, but wasn't taken here.\n", Dict{Any,Any}(:class => "info")
)
````





##### Example

Familiarity with equations for lines, circles, and ellipses is
important, as these fundamental geometric shapes are often building
blocks in the description of other more complicated things.

The point-slope equation of a line, $y = y_0 + m \cdot (x - x_0)$
finds an analog. The slope, $m$, is replaced with a vector $\vec{v}$
and the point, $(x_0, y_0)$ is replaced with a vector $\vec{p}$
identified with a point in the plane. A parameterization would then be
$f(t) = \vec{p} + (t - t_0) \vec{v}$. From this, we have $f(t_0) =
\vec{p}$.




The unit circle is instrumental in introducing the trigonometric
functions though the identification of and angle $t$ with a point on
the unit circle $(x,y)$ through $y = \sin(t)$ and $x=\cos(t)$. With
this identification certain properties of the trigonometric functions
are immediately seen, such as the period of $\sin$ and $\cos$ being
$2\pi$, or the angles for which $\sin$ and $\cos$ are positive or even
increasing. Further, this gives a natural parameterization for a
vector-valued function whose plot yields the unit circle, namely
$\vec{f}(t) = \langle \cos(t), \sin(t) \rangle$. This
parameterization starts (at $t=0$) at the point $(1, 0)$. More
generally, we might have additional parameters $\vec{f}(t) = \vec{p} +
R \cdot \langle \cos(\omega(t-t_0)), \sin(\omega(t-t_0)) \rangle$ to
change the origin, $\vec{p}$; the radius, $R$; the starting angle,
$t_0$; and the rotational frequency, $\omega$.


An ellipse has a slightly more general equation than a circle and in
simplest forms may satisfy the equation $x^2/a^2 +  y^2/b^2 = 1$, where *when*
$a=b$ a circle is being described. A vector-valued function of the form
$\vec{f}(t) = \langle a\cdot\cos(t),  b\cdot\sin(t) \rangle$ will trace out an ellipse.


The above description of an ellipse is useful, but it can also be useful to re-express the ellipse so that one of the foci is at the origin. With this, the ellipse can be given in *polar* coordinates through a description of the radius:

$$~
r(\theta) = \frac{a (1 - e^2)}{1 + e \cos(\theta)}.
~$$

Here, $a$ is the semi-major axis ($a > b$); $e$ is the *eccentricity* given by $b = a \sqrt{1 - e^2}$; and $\theta$ a polar angle.

Using the conversion to Cartesian equations, we have
$\vec{x}(\theta) = \langle r(\theta) \cos(\theta), r(\theta) \sin(\theta)\rangle$.

For example:

````julia

a, ecc = 20, 3/4
f(t) = a*(1-ecc^2)/(1 + ecc*cos(t)) * [cos(t), sin(t)]
plot_parametric_curve(f, 0, 2pi, legend=false)
scatter!([0],[0], markersize=4)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_15_1.png)





##### Example

The [Spirograph](https://en.wikipedia.org/wiki/Spirograph) is "... a geometric drawing toy that produces mathematical roulette curves of the variety technically known as hypotrochoids and epitrochoids. It was developed by British engineer Denys Fisher and first sold in 1965." These can be used to make interesting geometrical curves.

Following Wikipedia: Consider a fixed outer circle $C_o$ of radius $R$ centered at the origin. A smaller inner circle  $C_i$ of radius $r < R$ rolling inside $C_o$ and is continuously tangent to it. $C_i$ will be assumed never to slip on $C_o$ (in a real Spirograph, teeth on both circles prevent such slippage). Now assume that a point $A$ lying somewhere inside $C_{i}$is located a distance $\rho < r$ from $C_i$'s center.

The center of the inner circle will move in a circular manner with radius $R-r$. The fixed point on the inner circle will rotate about this center. The accumulated angle may be described by the angle the point of contact of the inner circle with the outer circle. Call this angle $t$.

Suppose the outer circle is centered at the origin and the inner circle starts ($t=0$) with center $(R-r, 0)$ and rotates around counterclockwise. Then if the point of contact makes angle $t$, the arc length along the outer circle is $Rt$. The inner circle will have moved a distance $r t'$ in the opposite direction, so $Rt =-r t'$ and solving the angle will be $t' = -(R/r)t$.

If the initial position of the fixed point is at $(\rho, 0)$ relative to the origin, then the following function will describe the motion:

$$~
\vec{s}(t) = (R-r) \cdot \langle \cos(t), \sin(t) \rangle +
\rho \cdot \langle \cos(-\frac{R}{r}t), \sin(-\frac{R}{r}t) \rangle.
~$$



To visualize this we first define a helper function to draw a circle at point $P$ with radius $R$:

````julia

circle!(P, R; kwargs...) = plot_parametric_curve!(t -> P + R*[cos(t), sin(t)], 0, 2pi;kwargs...)
````


````
circle! (generic function with 1 method)
````





Then we have this function to visualize the spirograph for different $t$ values:

````julia

function spiro(t; r=2, R=5, rho=0.8*r)

    cent(t) = (R-r) * [cos(t), sin(t)]

    p = plot(legend=false, aspect_ratio=:equal)
    circle!([0,0], R, color=:blue)
    circle!(cent(t), r, color=:black)

    tp(t) = -R/r * t

    s(t) = cent(t) + rho * [cos(tp(t)), sin(tp(t))]
    plot_parametric_curve!(s, 0, t, n=1000, color=:red)

    p
end
````


````
spiro (generic function with 1 method)
````





And we can see the trace for $t=\pi$:


````julia

spiro(pi)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_18_1.png)



The point of contact is at $(-R, 0)$, as expected. Carrying this forward to a full circle's worth is done through:

````julia

spiro(2pi)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_19_1.png)



The curve does not match up at the start. For that, a second time around the outer circle is needed:

````julia

spiro(4pi)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_20_1.png)



Whether the curve will have a period or not is decided by the ratio of $R/r$ being rational or irrational.


##### Example

[Ivars Peterson](http://www.phschool.com/science/science_news/articles/tilt_a_whirl.html) described the carnival ride "tilt-a-whirl" as a chaotic system, whose equations of motion are presented in [American Journal of Physics](https://doi.org/10.1119/1.17742) by Kautz and Huggard. The tilt-a-whirl has a platform that moves in a circle that also moves up and down. To describe the  motion of a point on the platform assuming it has radius $R$ and period $T$ and rises twice in that period could be done with the function:

$$~
\vec{u}(t) = \langle R \sin(2\pi t/T), R \cos(2\pi t/T), h + h \cdot \sin(2\pi t/ T) \rangle.
~$$

A passenger sits on a circular platform with radius $r$ attached at some point on the larger platform. The dynamics of the person on the tilt-a-whirl depend on physics, but for simplicity, let's assume the platform moves at a constant rate with period $S$ and has no relative $z$ component. The motion of the platform in relation to the point it is attached would be modeled by:

$$~
\vec{v}(t) = \langle r \sin(2\pi t/S), r \sin(2\pi t/S), 0 \rangle.
~$$

And the motion relative to the origin would be the vector sum, or superposition:

$$~
\vec{f}(t) = \vec{u}(t) + \vec{v}(t).
~$$

To visualize for some parameters, we have:

````julia

plotly()
M, m = 25, 5
height = 5
S, T = 8, 2
outer(t) = [M * sin(2pi*t/T), M * cos(2pi*t/T), height*(1 +sin(2pi * (t-pi/2)/T))]
inner(t) = [m * sin(2pi*t/S), m * cos(2pi*t/S), 0]
f(t) = outer(t) + inner(t)
plot_parametric_curve(f, 0, 8, n=1000)
````


````
Plot{Plots.PlotlyBackend() n=1}
````





## Limits and continuity

The definition of a limit for a univariate function is: For every $\epsilon > 0$ there exists a $\delta > 0$ such that *if* $0 \leq |x-c| < \delta$ *then* $|f(x) - L | < \epsilon$.

If  the notion of "$f$ is close to $L$" is replaced by close in the sense of a norm, or vector distance, then the same limit definition can be used, with the new wording "... $\| \vec{f}(x) - L \| < \epsilon$".


The notion of continuity is identical: $\vec{f}(t)$ is continuous at $t_0$ if $\lim_{t \rightarrow t_0}\ve{f}(t) = \vec{f}(t_0)$.

A consequence of the triangle inequality is that a vector-valued function is continuous or has a limit if and only it its component functions do.

### derivatives

If $\vec{f}(t)$ is  vector valued,  and $\Delta t > 0$ then we can consider the vector:

$$~
\vec{f}(t + \Delta t) - \vec{f}(t)
~$$

For example, if $\vec{f}(t) = \langle 3\cos(t), 2\sin(t) \rangle$ and $t=\pi/4$ and $\Delta t = \pi/16$ we have this picture:

````julia
gr()  # better arrows
f(t) = [3cos(t), 2sin(t)]
t, Deltat = pi/4, pi/16
df = f(t + Deltat) - f(t)

plot(legend=false)
arrow!([0,0], f(t))
arrow!([0,0], f(t+Deltat))
arrow!(f(t), df)
````





The length of the difference appears to be related to the length of $\Delta t$, in a similar manner as the univariate derivative. The following limit defines the *derivative* of a vector-valued function:

$$~
\vec{f}'(t) = \lim_{\Delta t \rightarrow 0} \frac{f(t + \Delta t) - f(t)}{\Delta t}.
~$$

The limit exists if the component limits do. The component limits are just the derivatives of the component functions. So, if $\vec{f}(t) = \langle x(t), y(t) \rangle$, then $\vec{f}'(t) = \langle x'(t), y'(t) \rangle$.

If the derivative is never $\vec{0}$, the curve is called *regular*. For a regular
curve
the derivative is a tangent vector to the parameterized curve, akin to the case for a univariate function. We can use `ForwardDiff` to compute the derivative in the exact same manner as was done for univariate functions:

````julia

using ForwardDiff
D(f,n=1) = n > 1 ? D(D(f),n-1) : x -> ForwardDiff.derivative(f, float(x))
Base.adjoint(f::Function) = D(f)         # allow f' to compute derivative
````




(This is already done by the `CalculusWithJulia package.)

We can visualize the tangential property through a graph:

````julia

f(t) = [3cos(t), 2sin(t)]
p = plot_parametric_curve(f, 0, 2pi, legend=false, aspect_ratio=:equal)
for t in [1,2,3]
    arrow!(f(t), f'(t))   # add arrow with tail on curve, in direction of derivative
end
p
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_24_1.png)




### Symbolic representation

Were symbolic expressions used in place of functions, the vector-valued function would naturally be represented as a vector of expressions:

````julia

using SymPy
@vars t
vvf = [cos(t), sin(t), t]
````


````
3-element Array{Sym,1}:
 cos(t)
 sin(t)
      t
````





We will see working with these expressions is not identical to working with a vector-valued function.

To plot, we can avail ourselves of the the parametric plot syntax. The following expands to `plot(cos(t), sin(t), t, 0, 2pi)`:


````julia

plot(vvf..., 0, 2pi)
````


````
Plot{Plots.PlotlyBackend() n=1}
````






The `unzip` usage, as was done above, could be used, but it would be  more trouble in this case.

To evaluate the function at a given value, say $t=2$, we can use `subs` with broadcasting to substitute into each component:

````julia

subs.(vvf, t.=>2)
````


````
3-element Array{Sym,1}:
 cos(2)
 sin(2)
      2
````





The notation is a bit subtle: `subs.(` will broadcast over the 3 components in `vvf`. This is familiar, but broadcasting tries to match up sizes of collections. Without a "dot", `vvf` would have size 3 and `t=>2` has size 2 as an iterable object; and this causes a mismatch. We need to make `t=>2` appear to have just length 1. The `t.=>2` expresses this. (The "dot" syntactically indicates that `t .=> 2` will be a scalar.). Alternatively,  would wrapping the pair in a tuple, as in `(t=>2, )` or using `Ref`, as in `Ref(t=>2)`, will ensure broadcasting treats the pair as a scalar value. (This will be unnecessary as version `1.3` of `Julia`, as pairs will not be considered iterable for broadcasting purposes.)


Limits are performed component by component, and can also be defined by broadcasting, again with the need to adjust the values:

````julia

@syms delta
limit.((subs.(vvf, t .=> t + delta) - vvf) / delta, delta .=> 0)
````


````
3-element Array{Sym,1}:
 -sin(t)
  cos(t)
       1
````






Derivatives, as was just done through a limit, are a bit more
straightforward than evaluation or limit taking, as we won't bump into
the shape mismatch when broadcasting:

````julia

diff.(vvf, t)
````


````
3-element Array{Sym,1}:
 -sin(t)
  cos(t)
       1
````





The second derivative, can be found through:

````julia

diff.(vvf, t, t)
````


````
3-element Array{Sym,1}:
 -cos(t)
 -sin(t)
       0
````



````
CalculusWithJulia.WeaveSupport.Alert("If these differences are too subtle, 
it might be best to work with a function and when a symbolic expression is 
desirable, simply evaluate the function for a symbolic value.\n", Dict{Any,
Any}(:class => "info"))
````





### Applications of the derivative

Here are some sample applications of the derivative.

##### Example: equation of the tangent line
The derivative of a vector-valued function is similar to that of a univariate function, in that it indicates a direction tangent to a curve. The point-slope form offers a straightforward parameterization. We have a point given through the vector-valued function and a direction given by its derivative. (After identifying a vector with its tail at the origin with the point that is the head of the vector.)

With this, the equation is simply $\vec{tl}(t) = \vec{f}(t_0) + \vec{f}'(t_0) \cdot (t - t_0)$, where the dot indicates scalar multiplication.



##### Example: parabolic motion

In physics, we learn that the equation $F=ma$ can be used to derive a formula for postion, when acceleration, $a$, is a constant. The resulting equation of motion is $x = x_0 + v_0t + (1/2) at^2$. Similarly, if $x(t)$ is a vector-valued postion vector, and the *second* derivative, $x''(t) =\vec{a}$, a constant, then we have: $x(t) = \vec{x_0} + \vec{v_0}t + (1/2) \vec{a} t^2$.

For two dimensions, we have the force due to gravity acts downward, only in the $y$ direction. The acceleration is then $\vec{a} = \langle 0, -g \rangle$. If we start at the origin, with initial velocity $\vec{v_0} = \langle 2, 3\rangle$, then we can plot the trajectory until the object returns to ground ($y=0$) as follows:

````julia

gravity = 9.8
x0, v0, a = [0,0], [2, 3], [0, -gravity]
xpos(t) = x0 + v0*t + (1/2)*a*t^2

using Roots
t_0 = fzero(t -> xpos(t)[2], 1/10, 100)  # find when y=0

plot_parametric_curve(xpos, 0, t_0)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_34_1.png)




##### Example: tractrix




A [tractrix](https://en.wikipedia.org/wiki/Tractrix), studied by Perrault, Newton, Huygens, and many others, is the curve along which an object moves when pulled in a horizontal plane by a line segment attached to a pulling point (Wikipedia). If the object is placed at $(a,0)$ and the puller at the origin, and the puller moves along the positive $x$ axis, then the line will always be tangent to the curve and of fixed length, so determinable from the motion of the puller. In this example $dy/dx = -\sqrt{a^2-x^2}/x$.

This is the key property: "Due to the geometrical way it was defined, the tractrix has the property that the segment of its tangent, between the asymptote and the point of tangency, has constant length $a$."


The tracks made by the front and rear bicycle wheels also have this
same property and similarly afford a mathematical description. We follow
[Dunbar, Bosman, and Nooij](https://doi.org/10.2307/2691097) from *The
Track of a Bicycle Back Tire* below, though
[Levi and Tabachnikov](https://projecteuclid.org/download/pdf_1/euclid.em/1259158427)
and
[Foote, Levi, and Tabachnikov](https://www.tandfonline.com/doi/abs/10.4169/amer.math.monthly.120.03.199)
were also consulted.  Let $a$ be the distance between the front and
back wheels, whose positions are parameterized by $\vec{F}(t)$ and
$\vec{B}(t)$, respectively. The key property is the distance between
the two is always $a$, and, as the back wheel is always moving in the
direction of the front wheel, we have $\vec{B}'(t)$ is in the
direction of $\vec{F}(t) - \vec{B}(t)$, that is the vector
$(\vec{F}(t)-\vec{B}(t))/a$ is a unit vector in the direction of the
derivative of $\vec{B}$. How long is the derivative vector? That would
be answered by the speed of the back wheel, which is related to the
velocity of the front wheel. But only the component of the velocity in
the direction of $\vec{F}(t)-\vec{B}(t)$, so the speed of the back
wheel is the length of the projection of $\vec{F}'(t)$ onto the unit
vector $(\vec{F}(t)-\vec{B}(t))/a$, which is identified through the dot product.

Combined, this gives the following equations relating $\vec{F}(t)$ to $\vec{B}(t)$:

$$~
s_B(t) = \vec{F}'(t) \cdot \frac{\vec{F}(t)-\vec{B}(t)}{a}, \quad
\vec{B}'(t) = s_B(t) \frac{\vec{F}(t)-\vec{B}(t)}{a}.
~$$

This is a *differential* equation describing the motion of the back wheel in terms of the front wheel.

If the back wheel trajectory is known, the relationship is much easier, as the two differ by a vector of length $a$ in the direction of $\vec{B}'(t)$, or:

$$~
F(t) = \vec{B}(t) + a \frac{\vec{B'(t)}}{\|\vec{B}'(t)\|}.
~$$


We don't discuss when a differential equation has a solution, or if it
is unique when it does, but note that the differential equation above
may be solved numerically, in a manner somewhat similar to what was
discussed in [ODEs](../ODEs/odes.html). Though here we will use the
`DifferentialEquations` package for finding the numeric solution.

We can define our equation as follows, using `p` to pass in the two parameters: the wheel-base length $a$, and $F(t)$, the parameterization of the front wheel in time:

````julia

using DifferentialEquations, LinearAlgebra

function bicycle(dB, B, p, t)

  a, F = p   # unpack parameters

  speed =  F'(t) ⋅ (F(t) - B) / a
  dB[1], dB[2] = speed * (F(t) - B) / a

end
````


````
bicycle (generic function with 1 method)
````





Let's consider a few simple cases first. We suppose $a=1$ and the front wheel moves in a circle of radius $3$. Here is how we can plot two loops:

````julia

t0, t1 = 0.0, 4pi
tspan = (t0, t1)  # time span to consider

a = 1
F(t) = 3 * [cos(t), sin(t)]
p = (a, F)      # combine parameters

B0 = F(0) - [0, a]  # some initial position for the back
prob = ODEProblem(bicycle, B0, tspan, p)

out = DifferentialEquations.solve(prob, reltol=1e-6)
````





We qualify the `solve` function, as `SymPy` also has a `solve` function. (We would also need to qualify that as `SymPy.solve` once `DifferentialEquations` is loaded.) The object `out` holds the answer. This object is callable, in that `out(t)` will return the numerically computed value for the answer to our equation at time point `t`.

To plot the two trajectories, we could use that `out.u` holds the $x$ and $y$ components of the computed trajectory, but more simply, we  call `out` like a function.

````julia

plt = plot_parametric_curve(F, t0, t1, legend=false)
plot_parametric_curve!(out,  t0, t1, linewidth=3)

## add the bicycle as a line segment at a few times along the path
for t in range(t0, t1, length=11)
    plot!(unzip([out(t), F(t)])..., linewidth=3, color=:black)
end
plt
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_38_1.png)




That the rear wheel track appears shorter, despite the rear wheel starting outside the circle, is typical of bicycle tracks and also a reason to rotate tires on car, as the front ones move a bit more than the rear, so presumably wear faster.

Let's look what happens if the front wheel wobbles back and forth following a sine curve. Repeating the above, only with $F$ redefined, we have:

````julia

a = 1
F(t) = [t, 2sin(t)]
p = (a, F)

B0 = F(0) - [0, a]  # some initial position for the back
prob = ODEProblem(bicycle, B0, tspan, p)

out = DifferentialEquations.solve(prob, reltol=1e-6)
plt = plot_parametric_curve(F, t0, t1, legend=false)
plot_parametric_curve!(t->out(t),  t0, t1, linewidth=3)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_39_1.png)



Again, the back wheel moves less than the front.


The motion of the back wheel need not be smooth, even if the motion of the front wheel is, as this curve illustrates:

````julia

a = 1
F(t) = [cos(t), sin(t)] + [cos(2t), sin(2t)]
p = (a, F)

B0 = F(0) - [0,a]
prob = ODEProblem(bicycle, B0, tspan, p)

out = DifferentialEquations.solve(prob, reltol=1e-6)
plt = plot_parametric_curve(F, t0, t1, legend=false)
plot_parametric_curve!(t->out(t),  t0, t1, linewidth=3)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_40_1.png)



The back wheel is moving backwards for part of the above trajectory.


This effect can happen even for a front wheel motion as simple as a circle when the front wheel radius is less than the wheelbase:

````julia

a = 1
F(t) = a/3 * [cos(t), sin(t)]
p = (a, F)

t0, t1 = 0.0, 25pi
tspan = (t0, t1)

B0 = F(0) - [0,a]
prob = ODEProblem(bicycle, B0, tspan, p)

out = DifferentialEquations.solve(prob, reltol=1e-6)
plt = plot_parametric_curve(F, t0, t1, legend=false, aspect_ratio=:equal)
plot_parametric_curve!(t->out(t),  t0, t1, linewidth=3)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_41_1.png)




Later we will characterize when there are cusps in the rear-wheel trajectory.


## Derivative rules.

From the definition, as it is for univariate functions, for vector-valued functions $\vec{f}, \vec{g}: R \rightarrow R^n$:

$$~
[\vec{f} + \vec{g}]'(t) = \vec{f}'(t) + \vec{g}'(t), \quad\text{and }
[a\vec{f}]'(t) = a \vec{f}'(t).
~$$

If $a(t)$ is a univariate (scalar) function of $t$, then a product rule holds:

$$~
[a(t) \vec{f}(t)]' = a'(t)\vec{f}(t) + a(t)\vec{f}'(t).
~$$

If $s$ is a univariate function, then the composition $\vec{f}(s(t))$ can be differentiated. Each component would satisfy the chain rule, and consequently:

$$~
\frac{d}{dt}\left(\vec{f}(s(t))\right) = \vec{f}'(s(t)) \cdot s'(t),
~$$

The dot being scalar multiplication by the derivative of the univariate function $s$.

vector-valued functions do not have multiplication or division defined for them, so there are no ready analogues of the product and quotient rule. However, the dot product and the cross product produce new functions that may have derivative rules available.

For the dot product, the combination $\vec{f}(t) \cdot \vec{g}(t)$ we have a univariate function of $t$, so we know a derivative is well defined. Can it be represented in terms of the vector-valued functions? In terms of the component functions, we have this calculation specific to $n=2$, but that which can be generalized:

$$~
\frac{d}{dt}(\vec{f}(t) \cdot \vec{g}(t)) =
\frac{d}{dt}(f_1(t) g_1(t) + f_2(t) g_2(t)) =
f_1'(t) g_1(t) + f_1(t) g_1'(t) + f_2'(t) g_2(t) + f_2(t) g_2'(t) =
f_1'(t) g_1(t) + f_2'(t) g_2(t) + f_1(t) g_1'(t)  + f_2(t) g_2'(t) =
\vec{f}'(t)\cdot \vec{g}(t) + \vec{f}(t) \cdot \vec{g}'(t).
~$$

Suggesting the that a product rule like formula applies for dot products.


For the cross product, we let `SymPy` derive a formula for us.

````julia

u1, u2, u3, v1, v2, v3 = SymFunction("u1, u2, u3, v1, v2, v3")
@vars t
u = [u1(t), u2(t), u3(t)]
v = [v1(t), v2(t), v3(t)]
````


````
3-element Array{Sym,1}:
 v₁(t)
 v₂(t)
 v₃(t)
````





Then the cross product a derivative:

````julia

using LinearAlgebra
diff.(u × v, t)
````


````
3-element Array{Sym,1}:
  u2(t)*Derivative(v3(t), t) - u3(t)*Derivative(v2(t), t) - v2(t)*Derivativ
e(u3(t), t) + v3(t)*Derivative(u2(t), t)
 -u1(t)*Derivative(v3(t), t) + u3(t)*Derivative(v1(t), t) + v1(t)*Derivativ
e(u3(t), t) - v3(t)*Derivative(u1(t), t)
  u1(t)*Derivative(v2(t), t) - u2(t)*Derivative(v1(t), t) - v1(t)*Derivativ
e(u2(t), t) + v2(t)*Derivative(u1(t), t)
````





Admittedly, that isn't very clear. With a peek at the answer, we show that the derivative is the same as the product rule would suggest ($\vec{u}' \times \vec{v} + \vec{u} \times \vec{v}'$):

````julia

diff.(u × v, t) - (diff.(u,t) × v + u × diff.(v,t))
````


````
3-element Array{Sym,1}:
 0
 0
 0
````






In summary, these two derivative formulas hold for vector-valued functions $R \rightarrow R^n$:

$$~
\begin{align}
(\vec{u} \cdot \vec{v})' &= \vec{u}' \cdot \vec{v} + \vec{u} \cdot \vec{v}',\\
(\vec{u} \times \vec{v})' &= \vec{u}' \times \vec{v} + \vec{u} \times \vec{v}'.
\end{align}
~$$

##### Application. Circular motion and the tangent vector.

The parameterization $\vec{r}(t) = \langle \cos(t), \sin(t) \rangle$ describes a circle. Characteristic of this motion is a constant radius, or in terms of a norm: $\| \vec{r}(t) \| = c$. The norm squared, can be expressed in terms of the dot product:

$$~
\| \vec{r}(t) \|^2 = \vec{r}(t) \cdot \vec{r}(t).
~$$

Differentiating this for the case of a constant radius yields the
equation $0 = [\vec{r}\cdot\vec{r}]'(t)$, which simplifies through the
product rule and commutativity of the dot product to $0 = 2 \vec{r}(t)
\cdot \vec{r}'(t)$. That is, the two vectors are orthogonal to each
other. This observation proves to be very useful, as will be seen.


##### Example: Kepler's laws

[Kepler](https://tinyurl.com/y38wragh)'s laws of planetary motion are summarized by:

* The orbit of a planet is an ellipse with the Sun at one of the two foci.

* A line segment joining a planet and the Sun sweeps out equal areas during equal intervals of time.

* The square of the orbital period of a planet is directly proportional to the cube of the semi-major axis of its orbit.

Kepler was a careful astronomer, and derived these laws empirically.
We show next how to derive these laws using vector calculus assuming some facts on Newtonian motion, as postulated by Newton. This approach is borrowed from [Joyce](https://mathcs.clarku.edu/~djoyce/ma131/kepler.pdf).

We adopt a sun-centered view of the universe, placing the sun at the origin and letting $\vec{x}(t)$ be the position of a planet relative to this origin. We can express this in terms of a magnitude and direction through $r(t) \hat{x}(t)$.

Newton's law of gravitational force between the sun and this planet is then expressed by:


$$~
\vec{F} = -\frac{G M m}{r^2} \hat{x}(t).
~$$

Newton's famous law relating force and acceleration is

$$~
\vec{F} = m \vec{a} = m \ddot{\vec{x}}.
~$$

Combining, Newton states $\vec{a} = -(GM/r^2) \hat{x}$.

Now to show the first law. Consider $\vec{x} \times \vec{v}$. It is constant, as:

$$~
\begin{align}
(\vec{x} \times \vec{v})' &= \vec{x}' \times \vec{v} + \vec{x} \times \vec{v}'\\
&= \vec{v} \times \vec{v} + \vec{x} \times \vec{a}.
\end{align}
~$$

Both terms are $\vec{0}$, as $\vec{a}$ is parallel to $\vec{x}$ by the above, and clearly $\vec{v}$ is parallel to itself.

This says, $\vec{x} \times \vec{v} = \vec{c}$ is a constant vector, meaning, the motion of $\vec{x}$ must lie in a plane, as $\vec{x}$ is always orthogonal to the fixed vector $\vec{c}$.

Now, by differentiating $\vec{x} = r \hat{x}$ we have:

$$~
\begin{align}
\vec{v} &= \vec{x}'\\
&= (r\hat{x})'\\
&= r' \hat{x} + r \hat{x}',
\end{align}
~$$

and so

$$~
\begin{align}
\vec{c} &= \vec{x} \times \vec{v}\\
&= (r\hat{x}) \times (r'\hat{x} + r \hat{x}')\\
&= r^2 (\hat{x} \times \hat{x}').
\end{align}
~$$

From this, we can compute $\vec{a} \times \vec{c}$:

$$~
\begin{align}
\vec{a} \times \vec{c} &= (-\frac{GM}{r^2})\hat{x} \times r^2(\hat{x} \times \hat{x}')\\
&= -GM \hat{x} \times (\hat{x} \times \hat{x}') \\
&= GM (\hat{x} \times \hat{x}')\times \hat{x}.
\end{align}
~$$

The last line by anti-commutativity.

But, the triple cross product can be simplified through the identify
$(\vec{u}\times\vec{v})\times\vec{w} = (\vec{u}\cdot\vec{w})\vec{v} - (\vec{v}\cdot\vec{w})\vec{u}$. So, the above becomes:

$$~
\begin{align}
\vec{a} \times \vec{c} &=  GM ((\hat{x}\cdot\hat{x})\hat{x}' - (\hat{x} \cdot \hat{x}')\hat{x})\\
&= GM (1 \hat{x}' - 0 \hat{x}).
\end{align}
~$$

Now, since $\vec{c}$ is constant, we have:

$$~
\begin{align}
(\vec{v} \times \vec{c})' &= (\vec{a} \times \vec{c})\\
&= GM \hat{x}'\\
&= (GM\hat{x})'.
\end{align}
~$$

The two sides have the same derivative, hence differ by a constant:

$$~
\vec{v} \times \vec{c} = GM \hat{x} + \vec{d}.
~$$

As $\vec{u}$ and $\vec{v}\times\vec{c}$ lie in the same plane - orthogonal to $\vec{c}$ - so does $\vec{d}$. With a suitable re-orientation, so that $\vec{d}$ is along the $x$ axis, $\vec{c}$ is along the $z$-axis, then we have $\vec{c} = \langle 0,0,c\rangle$ and $\vec{d} = \langle d ,0,0 \rangle$, and $\vec{x} = \langle x, y, 0 \rangle$. Set $\theta$ to be the angle, then $\hat{x} = \langle \cos(\theta), \sin(\theta), 0\rangle$.

Now

$$~
\begin{align}
c^2 &= \|\vec{c}\|^2 \\
&= \vec{c} \cdot \vec{c}\\
&= (\vec{x} \times \vec{v}) \cdot \vec{c}\\
&= \vec{x} \cdot (\vec{v} \times \vec{c})\\
&= r\hat{x} \cdot (GM\hat{x} + \vec{d})\\
&= GMr + r \hat{x} \cdot \vec{d}\\
&= GMr + rd \cos(\theta).
\end{align}
~$$

Solving, this gives the radial distance in the form of an ellipse:

$$~
r = \frac{c^2}{GM + d\cos(\theta)} =
\frac{c^2/(GM)}{1 + (d/GM) \cos(\theta)}.
~$$



Kepler's second law can also be derived from vector calculus. This derivation follows that given at [MIT OpenCourseWare](https://ocw.mit.edu/courses/mathematics/18-02sc-multivariable-calculus-fall-2010/1.-vectors-and-matrices/part-c-parametric-equations-for-curves/session-21-keplers-second-law/MIT18_02SC_MNotes_k.pdf) [OpenCourseWare](https://ocw.mit.edu/courses/mathematics/18-02sc-multivariable-calculus-fall-2010/index.htm).

The second law states that the area being swept out during a time duration only depends on the duration of time, not the time. Let $\Delta t$ be this duration. Then if $\vec{x}(t)$ is the position vector, as above, we have the area swept out between $t$ and $t + \Delta t$ is visualized along the lines of:


````julia
x1(t) = [cos(t), 2 * sin(t)]
t0, t1, Delta = 1.0, 2.0, 1/10
plot_parametric_curve(x1, 0, pi/2)

arrow!([0,0], x1(t0)); arrow!([0,0], x1(t0 + Delta))
arrow!(x1(t0), x1(t0+Delta)- x1(t0), linewidth=5)
````





The area swept out, is basically the half the area of the parallelogram formed by $\vec{x}(t)$ and $\Delta \vec{x}(t) = \vec{x}(t + \Delta t) - \vec{x}(t))$. This area is $(1/2) (\vec{x} \times \Delta\vec{x}(t))$.

If we divide through by $\Delta t$, and take a limit we have:

$$~
\frac{dA}{dt} = \| \frac{1}{2}\lim_{\Delta t \rightarrow 0} (\vec{x} \times \frac{\vec{x}(t + \Delta t) - \vec{x}(t)}{\Delta t})\| =
\frac{1}{2}\|\vec{x} \times \vec{v}\|.
~$$

But we saw above, that for the motion of a planet, that $\vec{x} \times \vec{v} = \vec{c}$, a constant. This says, $dA$ is a constant independent of $t$, and consequently, the area swept out over a duration of time will not depend on the particular times involved, just the duration.


The third law relates the period to a parameter of the ellipse. We have from the above a strong suggestion that area of the ellipse can be found by integrating $dA$ over the period, say $T$. Assuming that is the case and letting $a$ be the semi-major axis length, and $b$ the semi-minor axis length, then

$$~
\pi a b = \int_0^T dA = \int_0^T (1/2) \|\vec{x} \times \vec{v}\| dt = \| \vec{x} \times \vec{v}\| \frac{T}{2}.
~$$

As $c = \|\vec{x} \times \vec{v}\|$ is a constant, this allows us to express $c$ by: $2\pi a b/T$.

But, we have

$$~
r(\theta) = \frac{c^2}{GM + d\cos(\theta)} = \frac{c^2/(GM)}{1 + d/(GM) \cos(\theta)}.
~$$

So, $e = d/(GM)$ and $a (1 - e^2) = c^2/(GM)$. Using $b = a \sqrt{1-e^2}$ we have:

$$~
a(1-e^2) = c^2/(GM) = (\frac{2\pi a b}{T})^2 \frac{1}{GM} =
\frac{(2\pi)^2}{GM} \frac{a^2 (a^2(1-e^2))}{T^2},
~$$

or after cancelling $(1-e^2)$ from each side:

$$~
T^2 = \frac{(2\pi)^2}{GM} \frac{a^4}{a} = \frac{(2\pi)^2}{GM} a^3.
~$$


----

The above shows how Newton might have derived Kepler's observational facts. Next we show, that assuming the laws of Kepler can anticipate Newton's equation for gravitational force. This follows [Wikipedia](https://en.wikipedia.org/wiki/Kepler%27s_laws_of_planetary_motion#Planetary_acceleration).

Now let $\vec{r}(t)$ be the position of the planet relative to the Sun at the origin, in two dimensions (we used $\vec{x}(t)$ above). Assume $\vec{r}(0)$ points in the $x$ direction.
Write $\vec{r} = r \hat{r}$. Define $\hat{\theta(t)}$ to be the mapping from time $t$ to the angle  defined by $\hat{r}$ through the unit circle.

Then we express the velocity ($\dot{\vec{r}}$) and acceleration ($\ddot{\vec{r}}$) in terms of the orthogonal vectors $\hat{r}$ and $\hat{\theta}$, as follows:

$$~
\frac{d}{dt}(r \hat{r}) = \dot{r} \hat{r} + r \dot{\hat{r}} =  \dot{r} \hat{r} + r \dot{\theta}\hat{\theta}.
~$$

The last equality from expressing $\hat{r}(t) = \hat{r}(\theta(t))$ and using the chain rule, noting $d(\hat{r}(\theta))/d\theta = \hat{\theta}$.

Continuing,

$$~
\frac{d^2}{dt^2}(r \hat{r}) =
(\ddot{r} \hat{r} + \dot{r} \dot{\hat{r}}) +
(\dot{r} \dot{\theta}\hat{\theta} + r \ddot{\theta}\hat{\theta} + r \dot{\theta}\dot{\hat{\theta}}).
~$$

Noting, similar to above, $\dot{\hat{\theta}}  = d\hat{\theta}/dt = d\hat{\theta}/d\theta \cdot d\theta/dt = -\dot{\theta} \hat{r}$ we can express the above in terms of $\hat{r}$ and $\hat{\theta}$ as:

$$~
\vec{a} = \frac{d^2}{dt^2}(r \hat{r}) = (\ddot{r} - r (\dot{\theta})^2) \hat{r}  + (r\ddot{\theta}  + 2\dot{r}\dot{\theta}) \hat{\theta}.
~$$

That is, in general, the acceleration has a radial component and a transversal component.

Kepler's second law says that the area increment over time is constant ($dA/dt$), but this area increment is approximated by the following wedge in polar coordinates: $dA = (1/2) r \cdot rd\theta$. We have then $dA/dt = r^2 \dot{\theta}$ is constant.

Differentiating, we have:

$$~
0 = \frac{d(r^2 \dot{\theta})}{dt} = 2r\dot{r}\dot{\theta} + r^2 \ddot{\theta},
~$$

which is the tranversal component of the acceleration times $r$, as decomposed above. This means, that the acceleration of the planet is completely towards the Sun at the origin.

Kepler's first law, relates $r$ and $\theta$ through the polar equation of an ellipse:

$$~
r = \frac{p}{1 + \epsilon \cos(\theta)}.
~$$

Expressing in terms of $p/r$ and differentiating in $t$ gives:

$$~
-\frac{p \dot{r}}{r^2} = -\epsilon\sin(\theta) \dot{\theta}.
~$$

Or

$$~
p\dot{r} = \epsilon\sin(\theta) r^2 \dot{\theta} = \epsilon \sin(\theta) C,
~$$

For a constant $C$, used above, as the second laws implies $r^2 \dot{\theta}$ is constant. (This constant can be expressed in terms of parameters describing the ellipse.)

Differentiating again in $t$, gives:

$$~
p \ddot{r} = C\epsilon \cos(\theta) \dot{\theta} = C\epsilon \cos(\theta)\frac{C}{r^2}.
~$$

So $\ddot{r} = (C^2 \epsilon / p) \cos{\theta} (1/r^2)$.

The radial acceleration from above is:

$$~
\ddot{r} - r (\dot{\theta})^2 =
(C^2 \epsilon/p) \cos{\theta} \frac{1}{r^2} - r\frac{C^2}{r^4} = \frac{C^2}{pr^2}(\epsilon \cos(\theta) - \frac{p}{r}).
~$$

Using $p/r = 1 + \epsilon\cos(\theta)$, we have the radial acceleration is $C^2/p \cdot (1/r^2)$. That is the acceleration, is proportional to the inverse square of the position, and using the relation between $F$, force, and acceleration, we see the force on the planet follows the inverse-square law of Newton.



### Moving frames of reference

In the last example, it proved useful to represent vectors in terms of
other unit vectors, in that case $\hat{r}$ and $\hat{\theta}$. Here we
discuss a coordinate system defined intrinsically by the motion along
the trajectory of a curve.

Let $r(t)$ be a smooth vector-valued function in $R^3$. It gives rise to a space curve, through its graph. This curve has tangent vector $\vec{r}'(t)$, indicating the direction of travel along $\vec{r}$ as $t$ increases. The length of $\vec{r}'(t)$ depends on the parameterization of $\vec{r}$, as for any increasing, differentiable function $s(t)$, the composition $\vec{r}(s(t))$ will have derivative, $\vec{r}'(s(t)) s'(t)$, having the same direction as $\vec{r}'(t)$ (at suitably calibrated points), but not the same magnitude, the factor of $s(t)$ being involved.

To discuss properties intrinsic to the curve, the unit vector is considered:

$$~
\hat{T}(t) = \frac{\vec{r}'(t)}{\|\vec{r}'(t)\|}.
~$$


The function $\hat{T}(t)$ is the unit tangent vector.
Now define the unit *normal*, $\hat{N}(t)$, by:

$$~
\hat{N}(t) = \frac{\hat{T}'(t)}{\| \hat{T}'(t) \|}.
~$$

Since $\|\hat{T}(t)\| = 1$, a constant, it must be that $\hat{T}'(t) \cdot \hat{T}(t) = 0$, that is, the $\hat{N}$ and $\hat{T}$ are orthogonal.

Finally, define the *binormal*, $\hat{B}(t) = \hat{T}(t) \times \hat{N}(t)$. At each time $t$, the three unit vectors are orthogonal to each other. They form a moving coordinate system for the motion along the curve that does not depend on the parameterization.

We can visualize this, for example along a [Viviani](https://tinyurl.com/y4lo29mv) curve, as is done in a [Wikipedia](https://en.wikipedia.org/wiki/Frenet%E2%80%93Serret_formulas) animation:


````julia

function viviani(t, a=1)
    [a*(1-cos(t)), a*sin(t), 2a*sin(t/2)]
end


Tangent(t) = viviani'(t)/norm(viviani'(t))
Normal(t) = Tangent'(t)/norm(Tangent'(t))
Binormal(t) = Tangent(t) × Normal(t)

p = plot(legend=false, aspect_ratio=:equal)
plot_parametric_curve!(viviani, -2pi, 2pi)

t0, t1 = -pi/3, pi/2 + 2pi/5
r0, r1 = viviani(t0), viviani(t1)
arrow!(r0, Tangent(t0)); arrow!(r0, Binormal(t0)); arrow!(r0, Normal(t0))
arrow!(r1, Tangent(t1)); arrow!(r1, Binormal(t1)); arrow!(r1, Normal(t1))
p
````


````
Plot{Plots.PlotlyBackend() n=1}
````








----

The *curvature* of a 3-dimensional space curve is defined by:

$$~
\kappa = \frac{\| r'(t) \times r''(t) \|}{\| r'(t) \|^3}
~$$


For $2$ dimensional space curves, the same formula applies after embedding a $0$ third component. It can also be expressed directly as $(x'y''-x''y')/\|r'\|^3$.



This can also be defined as derivative of the tangent vector, $\hat{T}$,
*when* the curve is parameterized by arc length, a topic still to be taken up.  The vector $\vec{r}'(t)$ is the direction of motion, whereas
$\vec{r}''(t)$ indicates how fast and in what direction this is
changing. For curves with little curve in them, the two will be nearly
parallel and the cross product small (reflecting the presence of
$\cos(\theta)$ in the definition). For "curvy" curves, $\vec{r}''$ will be
in a direction opposite of $\vec{r}'$ to the $\cos(\theta)$ term in the
cross product will be closer to $1$.

Let $\vec{r}(t) = k \cdot \langle \cos(t), \sin(t), 0 \rangle$. This will have curvature:

````julia

@vars k positive=true
@vars t real=true
r1 = k * [cos(t), sin(t),0]
norm(diff.(r1,t) × diff.(r1,t,t)) / norm(diff.(r1,t))^3 |> simplify
````


````
Error: UndefVarError: simplify not defined
````





For larger circles (bigger $\|k\|$) there is less curvature. The limit being a line with curvature $0$.

If a curve is imagined to have a tangent "circle" (second order Taylor series approximation), then the curvature of that circle matches the curvature of the curve.


The [torsion](https://en.wikipedia.org/wiki/Torsion_of_a_curve), $\tau$, of a space curve ($n=3$), is a measure of how sharply the curve is twisting out of the plane of curvature.

The torsion is defined for smooth curves by

$$~
\tau = \frac{(\vec{r}' \times \vec{r}'') \cdot \vec{r}'''}{\|\vec{r}' \times \vec{r}''\|^2}.
~$$

For the torsion to be defined, the cross product $\vec{r}' \times \vec{r}''$ must be non zero, that is the two must not be parallel or zero.


## Arc length

In [Arc length](../integrals/arc_length.html) there is a discussion of how to find the arc length of a parameterized curve in 2 dimensions. The general case is discussed by [Destafano](https://randomproofs.files.wordpress.com/2010/11/arc_length.pdf) who shows if the curve $C$ is parameterized by a smooth function $\vec{r}(t)$ over an interval $I$, that the arc length of $C$ is $\int_I \| \vec{r}'(t)  \| dt$.

If we associate $\vec{r}'(t)$ with the velocity, then this is the integral of the speed (the magnitude of the velocity).

Let $I=[a,b]$ and $s(t): [v,w] \rightarrow [a,b]$ such that $s$ is increasing and differentiable. Then $\vec{\phi} = \vec{r} \circ s$ will have have

$$~
\text{arc length} =
\int_v^w \| \vec{\phi}'(t)\| dt =
\int_v^w \| \vec{r}'(s(t))\| s'(t) dt =
\int_a^b \| \vec{r}'(u) \| du,
~$$

by a change of variable $u=s(t)$. As such the arc length is a property of the curve and not the parameterization of the curve.

For some parameterization, we can define

$$~
s(t) = \int_0^t \| \vec{r}'(u) \| du
~$$

Then by the fundamental theorem of calculus, $s(t)$ is non-decreasing. If $\vec{r}'$ is assumed to be non-zero and continuous (regular), then $s(t)$ has a derivative and an inverse which is monotonic. Using the inverse function $s^{-1}$ to change variables ($\vec{\phi} = \vec{r} \circ s^{-1}$) has

$$~
\int_0^c \| \phi'(t) \| dt =
\int_{s^{-1}(0)}^{s^{-1}(c)} \| \vec{r}'(u) \| du =
s(s^{-1}(c)) - s(s^{-1}(0)) =
c
~$$

That is, the arc length from $[0,c]$ for $\phi$ is just $c$; the curve $C$ is parameterized by arc length.


##### Example

Viviani's curve is the intersection of sphere of radius $a$ with a cylinder of radius $a$. A parameterization was given previously by:

````julia

function viviani(t, a=1)
    [a*(1-cos(t)), a*sin(t), 2a*sin(t/2)]
end
````


````
viviani (generic function with 2 methods)
````





The curve is traced out over the interval $[0, 4\pi]$. We try to find the arc-length:

````julia

@vars t a positive=true
speed = simplify(norm(diff.(viviani(t, a), t)))
integrate(speed, (t, 0, 4*PI))
````


````
Error: UndefVarError: simplify not defined
````





We see that the answer depends linearly on $a$, but otherwise is a constant expressed as an integral. We use `QuadGk` to provide a numeric answer for the case $a=1$:

````julia

using QuadGK
quadgk(t -> norm(viviani'(t)), 0, 4pi)
````


````
(15.280791156110851, 3.750801136348514e-8)
````






##### Example

Very few parameterized curves admit a closed-form expression for parameterization by arc-length. Let's consider the helix expressed by $\langle a\cos(t), a\sin(t), bt\rangle$, as this does allow such a parameterization.

````julia

@vars a b t al positive=true
helix = [a*cos(t), a*sin(t), b*t]
speed = simplify( norm(diff.(helix, t)) )
s = integrate(speed, (t, 0, al))
````


````
Error: UndefVarError: simplify not defined
````





So `s` is a linear function. We can re-parameterize by:

````julia

eqn = subs.(helix, t.=> al/sqrt(a^2 + b^2))
````


````
3-element Array{Sym,1}:
 a*cos(al/sqrt(a^2 + b^2))
 a*sin(al/sqrt(a^2 + b^2))
      al*b/sqrt(a^2 + b^2)
````





To see that the speed, $\| \vec{\phi}' \|$, is constantly $1$:

````julia

simplify(norm(diff.(eqn,al)))
````


````
Error: UndefVarError: simplify not defined
````





From this, we have the arc length is:

$$~
\int_0^t \| \vec{\phi}'(u) \| du = \int_0^t 1 du = t
~$$

----

Parameterizing by arc-length is only explicitly possible for a few examples, however knowing it can be done in theory, is important. Some formulas are simplified, such as the tangent, normal, and binormal. Let $\vec{r}(s)$ be parameterized by arc length, then:

$$~
\hat{T}(s)= \vec{r}'(s) / \| \vec{r}'(s) \| = \vec{r}'(s),\quad
\hat{N}(s) = \hat{T}'(s) / \| \hat{T}'(s)\| = \hat{T}'(s)/\kappa,\quad
\hat{B} = \hat{T} \times \hat{N},
~$$

As before, but further, we have if $\kappa$ is the curvature and $\tau$ the torsion, these relationships expressing the derivatives with respect to $s$ in terms of the components in the frame:

$$~
\begin{array}{}
\hat{T}'(s) &=                    &\kappa \hat{N}(s)  &\\
\hat{N}'(s) &= -\kappa \hat{T}(s) &                   &+ \tau \hat{B}(s)\\
\hat{B}'(s) &=                    &-\tau \hat{N}(s)   &
\end{array}
~$$

These are the [Frenet-Serret](https://en.wikipedia.org/wiki/Frenet%E2%80%93Serret_formulas) formulas.

##### Example

Continuing with our parameterization of a helix by arc length, we can compute the curvature and torsion by differentiation:

````julia

gamma = subs.(helix, t.=> al/sqrt(a^2 + b^2))   # gamma parameterized by arc length
@vars u positive=true
gamma = subs.(gamma, al .=> u)                  # u is arc-length parameterization
````


````
3-element Array{Sym,1}:
 a*cos(u/sqrt(a^2 + b^2))
 a*sin(u/sqrt(a^2 + b^2))
      b*u/sqrt(a^2 + b^2)
````



````julia

T = diff.(gamma, u)
norm(T)
````


````
_________________________________________________________
        ╱  2    2⎛     u      ⎞    2    2⎛     u      ⎞           
       ╱  a ⋅sin ⎜────────────⎟   a ⋅cos ⎜────────────⎟           
      ╱          ⎜   _________⎟          ⎜   _________⎟           
     ╱           ⎜  ╱  2    2 ⎟          ⎜  ╱  2    2 ⎟       2   
    ╱            ⎝╲╱  a  + b  ⎠          ⎝╲╱  a  + b  ⎠      b    
   ╱      ───────────────────── + ───────────────────── + ─────── 
  ╱               2    2                  2    2           2    2 
╲╱               a  + b                  a  + b           a  + b
````





The length is one, as the speed of a curve parameterized by arc-length is 1.

````julia

out = diff.(T, u)
````


````
3-element Array{Sym,1}:
 -a*cos(u/sqrt(a^2 + b^2))/(a^2 + b^2)
 -a*sin(u/sqrt(a^2 + b^2))/(a^2 + b^2)
                                     0
````





This should be $\kappa \hat{N}$, so we do:

````julia

kappa = norm(out)
Norm = out/kappa
kappa |> simplify
````


````
Error: UndefVarError: simplify not defined
````





Interpreting, $a$ is the radius of the circle and $b$ how tight the coils are. If $a$ gets much larger than $b$, then the curvature is like $1/a$, just as with a circle. If $b$ gets very big, then the trajectory looks more stretched out and the curvature gets smaller.


To find the torsion, we find, $\hat{B}$ then differentiate:

````julia

B = T × Norm
out = diff.(B, u)
````


````
3-element Array{Sym,1}:
 a*b*cos(u/sqrt(a^2 + b^2))/((a^2 + b^2)^2*sqrt(a^2*sin(u/sqrt(a^2 + b^2))^
2/(a^2 + b^2)^2 + a^2*cos(u/sqrt(a^2 + b^2))^2/(a^2 + b^2)^2))
 a*b*sin(u/sqrt(a^2 + b^2))/((a^2 + b^2)^2*sqrt(a^2*sin(u/sqrt(a^2 + b^2))^
2/(a^2 + b^2)^2 + a^2*cos(u/sqrt(a^2 + b^2))^2/(a^2 + b^2)^2))
                                                                           
                                                             0
````





This looks complicated, as does `Norm`:

````julia

Norm
````


````
3-element Array{Sym,1}:
 -a*cos(u/sqrt(a^2 + b^2))/((a^2 + b^2)*sqrt(a^2*sin(u/sqrt(a^2 + b^2))^2/(
a^2 + b^2)^2 + a^2*cos(u/sqrt(a^2 + b^2))^2/(a^2 + b^2)^2))
 -a*sin(u/sqrt(a^2 + b^2))/((a^2 + b^2)*sqrt(a^2*sin(u/sqrt(a^2 + b^2))^2/(
a^2 + b^2)^2 + a^2*cos(u/sqrt(a^2 + b^2))^2/(a^2 + b^2)^2))
                                                                           
                                                          0
````






However, the torsion, up to a sign, simplifies nicely:

````julia

norm(out) |> simplify
````


````
Error: UndefVarError: simplify not defined
````





Here, when $b$ gets large, the curve looks more and more "straight" and the torsion decreases. Similarly, if $a$ gets big, the torsion decreases.


##### Example

[Levi and Tabachnikov](https://projecteuclid.org/download/pdf_1/euclid.em/1259158427) consider the trajectories of the front and rear bicycle wheels. Recall the notation previously used: $\vec{F}(t)$ for the front wheel, and $\vec{B}(t)$ for the rear wheel trajectories. Consider now their parameterization by arc length, using $u$ for the arc-length parameter for $\vec{F}$ and $v$ for $\vec{B}$. We define $\alpha(u)$ to be the steering angle of the bicycle. This can be found as the angle between the tangent vector of the path of $\vec{F}$ with the vector $\vec{B} - \vec{F}$. Let $\kappa$ be the curvature of the front wheel and $k$ the curvature of the back wheel.

![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_62_1.png)



Levi and Tabachnikov prove in their Proposition 2.4:

$$~
\begin{align}
\kappa(u) &= \frac{d\alpha(u)}{du} + \frac{\sin(\alpha(u))}{a},\\
|\frac{du}{dv}| &= |\cos(\alpha)|, \quad \text{and}\\
k &= \frac{\tan(\alpha)}{a}.
\end{align}
~$$

The first equation relates the steering angle with the curvature. If the steering angle is not changed ($d\alpha/du=0$) then the curvature is constant and the motion is circular. It will be greater for larger angles (up to $\pi/2$). As the curvature is the reciprocal of the radius, this means the radius of the circular trajectory will be smaller. For the same constant steering angle, the curvature will be smaller for longer wheelbases, meaning the circular trajectory will have a larger radius. For cars, which have similar dynamics, this means longer wheelbase cars will take more room to make a U turn.

The second equation may be interpreted in ratio of arc lengths. The infinitesimal arc length of the rear wheel is proportional to that of the front wheel only scaled down by $\cos(\alpha)$. When $\alpha=0$ - the bike is moving in a straight line - and the two are the same. At the other extreme - when $\alpha=\pi/2$ - the bike must be pivoting on its rear wheel and the rear wheel has no arc length. This cosine, is related to the speed of the back wheel relative to the speed of the front wheel, which was used in the initial differential equation.

The last equation, relates the curvature of the back wheel track to the steering angle of the front wheel. When $\alpha=\pm\pi/2$, the rear-wheel curvature, $k$, is infinite, resulting in a cusp (no circle with non-zero radius will approximate the trajectory). This occurs when the front wheel is steered orthogonal to the direction of motion. As was seen in previous graphs of the trajectories, a cusp can happen for quite regular front wheel trajectories.


To derive the first one, we have previously noted that
when a curve is parameterized by arc length, the curvature is more directly computed: it is the magnitude of the derivative of the tangent vector.
The tangent vector is of unit length, when parametrized by arc length. This implies it's derivative will be orthogonal. If $\vec{r}(t)$ is a parameterization by arc length, then the curvature formula simplifies as:

$$~
\kappa(s) = \frac{\| \vec{r}'(s) \times \vec{r}''(s) \|}{\|\vec{r}'(s)\|^3} =
\frac{\| \vec{r}'(s) \times \vec{r}''(s) \|}{1} =
\| \vec{r}'(s) \| \| \vec{r}''(s) \| \sin(\theta) =
1 \| \vec{r}''(s) \| 1 = \| \vec{r}''(s) \|.
~$$


So in the above, the curvature is $\kappa = \| \vec{F}''(u) \|$ and $k = \|\vec{B}''(v)\|$.

On the figure, the tangent vector $\vec{F}'(u)$ is drawn, along with this unit vector rotated by $\pi/2$. We call these, for convenience, $\vec{U}$ and $\vec{V}$. We have $\vec{U} = \vec{F}'(u)$ and $\vec{V} = -(1/\kappa) \vec{F}''(u)$.


The key decomposition, is to express a unit vector in the direction of the line segment, as the vector $\vec{U}$ rotated by $\alpha$ degrees. Mathematically, this is usually expressed in matrix notation, but more explicitly by

$$~
\langle \cos(\alpha) \vec{U}_1 - \sin(\alpha) \vec{U}_2,
\sin(\alpha) \vec{U}_1 + \cos(\alpha) \vec{U}_2 =
\vec{U} \cos(\alpha) - \vec{V} \sin(\alpha).
~$$

With this, the mathematical relationship between $F$ and $B$ is just a multiple of this unit vector:

$$~
\vec{B}(u) = \vec{F}(u) - a \vec{U} \cos(\alpha) + a \vec{V} \sin(\alpha).
~$$

It must be that the tangent line of $\vec{B}$ is parallel to $\vec{U} \cos(\alpha) + \vec{V} \sin(\alpha)$. To utilize this, we differentiate $\vec{B}$ using the facts that $\vec{U}' = \kappa \vec{V}$ and $\vec{V}' = -\kappa \vec{U}$. These coming from $\vec{U} = \vec{F}'$ and so it's derivative in $u$ has magnitude yielding the curvature, $\kappa$, and direction orthogonal to $\vec{U}$.

$$~
\begin{align}
\vec{B}'(u) &= \vec{F}'(u)
-a \vec{U}' \cos(\alpha) -a \vec{U} (-\sin(\alpha)) \alpha'
+a \vec{V}' \sin(\alpha) + a \vec{V} \cos(\alpha) \alpha'\\
& =  \vec{U}
-a (\kappa) \vec{V} \cos(\alpha) + a \vec{U} \sin(\alpha) \alpha' +
a (-\kappa) \vec{U} \sin(\alpha) + a \vec{V} \cos(\alpha) \alpha' \\
&= \vec{U}
+ a(\alpha' - \kappa) \sin(\alpha) \vec{U}
+ a(\alpha' - \kappa) \cos(\alpha)\vec{V}.
\end{align}
~$$

Extend the 2-dimensional vectors to 3-d, by adding a zero $z$ component, then:

$$~
\begin{align}
\vec{0} &= (\vec{U}
+ a(\alpha' - \kappa) \sin(\alpha) \vec{U}
+ a(\alpha' - \kappa) \cos(\alpha)\vec{V}) \times
(-\vec{U} \cos(\alpha) +  \vec{V} \sin(\alpha)) \\
&= (\vec{U} \times \vec{V}) \sin(\alpha) +
a(\alpha' - \kappa) \sin(\alpha) \vec{U} \times  \vec{V} \sin(\alpha)) -
a(\alpha' - \kappa) \cos(\alpha)\vec{V} \times \vec{U} \cos(\alpha) \\
&= (\sin(\alpha) + a(\alpha'-\kappa) \sin^2(\alpha) +
a(\alpha'-\kappa) \cos^2(\alpha)) \vec{U} \times \vec{V} \\
&= (\sin(\alpha) + a (\alpha' - \kappa)) \vec{U} \times \vec{V}.
\end{align}
~$$

The terms $\vec{U} \times\vec{U}$ and $\vec{V}\times\vec{V}$ being $\vec{0}$, due to properties of the cross product. This says the scalar part must be $0$, or

$$~
\frac{\sin(\alpha)}{a} + \alpha' = \kappa.
~$$

As for the second equation,
from the expression for $\vec{B}'(u)$, after setting $a(\alpha'-\kappa) = -\sin(\alpha)$:

$$~
\begin{align}
\|\vec{B}'(u)\|^2
&= \| (1 -\sin(\alpha)\sin(\alpha)) \vec{U} -\sin(\alpha)\cos(\alpha) \vec{V} \|^2\\
&= \| \cos^2(\alpha) \vec{U} -\sin(\alpha)\cos(\alpha) \vec{V} \|^2\\
&= (\cos^2(\alpha))^2 + (\sin(\alpha)\cos(\alpha))^2\quad\text{using } \vec{U}\cdot\vec{V}=0\\
&= \cos^2(\alpha)(\cos^2(\alpha) + \sin^2(\alpha))\\
&= \cos^2(\alpha).
\end{align}
~$$

From this $\|\vec{B}(u)\| = |\cos(\alpha)\|$. But $1 = \|d\vec{B}/dv\| = \|d\vec{B}/du \| \cdot |du/dv|$ and $|dv/du|=|\cos(\alpha)|$ follows.





##### Example: evolutes and involutes

Following [Fuchs](https://doi.org/10.4169/amer.math.monthly.120.03.217) we discuss a geometric phenomenon known and explored by Huygens, and likely earlier. We stick to the two-dimensional case, Fuchs extends this to three dimensions. The following figure

![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_64_1.png)



is that of an ellipse with many *normal* lines drawn to it. The normal lines appear to intersect in a somewhat diamond-shaped curve. This curve is the evolute of the ellipse. We can characterize this using the language of planar curves.

Consider a parameterization of a curve by arc-length, $\vec\gamma(s) = \langle u(s), v(s) \rangle$. The unit *tangent* to this curve is $\vec\gamma'(s) = \hat{T}(s) = \langle u'(s), v'(s) \rangle$ and by simple geometry the unit *normal* will be $\hat{N}(s) = \langle -v'(s), u'(s) \rangle$. At a time $t$, a line through the curve parameterized by $\vec\gamma$ is given by $l_t(a) = \vec\gamma(t) + a \hat{N}(t)$.

Consider two nearby points $t$ and $t+\epsilon$ and the intersection of $l_t$ and $l_{t+\epsilon}$. That is, we need points $a$ and $b$ with: $l_t(a) = l_{t+\epsilon}(b)$. Setting the components equal, this is:

$$~
\begin{align}
u(t) - av'(t) &= u(t+\epsilon) - bv'(t+\epsilon) \\
v(t) - au'(t) &= v(t+\epsilon) - bu'(t+\epsilon).
\end{align}
~$$

This is a linear equation in two unknowns ($a$ and $b$) which can be solved. Here is the value for `a`:

````julia

u, v = SymFunction("u, v")
@vars t epsilon w
@vars a b
γ(t) = [u(t),v(t)]
n(t) = subs.(diff.([-v(w), u(w)], w), w.=>t)
l(a, t) = γ(t) + a * n(t)
out = SymPy.solve(l(a, t) - l(b, t+epsilon), [a,b])
out[a]
````


````
⎛                  ⎛d       ⎞│                            ⎛d       ⎞│     
  ⎞
-⎜(u(t) - u(ε + t))⋅⎜──(u(w))⎟│        + (v(t) - v(ε + t))⋅⎜──(v(w))⎟│     
  ⎟
 ⎝                  ⎝dw      ⎠│w=ε + t                     ⎝dw      ⎠│w=ε +
 t⎠
───────────────────────────────────────────────────────────────────────────
───
           d        ⎛d       ⎞│          d        ⎛d       ⎞│              
   
           ──(u(t))⋅⎜──(v(w))⎟│        - ──(v(t))⋅⎜──(u(w))⎟│              
   
           dt       ⎝dw      ⎠│w=ε + t   dt       ⎝dw      ⎠│w=ε + t       
   

 
 
 
─
````





Letting $\epsilon \rightarrow 0$ we get an expression for $a$ that will describe the evolute at time $t$ in terms of the function $\gamma$. Looking at the expression above, we can see that dividing the *numerator* by $\epsilon$ and taking a limit will yield $u'(t)^2 + v'(t)^2$. If the *denominator* has a limit after dividing by $\epsilon$, then we can find the description sought. Pursuing this leads to:

$$~
\frac{u'(t) v'(t+\epsilon) - v'(t) u'(t+\epsilon)}{\epsilon} =
\frac{u'(t) v'(t+\epsilon) -u'(t)v'(t) + u'(t)v'(t)- v'(t) u'(t+\epsilon)}{\epsilon} =
\frac{u'(t)(v'(t+\epsilon) -v'(t))}{\epsilon} + \frac{(u'(t)- u'(t+\epsilon))v'(t)}{\epsilon},
~$$

which in the limit will give $u'(t)v''(t) - u''(t) v'(t)$. All told, in the limit as $\epsilon \rightarrow 0$ we get

$$~
a = \frac{u'(t)^2 + v'(t)^2}{u'(t)v''(t) - v'(t) u''(t)} = 1/(\|\vec\gamma'\|\kappa) = 1/(\|\hat{T}\|\kappa) = 1/\kappa,
~$$

with $\kappa$ being the curvature of the planar curve. That is, the evolute of $\vec\gamma$ is described by:

$$~
\vec\beta(s) = \vec\gamma(s) + \frac{1}{\kappa(s)}\hat{N}(s).
~$$

Revisualizing:

````julia

unit_vec(x) = x/norm(x)

r(t) = [2cos(t), sin(t), 0]
Tangent(t) = unit_vec(r'(t))
Normal(t) = unit_vec(Tangent'(t))
curvature(t) = norm(r'(t) × r''(t) ) / norm(r'(t))^3

plot_parametric_curve(t -> r(t)[1:2], 0, 2pi, legend=false, aspect_ratio=:equal)
plot_parametric_curve!(t -> (r(t) + Normal(t)/curvature(t))[1:2], 0, 2pi)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_66_1.png)



We computed the above illustration using $3$ dimensions (hence the use of `[1:2]...`) as the curvature formula is easier to express. Recall, the curvature also appears in the [Frenet-Serret](https://en.wikipedia.org/wiki/Frenet%E2%80%93Serret_formulas) formulas: $d\hat{T}/ds = \kappa \hat{N}$ and $d\hat{N}/ds = -\kappa \hat{T}+ \tau \hat{B}$. In a planar curve, as under consideration, the binormal is $\vec{0}$. This allow the computation of $\vec\beta(s)'$:

$$~
\begin{align}
\vec{\beta}' &= \frac{d(\vec\gamma + (1/k) \hat{N})}{dt}\\
&= \hat{T} + (-\frac{k'}{k^2}\hat{N} + \frac{1}{k} \hat{N}')\\
&= \hat{T} - \frac{k'}{k^2}\hat{N} + \frac{1}{k} (-\kappa \hat{T})\\
&= - \frac{k'}{k^2}\hat{N}.
\end{align}
~$$

We see $\vec\beta'$ is zero (the curve is non-regular) when $\kappa'(s) = 0$. The curvature changes from increasing to decreasing, or vice versa at each of the 4 crossings of the major and minor axes - there are 4 non-regular points, and we see $4$ cusps in the evolute.


The curve parameterized by $\vec{r}(t) = 2(1 - \cos(t)) \langle \cos(t), \sin(t)\rangle$ over $[0,2\pi]$ is cardiod. It is formed by rolling a circle of radius $r$ around another similar sized circle. The following graphically shows the evolute is a smaller cardiod (one-third the size). For fun, the evolute of the evolute is drawn:

````julia

r(t) = 2*(1 - cos(t)) * [cos(t), sin(t), 0]

function evolute(r)
    Tangent(t) = unit_vec(r'(t))
    Normal(t) = unit_vec(Tangent'(t))
    curvature(t) = norm(r'(t) × r''(t) ) / norm(r'(t))^3
    t -> r(t) + 1/curvature(t)*Normal(t)
end

plot_parametric_curve(t -> r(t)[1:2], 0, 2pi, legend=false, aspect_ratio=:equal)
plot_parametric_curve!(t -> evolute(r)(t)[1:2], 0, 2pi...)
plot_parametric_curve!(t -> ((evolute∘evolute)(r)(t))[1:2], 0, 2pi)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_67_1.png)



----

If $\vec\beta$ is *the* **evolute** of $\vec\gamma$, then $\vec\gamma$ is *an* **involute** of $\beta$. For a given curve, there is a parameterized family of involutes. While this definition has a pleasing self-referentialness, it doesn't have an immediately clear geometric interpretation. For that, consider the image of a string of fixed length $a$ attached to the curve $\vec\gamma$ at some point $t_0$. As this curve wraps around the curve traced by $\vec\gamma$ it is held taut so that it makes a tangent at the point of contact. The end of the string will trace out a curve and this is the trace of an *involute*.

````julia

using QuadGK

r(t) = [t, cosh(t)]
t0, t1 = -2, 0
a = t1

Tangent(t) = unit_vec(r'(t))
beta(t) = r(t) - Tangent(t) * quadgk(t -> norm(r'(t)), a, t)[1]

p = plot_parametric_curve(r, -2, 2, legend=false)
plot_parametric_curve!(beta, t0, t1)
for t in range(t0,-0.2, length=4)
    arrow!(r(t), -Tangent(t) * quadgk(t -> norm(r'(t)), a, t)[1])
    scatter!(unzip([r(t)])...)
end
p
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_68_1.png)





This lends itself to this mathematical description, if $\vec\gamma(t)$ parameterizes the planar curve, then an involute for $\vec\gamma(t)$ is described by:

$$~
\vec\beta(t) = \vec\gamma(t) + \left(a - \int_{t_0}^t \| \vec\gamma'(t)\| dt) \hat{T}(t)\right),
~$$

where $\hat{T}(t) = \vec\gamma'(t)/\|\vec\gamma'(t)\|$ is the unit tangent vector. The above uses two parameters ($a$ and $t_0$), but only one is needed, as there is an obvious redundancy (a point can *also* be expressed by $t$ and the shortened length of string). [Wikipedia](https://en.wikipedia.org/wiki/Involute) uses this definition for $a$ and $t$ values in an interval $[t_0, t_1]$:

$$~
\vec\beta_a(t) = \vec\gamma(t) - \frac{\vec\gamma'(t)}{\|\vec\gamma'(t)\|}\int_a^t \|\vec\gamma'(t)\| dt.
~$$

If $\vec\gamma(s)$ is parameterized by arc length, then this simplifies quite a bit, as the unit tangent is just $\vec\gamma'(s)$ and the remaining arc length just $(s-a)$:

$$~
\vec\beta_a(s) = \vec\gamma(s) - \vec\gamma'(s) (s-a)
=\vec\gamma(s) - \hat{T}_{\vec\gamma}(s)(s-a).\quad\text{($s$ is the arc-length parameter)}
~$$

With this characterization, we see several properties:

* From $\vec\beta_a'(s) = \hat{T}(s) - (\kappa(s) \hat{N}(s) (s-a) + \hat{T}(s)) = -\kappa_{\vec\gamma}(s) \cdot (s-a) \cdot \hat{N}_{\vec\gamma}(s)$,  the involute is *not* regular at $s=a$, as its derivative is zero.

* As $\vec\beta_a(s) = \vec\beta_0(s) + a\hat{T}(s)$, the family of curves is parallel.

* The evolute of $\vec\beta_a(s)$, $s$ the arc-length parameter of $\vec\gamma$, can be shown to be $\vec\gamma$. This requires more work:

The evolute for $\vec\beta_a(s)$ is:

$$~
\vec\beta_a(s) + \frac{1}{\kappa_{\vec\beta_a}(s)}\hat{N}_{\vec\beta_a}(s).
~$$

In the following we show that:

$$~
\begin{align}
\kappa_{\vec\beta_a}(s) &= 1/(s-a),\\
\hat{N}_{\vec\beta_a}(s) &= \hat{T}_{\vec\beta_a}'(s)/\|\hat{T}_{\vec\beta_a}'(s)\| = -\hat{T}_{\vec\gamma}(s).
\end{align}
~$$

The first shows in a different way that when $s=a$ the curve is not regular, as the curvature fails to exists. In the above figure, when the involute touches $\vec\gamma$, there will be a cusp.

With these two identifications and using $\vec\gamma'(s) = \hat{T}_{\vec\gamma(s)}$, we have the evolute simplifies to

$$~
\vec\beta_a(s) + \frac{1}{\kappa_{\vec\beta_a}(s)}\hat{N}_{\vec\beta_a}(s) =
\vec\gamma(s) + \vec\gamma'(s)(s-a)  + \frac{1}{\kappa_{\vec\beta_a}(s)}\hat{N}_{\vec\beta_a}(s) =
\vec\gamma(s) + \hat{T}_{\vec\gamma}(s)(s-a) + \frac{1}{1/(s-a)} (-\hat{T}_{\vec\gamma}(s)) = \vec\gamma(s).
~$$

That is the evolute an an involute of $\vec\gamma(s)$ is $\vec\gamma(s)$.


We have:

$$~
\begin{align}
\beta_a(s) &= \vec\gamma - \vec\gamma'(s)(s-a)\\
\beta_a'(s) &= -\kappa_{\vec\gamma}(s)(s-a)\hat{N}_{\vec\gamma}(s)\\
\beta_a''(s) &= (-\kappa_{\vec\gamma}(s)(s-a))' \hat{N}_{\vec\gamma}(s) + (-\kappa_{\vec\gamma}(s)(s-a))(-\kappa_{\vec\gamma}\hat{T}_{\vec\gamma}(s)),
\end{align}
~$$

the last line by the Frenet-Serret for formula *planar* curves which show $\hat{T}'(s) = \kappa(s) \hat{N}$ and $\hat{N}'(s) = -\kappa(s)\hat{T}(s)$.

To compute the curvature of $\vec\beta_a$, we need to compute both:

$$~
\begin{align}
\| \vec\beta' \|^3 &= |\kappa^3 (s-a)^3|\\
\| \vec\beta' \times \vec\beta'' \| &= |\kappa(s)^3 (s-a)^2|,
\end{align}
~$$

the last line using both $\hat{N}\times\hat{N} = \vec{0}$ and $\|\hat{N}\times\hat{T}\| = 1$. The curvature then is $\kappa_{\vec\beta_a}(s) = 1/(s-a)$.

Using the formula for $\vec\beta'$ above, we get $\hat{T}_\beta(s)=\hat{N}_{\vec\gamma}(s)$ so $\hat{T}_\beta(s)' = -\kappa_{\vec\gamma}(s) \hat{T}_{\vec\gamma}(s)$ with unit vector just $\hat{N}_{\vec\beta_a} = -\hat{T}_{\vec\gamma}(s)$.


----

Show that *an* involute of the cycloid $\vec{r}(t) = \langle t - \sin(t), 1 - \cos(t) \rangle$ is also a cycloid. We do so graphically:

````julia

r(t) = [t - sin(t), 1 - cos(t)]
## find *involute*: r - r'/|r'| * int(|r'|, a, t)
t0, t1, a = 2PI, PI, PI
@vars t real=true
rp = diff.(r(t), t)
speed = 2sin(t/2)

ex = r(t) - rp/speed * integrate(speed, a, t)

plot_parametric_curve(r, 0, 4pi, legend=false)
plot_parametric_curve!(u -> SymPy.N.(subs.(ex, t .=> u)), 0, 4pi)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_69_1.png)




The expression `ex` is secretly `[t + sin(t), 3 + cos(t)]`, another cycloid.





## Questions

###### Question

A cycloid is formed by pushing a wheel on a surface without slipping. The position of a fixed point on the outer rim of the wheel traces out the cycloid. Suppose the wheel has radius $R$ and the initial position of the point is at the bottom, $(0,0)$. Let $t$ measure angle measurement, in radians. Then the point of contact of the wheel will be at $Rt$, as that is the distance the wheel will have rotated. That is, the hub of the wheel will move according to $\langle Rt,~ R\rangle$. Relative to the hub, the point on the rim will have coordinates $\langle -R\sin(t), -R\cos(t) \rangle$, so the superposition gives:

$$~
\vec{r}(t) = \langle Rt - R\sin(t), R - R\cos(t) \rangle.
~$$

What is the position at $t=\pi/4$?

````
CalculusWithJulia.WeaveSupport.Radioq(["`[0.181172, 0.5]`", "`[0.0782914, 0
.292893 ]`", "`[0.570796, 1.0]`"], 2, "", nothing, [1, 2, 3], ["`[0.181172,
 0.5]`", "`[0.0782914, 0.292893 ]`", "`[0.570796, 1.0]`"], "", false)
````





And the position at $\pi/2$?


````
CalculusWithJulia.WeaveSupport.Radioq(["`[0.570796, 1.0]`", "`[0.181172, 0.
5]`", "`[0.0782914, 0.292893 ]`"], 1, "", nothing, [1, 2, 3], ["`[0.570796,
 1.0]`", "`[0.181172, 0.5]`", "`[0.0782914, 0.292893 ]`"], "", false)
````





###### Question

Suppose instead of keeping track of a point on the outer rim of the wheel, a point a distance $r < R$ from the hub is chosen in the above description of a cycloid (a [Curtate](http://mathworld.wolfram.com/CurtateCycloid.html) cycloid). If we start at $\langle 0,~ R-r \rangle$, what will be the position at $t$?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$\langle R
t - r\sin(t),~ R - r\cos(t) \rangle$", L"$\langle Rt - R\sin(t),~ R - R\cos
(t) \rangle$", L"$\langle -r\sin(t),~ -r\cos(t) \rangle$"], 1, "", nothing,
 [1, 2, 3], LaTeXStrings.LaTeXString[L"$\langle Rt - r\sin(t),~ R - r\cos(t
) \rangle$", L"$\langle Rt - R\sin(t),~ R - R\cos(t) \rangle$", L"$\langle 
-r\sin(t),~ -r\cos(t) \rangle$"], "", false)
````





###### Question

For the  cycloid $\vec{r}(t) = \langle t - \sin(t),~ 1 - \cos(t) \rangle$, find a simplified expression for $\| \vec{r}'(t)\|$.

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$1$", L"$\
sqrt{2 - 2\cos(t)}$", L"$1 + \cos(t) + \cos(2t)$", L"$1 - \cos(t)$"], 2, ""
, nothing, [1, 2, 3, 4], LaTeXStrings.LaTeXString[L"$1$", L"$\sqrt{2 - 2\co
s(t)}$", L"$1 + \cos(t) + \cos(2t)$", L"$1 - \cos(t)$"], "", false)
````





###### Question

The cycloid $\vec{r}(t) = \langle t - \sin(t),~ 1 - \cos(t) \rangle$ has a formula for the arc length from $0$ to $t$ given by: $l(t) = 4 - 4\cos(t/2)$.

Plot the following two equations over $[0,8]$ which are a reparameterization of the cycloid by $l^{-1}(t)$.

````julia

γ(s) = 2 * acos(1-s/4)
x1(s) = γ(s) - sin(γ(s))
y1(s) = 1 - cos(γ(s))
````


````
y1 (generic function with 1 method)
````





How many arches of the cycloid are traced out?

````
CalculusWithJulia.WeaveSupport.Radioq([1, 2, 3], 1, "", nothing, [1, 2, 3],
 [1, 2, 3], "", false)
````





###### Question

Consider the cycloid  $\vec{r}(t) = \langle t - \sin(t),~ 1 - \cos(t) \rangle$

What is the derivative at $t=\pi/2$?

````
CalculusWithJulia.WeaveSupport.Radioq(["`[0,0]`", "`[1,1]`", "`[2,0]`"], 2,
 "", nothing, [1, 2, 3], ["`[0,0]`", "`[1,1]`", "`[2,0]`"], "", false)
````





What is the derivative at $t=\pi$?


````
CalculusWithJulia.WeaveSupport.Radioq(["`[2,0]`", "`[1,1]`", "`[0,0]`"], 1,
 "", nothing, [1, 2, 3], ["`[2,0]`", "`[1,1]`", "`[0,0]`"], "", false)
````





###### Question

Consider the circle $\vec{r}(t) = R \langle \cos(t),~ \sin(t) \rangle$, $R > 0$. Find the norm of $\vec{r}'(t)$:

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$1$", L"$1
/R$", L"$R$", L"$R^2$"], 3, "", nothing, [1, 2, 3, 4], LaTeXStrings.LaTeXSt
ring[L"$1$", L"$1/R$", L"$R$", L"$R^2$"], "", false)
````






###### Question

The curve described by $\vec{r}(t) = \langle 10t,~ 10t - 16t^2\rangle$ models the flight of an arrow. Compute the length traveled from when it is launched to when it returns to the ground.

````
CalculusWithJulia.WeaveSupport.Numericq(7.173709841851994, 0.001, "", "[7.1
7271, 7.17471]", 7.172709841851994, 7.174709841851994, "", "")
````






##### Question

Let $\vec{r}(t) = \langle t, t^2 \rangle$ describe a parabola. What is the arc length between $0 \leq t \leq 1$? First, what is a formula for the speed ($\| \vec{r}'(t)\|$)?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$1 + 4t^2$
", L"$t + t^2$", L"$\sqrt{1 + 4t^2}$", L"$1$"], 3, "", nothing, [1, 2, 3, 4
], LaTeXStrings.LaTeXString[L"$1 + 4t^2$", L"$t + t^2$", L"$\sqrt{1 + 4t^2}
$", L"$1$"], "", false)
````





Numerically find the arc length.

````
CalculusWithJulia.WeaveSupport.Numericq(1.4789428575446002, 0.001, "", "[1.
47794, 1.47994]", 1.4779428575446003, 1.4799428575446, "", "")
````








##### Question

Let $\vec{r}(t) = \langle t, t^2 \rangle$ describe a parabola. What is the curvature of $\vec{r}(t)$ at $t=0$?

````
CalculusWithJulia.WeaveSupport.Numericq(2, 0, "", "[2.0, 2.0]", 2, 2, "", "
")
````





The curvature at $1$ will be

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"less than 
the curvature at $t=0$", L"the same as the curvature at $t=0$", L"greater t
han the curvature at $t=0$"], 1, "", nothing, [1, 2, 3], LaTeXStrings.LaTeX
String[L"less than the curvature at $t=0$", L"the same as the curvature at 
$t=0$", L"greater than the curvature at $t=0$"], "", false)
````





The curvature as $t\rightarrow \infty$ will be

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$0$", L"$1
$", L"$\infty$"], 1, "", nothing, [1, 2, 3], LaTeXStrings.LaTeXString[L"$0$
", L"$1$", L"$\infty$"], "", false)
````





----

Now, if we have a more general parabola by introducing a parameter $a>0$: $\vec{r}(t) = \langle t, a\cdot t^2 \rangle$, What is the curvature of $\vec{r}(t)$ at $t=0$?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$2a$", L"$
1$", L"$2/a$", L"$2$"], 1, "", nothing, [1, 2, 3, 4], LaTeXStrings.LaTeXStr
ing[L"$2a$", L"$1$", L"$2/a$", L"$2$"], "", false)
````





###### Question

Projectile motion with constant acceleration is expressed parametrically by $\vec{x}(t) = \vec{x}_0 + \vec{v}_0 t + (1/2) \vec{a} t^2$, where $\vec{x}_0$ and $\vec{v}_0$ are initial positions and velocity respectively. In [Strang](https://ocw.mit.edu/resources/res-18-001-calculus-online-textbook-spring-2005/textbook/MITRES_18_001_strang_12.pdf) p451, we find an example utilizing this formula to study the curve of a baseball. Place the pitcher at the origin, the batter along the $x$ axis, then a baseball thrown with spin around its $z$ axis will have acceleration in the $y$ direction in addition to the acceration due to gravity in the $z$ direction. Suppose the ball starts 5 feet above the ground when pitched ($\vec{x}_0 = \langle 0,0, 5\rangle$), and has initial velocity $\vec{v}_0 = \langle 120, -2, 2 \rangle$. (120 feet per second is about 80 miles per hour). Suppose the pitcher can produce an acceleration in the $y$ direction of $16ft/sec^2$, then $\vec{a} = \langle 0, 16, -32\rangle$ in these units. (Gravity is $9.8m/s^2$ or $32ft/s^2$.)


The plate is 60 feet away. How long will it take for the ball to reach the batter? (When the first component is $60$?)

````
CalculusWithJulia.WeaveSupport.Numericq(0.5, 0.001, "", "[0.499, 0.501]", 0
.499, 0.501, "", "")
````





At $t=1/4$ the ball is half-way to home. If the batter reads the ball at this point, where in the $y$ direction is the ball?

````
CalculusWithJulia.WeaveSupport.Numericq(0.0, 0.001, "", "[-0.001, 0.001]", 
-0.001, 0.001, "", "")
````





At $t=1/2$ has the ball moved more than 1/2 foot in the $y$ direction?

````
CalculusWithJulia.WeaveSupport.Radioq(["Yes", "No"], 1, "", nothing, [1, 2]
, ["Yes", "No"], "", false)
````






###### Question

In [Strang](https://ocw.mit.edu/resources/res-18-001-calculus-online-textbook-spring-2005/textbook/MITRES_18_001_strang_12.pdf) we see this picture describing  a curve:

![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_89_1.png)



Strang notes that the curve is called the "witch of Agnesi" after Maria Agnesi, the author of the first three-semester calculus book. (L'Hopital's book did not contain integration.)


We wish to identify the parameterization. Using $\theta$ an angle in standard position, we can see that the component functions $x(\theta)$ and $y(\theta)$ may be found using trigonometric analysis.

What is the $x$ coordinate of point $A$? (Also the $x$ coordinate of $P$.)

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$2\tan(\th
eta)$", L"$\cot(\theta)$", L"$2\cot(\theta)$", L"$\tan(\theta)$"], 3, "", n
othing, [1, 2, 3, 4], LaTeXStrings.LaTeXString[L"$2\tan(\theta)$", L"$\cot(
\theta)$", L"$2\cot(\theta)$", L"$\tan(\theta)$"], "", false)
````





Using the polar form of a circle, the length between the origin and $B$ is given by $2\cos(\theta-\pi/2) = 2\sin(\theta)$. Using this, what is the $y$ coordinate of $B$?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$2\sin(\th
eta)$", L"$2\sin^2(\theta)$", L"$\sin(\theta)$", L"$2$"], 2, "", nothing, [
1, 2, 3, 4], LaTeXStrings.LaTeXString[L"$2\sin(\theta)$", L"$2\sin^2(\theta
)$", L"$\sin(\theta)$", L"$2$"], "", false)
````






###### Question

Let $n > 0$, $\vec{r}(t) = \langle t^(n+1),t^n\rangle$. Find the speed, $\|\vec{r}'(t)\|$.

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$\frac{\sq
rt{n^{2} t^{2 n} + t^{2 n + 2} \left(n + 1\right)^{2}}}{t}$", L"$\sqrt{n^2 
+ t^2}$", L"$t^n + t^{n+1}$"], 1, "", nothing, [1, 2, 3], LaTeXStrings.LaTe
XString[L"$\frac{\sqrt{n^{2} t^{2 n} + t^{2 n + 2} \left(n + 1\right)^{2}}}
{t}$", L"$\sqrt{n^2 + t^2}$", L"$t^n + t^{n+1}$"], "", false)
````





For $n=2$, the arc length of $\vec{r}$ can be found exactly. What is the arc-length between $0 \leq t \leq a$?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$\frac{a^{
2} \sqrt{9 a^{2} + 4}}{3} + \frac{4 \sqrt{9 a^{2} + 4}}{27} - \frac{8}{27}$
", L"$\frac{2 a^{\frac{5}{2}}}{5}$", L"$\sqrt{a^2 + 4}$"], 1, "", nothing, 
[1, 2, 3], LaTeXStrings.LaTeXString[L"$\frac{a^{2} \sqrt{9 a^{2} + 4}}{3} +
 \frac{4 \sqrt{9 a^{2} + 4}}{27} - \frac{8}{27}$", L"$\frac{2 a^{\frac{5}{2
}}}{5}$", L"$\sqrt{a^2 + 4}$"], "", false)
````






###### Question

The [astroid](http://www-history.mcs.st-and.ac.uk/Curves/Astroid.html) is one of the few curves with an exactly computable arc-length. The curve is parametrized by $\vec{r}(t) = \langle a\cos^3(t), a\sin^3(t)\rangle$. For $a=1$ find the arc-length between $0 \leq t \leq \pi/2$.

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$\sqrt{2}$
", L"$3/2$", L"$\pi/2$", L"$2$"], 2, "", nothing, [1, 2, 3, 4], LaTeXString
s.LaTeXString[L"$\sqrt{2}$", L"$3/2$", L"$\pi/2$", L"$2$"], "", false)
````






###### Question


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_95_1.png)




Let $F$ and $B$ be pictured above. Which is the red curve?

````
CalculusWithJulia.WeaveSupport.Radioq(["The front wheel", "The back wheel"]
, 1, "", nothing, [1, 2], ["The front wheel", "The back wheel"], "", false)
````







###### Question


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/vector_valued_functions_97_1.png)




Let $F$ and $B$ be pictured above. Which is the red curve?

````
CalculusWithJulia.WeaveSupport.Radioq(["The front wheel", "The back wheel"]
, 2, "", nothing, [1, 2], ["The front wheel", "The back wheel"], "", false)
````





###### Question

Let $\vec{\gamma}(s)$ be a parameterization of a curve by arc length and $s(t)$ some continuous increasing function of $t$. Then $\vec{\gamma} \circ s$ also parameterizes the curve. We have

$$~
\text{velocity}  = \frac{d (\vec{\gamma} \circ s)}{dt} = \frac{d\vec{\gamma}}{ds} \frac{ds}{dt} = \hat{T} \frac{ds}{dt}.
~$$

Continuing with a second derivative

$$~
\text{acceleration} = \frac{d^2(\vec{\gamma}\circ s)}{dt^2} =
\frac{d\hat{T}}{ds} \frac{ds}{dt} \frac{ds}{dt} + \hat{T} \frac{d^2s}{dt^2} = \frac{d^2s}{dt^2}\hat{T} + \kappa (\frac{ds}{dt})^2 \hat{N},
~$$

Using $ d\hat{T}{ds} = \kappa\hat{N}$ when parameterized by arc length.

This expresses the acceleration in term of the tangential part and the normal part. [Strang](https://ocw.mit.edu/resources/res-18-001-calculus-online-textbook-spring-2005/textbook/MITRES_18_001_strang_12.pdf) views this in terms of driving where the car motion is determined by the gas pedal and the brake pedal only giving acceleration in the $\hat{T}$ direction) and the steering wheel (giving acceleration in the $\hat{N}$ direction).


If a car is on a straight road, the $\kappa=0$. Is the acceleration along the $\hat{T}$ direction of the $\hat{N}$ direction?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"The $\hat{
T}$ direction", L"The $\hat{N}$ direction"], 1, "", nothing, [1, 2], LaTeXS
trings.LaTeXString[L"The $\hat{T}$ direction", L"The $\hat{N}$ direction"],
 "", false)
````





Suppose no gas or break is applied for a duration of time. The tangential acceleration will be $0$. During this time, which of these must  be $0$?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$\vec{\gam
ma} \circ s$", L"$ds/dt$", L"$d^2s/dt^2$"], 3, "", nothing, [1, 2, 3], LaTe
XStrings.LaTeXString[L"$\vec{\gamma} \circ s$", L"$ds/dt$", L"$d^2s/dt^2$"]
, "", false)
````





In going around a corner (with non-zero curvature), which is true?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"The accele
ration in the normal direction depends only on the speed ($ds/dt$) and not 
the curvature", L"The acceleration in the normal direction depends only on 
the curvature and not the speed ($ds/dt$)", L"The acceleration in the norma
l direction depends on both the curvature and the speed ($ds/dt$)"], 3, "",
 nothing, [1, 2, 3], LaTeXStrings.LaTeXString[L"The acceleration in the nor
mal direction depends only on the speed ($ds/dt$) and not the curvature", L
"The acceleration in the normal direction depends only on the curvature and
 not the speed ($ds/dt$)", L"The acceleration in the normal direction depen
ds on both the curvature and the speed ($ds/dt$)"], "", false)
````





###### Question

The evolute comes from the formula $\vec\gamma(T) - (1/\kappa(t)) \hat{N}(t)$. For hand computation, this formula can be explicitly given by two components $\langle X(t), Y(t) \rangle$ through:

$$~
\begin{align}
r(t) &= x'(t)^2 + y'(t)^2\\
k(t) &= x'(t)y''(t) - x''(t) y'(t)\\
X(t) &= x(t) - y'(t) r(t)/k(t)\\
Y(t) &= x(t) + x'(t) r(t)/k(t)
\end{align}
~$$

Let $\vec\gamma(t) = \langle t, t^2 \rangle = \langle x(t), y(t)\rangle$ be a parameterization of a parabola.

* Compute $r(t)$

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$1 - 4t^2$
", L"$1 - 2t$", L"$1 + 2t$", L"$1 + 4t^2$"], 4, "", nothing, [1, 2, 3, 4], 
LaTeXStrings.LaTeXString[L"$1 - 4t^2$", L"$1 - 2t$", L"$1 + 2t$", L"$1 + 4t
^2$"], "", false)
````





* Compute $k(t)$

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$2$", L"$-
8t$", L"$8t$", L"$-2$"], 1, "", nothing, [1, 2, 3, 4], LaTeXStrings.LaTeXSt
ring[L"$2$", L"$-8t$", L"$8t$", L"$-2$"], "", false)
````





* Compute $X(t)$

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$t - 4t(1+
2t)/2$", L"$t - 2(8t)/(1-2t)$", L"$t - 2t(1 + 4t^2)/2$", L"$t - 1(1+4t^2)/2
$"], 3, "", nothing, [1, 2, 3, 4], LaTeXStrings.LaTeXString[L"$t - 4t(1+2t)
/2$", L"$t - 2(8t)/(1-2t)$", L"$t - 2t(1 + 4t^2)/2$", L"$t - 1(1+4t^2)/2$"]
, "", false)
````





* Compute $Y(t)$

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$t^2 + 1(1
 + 4t^2)/2$", L"$t^2 - 2t(1+4t^2)/2$", L"$t^2 + 2t(1+4t^2)/2$", L"$t^2 - 1(
1+4t^2)/2$"], 1, "", nothing, [1, 2, 3, 4], LaTeXStrings.LaTeXString[L"$t^2
 + 1(1 + 4t^2)/2$", L"$t^2 - 2t(1+4t^2)/2$", L"$t^2 + 2t(1+4t^2)/2$", L"$t^
2 - 1(1+4t^2)/2$"], "", false)
````





###### Question

The following will compute the evolute of an ellipse:

````julia

@vars t a b
x = a*cos(t)
y = b*sin(t)
xp, xpp, yp, ypp = diff(x, t), diff(x,t,t), diff(y,t), diff(y,t,t)
r2 = xp^2 + yp^2
k = xp*ypp - xpp*yp
X = x - yp*r2/k     |> simplify
Y = y + xp * r2 / k |> simplify
[X,Y]
````




What is the resulting curve?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"An ellipse
 of the form $\langle a\cos(t), b\sin(t)$", L"An cubic parabola of the form
 $\langle ct^3, dt^2\rangle$", L"A cyloid of the form $c\langle t + \sin(t)
, 1 - \cos(t)\rangle$", L"An astroid of the form $c \langle \cos^3(t), \sin
^3(t) \rangle$"], 4, "", nothing, [1, 2, 3, 4], LaTeXStrings.LaTeXString[L"
An ellipse of the form $\langle a\cos(t), b\sin(t)$", L"An cubic parabola o
f the form $\langle ct^3, dt^2\rangle$", L"A cyloid of the form $c\langle t
 + \sin(t), 1 - \cos(t)\rangle$", L"An astroid of the form $c \langle \cos^
3(t), \sin^3(t) \rangle$"], "", false)
````

