# ODEs




Some relationships are easiest to describe in terms of derivatives. For example:

* One of Newton's famous laws, $F=ma$, describes the force on an
   object of mass $m$ in terms of the acceleration. The acceleration
   is the derivative of velocity, which in turn is the derivative of
   position. So if we know the rates of change of $v(t)$ or $x(t)$, we
   can differentiate to find $F$.




* Newton's law of [cooling](http://tinyurl.com/z4lmetp). This
  describes the temperature change in an object due to a difference in
  temperature with the object's surroundings. The formula being,
  $T'(t) = -r \left(T(t) - T_a \right)$, where $T(t)$ is temperature at time $t$
  and $T_a$ the ambient temperature.

* [Hooke's law](http://tinyurl.com/kbz7r8l) relates force on an object to the position on the object, through $F = k x$. This is appropriate for many systems involving springs. Combined with Newton's law $F=ma$, this leads to an equation that $x$ must satisfy: $m x''(t) = k x(t)$.

## Motion with constant acceleration

Let's consider the case of constant acceleration. This describes how nearby objects fall to earth, as the force due to gravity is assumed to be a constant, so the acceleration is the constant force divided by the constant mass.

With constant acceleration, what is the velocity?

As mentioned, we have $dv/dt = a$ for any velocity function $v(t)$, but in this case, the right hand side is assumed to be constant. How does this restrict the possible functions, $v(t)$, that the velocity can be?

Here we can integrate to find that any answer must look like the following for some constant of integration:

$$~
v(t) = \int \frac{dv}{dt} dt = \int a dt = at + C.
~$$

If we are given the velocity at a fixed time, say $v(t_0) = v_0$, then we can use the definite integral to get:

$$~
v(t) - v(t_0) = \int_{t_0}^t a dt = at - a t_0.
~$$

Solving, gives:

$$~
v(t) = v_0 + a (t - t_0).
~$$

This expresses the velocity at time $t$ in terms of the initial velocity, the constant acceleration and the time duration.

A natural question might be, is this the *only* possible answer? There are a few useful ways to think about this.

First, suppose there were another, say $u(t)$. Then define $w(t)$ to be the difference: $w(t) = v(t) - u(t)$. We would have that $w'(t) = v'(t) - u'(t) = a - a = 0$. But from the mean value theorem, a function whose derivative is *continuously* $0$, will necessarily be a constant. So at most, $v$ and $u$ will differ by a constant, but if both are equal at $t_0$, they will be equal for all $t$.

Second, since the derivative of any solution is a continuous function, it is true by the fundamental theorem of calculus that it *must* satisfy the form for the antiderivative. The initial condition makes the answer unique, as the indeterminate $C$ can take only one value.

Summarizing, we have

> If $v(t)$ satisfies the equation:
> $$~ v'(t) = a, \quad v(t_0) = v_0,~$$
> then the unique solution will be $v(t) = v_0 + a (t - t_0)$.


Next, what about position? Here we know that the time derivative of position yields the velocity, so we should have that the unknown position function satisfies this equation and initial condition:

$$~
x'(t) = v(t) = v_0 + a (t - t_0), \quad x(t_0) = x_0.
~$$

Again, we can integrate to get an answer for any value $t$:

$$~
x(t) - x(t_0) = \int_{t_0}^t \frac{dv}{dt} dt = (v_0t + \frac{1}{2}a t^2 - at_0 t) |_{t_0}^t =
(v_0 - at_0)(t - t_0) + \frac{1}{2} a (t^2 - t_0^2).
~$$

There are three constants: the initial value for the independent variable, $t_0$, and the two initial values for the velocity and position, $v_0, x_0$.  Assuming $t_0 = 0$, we can simplify the above to get a formula familiar from introductory physics:

$$~
x(t) = x_0 + v_0 t + \frac{1}{2} at^2.
~$$

Again, the mean value theorem can show that with the initial value specified this is the only possible solution.

## First-order initial-value problems

The two problems just looked at can be summarized by the following. We are looking for solutions to an equation of the form (taking $y$ and $x$ as the variables, in place of $x$ and $t$):

$$~
y'(x) = f(x), \quad y(x_0) = y_0.
~$$

This is called an *ordinary differential equation* (ODE), as it is an equation involving the ordinary derivative of an unknown function, $y$.

This is called a first-order, ordinary differential equation, as there is only the first derivative involved.

This is called an initial-value problem, as the value at the initial point $x_0$ is specified as part of the problem.

### Examples

Let's look at a few more examples, and then generalize.

##### Newton's Law of Cooling

Consider the ordinary differential equation given by Newton's law of cooling:

$$~
T'(t) = -r (T(t) - T_a), \quad T(0) = T_0
~$$

This equation is also first order, as it involves just the first derivative, but notice that on the right hand side is the function $T$, not the variable being differentiated against, $t$.

As we have a difference on the right hand side, we rename the variable through $U(t) = T(t) - T_a$. Then, as $U'(t) = T'(t)$, we have the equation:

$$~
U'(t) = -r U(t), \quad U(0) = U_0.
~$$


This shows that the rate of change of $U$ depends on $U$. Large postive values indicate a negative rate of change - a push back towards the origin, and large negative values of $U$ indicate a positive rate of change - again, a push back towards the origin. We shouldn't be surprised to either see a steady decay towards the origin, or oscillations about the origin.

What will we find? This equation is different from the previous two
equations, as the function $U$ appears on both sides. However, we can
rearrange to get:

$$~
\frac{dU}{dt}\frac{1}{U(t)} = -r.
~$$


This suggests integrating both sides, as before. Here we do the "$u$"-substitution $u = U(t)$, so $du = U'(t) dt$:

$$~
-rt + C = \int \frac{dU}{dt}\frac{1}{U(t)} dt =
\int \frac{1}{u}du = \log(u).
~$$

Solving gives: $u = U(t) = e^C e^{-rt}$. Using the initial condition forces $e^C = U(t_0) = T(0) - T_a$ and so our solution in terms of $T(t)$ is:


$$~
T(t) - T_a = (T_0 - T_a) e^{-rt}.
~$$

In words, the initial difference in temperature of the object and the environment exponentially decays to $0$.

That is, as $t > 0$ goes to $\infty$, the right hand will go to $0$ for $r > 0$, so $T(t) \rightarrow T_a$ - the temperature of the object will reach the ambient temperature. The rate of this is largest when the difference between $T(t)$ and $T_a$ is largest, so when objects are cooling the statement "hotter things cool faster" is appropriate.


A graph of the solution for $T_0=200$ and $T_a=72$ and $r=1/2$ is made
as follows. We've added a few line segments from the defining formula,
and see that they are indeed tangent to the solution found for the differential equation.

![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/odes_2_1.png)






The above is implicitly assuming that there could be no other
solution, than the one we found. Is that really the case? We will see
that there is a theorem that can answer this, but in this case, the
trick of taking the difference of two equations satisfying the
equation leads to the equation $W'(t) = r W(t), \text{ and } W(0) =
0$. This equation has a general solution of $W(t) = Ce^{rt}$ and the
initial condition forces $C=0$, so $W(t) = 0$, as before. Hence, the
initial-value problem for Newton's law of cooling has a unique
solution.



In general, the equation could be written as (again using $y$ and $x$ as the variables):

$$~
y'(x) = g(y), \quad y(x_0) = y_0
~$$


This is called an *autonomous*, first-order ODE, as the right-hand side does not depend on $x$.

Let $F(y) = \int_{y_0}^y du/g(u)$, then a solution to the above is $F(y) = x - x_0$, assuming $1/g(u)$ is integrable.


##### Example Toricelli's Law

[Toricelli's Law](http://tinyurl.com/hxvf3qp) describes the speed a jet of water will leave a vessel through an opening below the surface of the water. The formula is $v=\sqrt{2gh}$, where $h$ is the height of the water above the hole and $g$ the gravitational constant. This arises from equating the kinetic energy gained, $1/2 mv^2$ and potential energy lost, $mgh$, for the exiting water.

An application of Torricelli's law is to describe the volume of water in a tank over time, $V(t)$. Imagine a cylinder of cross sectional area $A$ with a hole of cross sectional diameter $a$ at the bottom, Then $V(t) = A h(t)$, with $h$ giving the height. The change in volume over $\Delta t$ units of time must be given by the value $a v(t) \Delta t$, or

$$~
V(t+\Delta t) - V(t) = -a v(t) \Delta t = -a\sqrt{2gh(t)}\Delta t
~$$

This suggests the following formula, written in terms of $h(t)$ should apply:

$$~
A\frac{dh}{dt} = -a \sqrt{2gh(t)}.
~$$

Rearranging, this gives an equation

$$~
\frac{dh}{dt} \frac{1}{\sqrt{h(t)}} = -\frac{a}{A}\sqrt{2g}.
~$$

Integrating both sides yields:

$$~
2\sqrt{h(t)} = -\frac{a}{A}\sqrt{2g} t + C.
~$$

If $h(0) = h_0 = V(0)/A$, we can solve for $C = 2\sqrt{h_0}$, or

$$~
\sqrt{h(t)} = \sqrt{h_0} -\frac{1}{2}\frac{a}{A}\sqrt{2g} t.
~$$


Setting $h(t)=0$ and solving for $t$ shows that the time to drain the tank would be $(2A)/(a\sqrt{2g})\sqrt{h_0}$.


##### Example

Consider now the equation

$$~
y'(x) = y(x)^2, \quad y(x_0) = y_0.
~$$

This is called a *non-linear* ordinary differential equation, as the $y$ variable on the right hand side presents itself in a non-linear form (it is squared). These equations may have solutions that are not defined for all times.

This particular problem can be solved as before by moving the $y^2$ to the left hand side and integrating to yield:

$$~
y(x) = - \frac{1}{C + x},
~$$

and with the initial condition:

$$~
y(x) = \frac{y_0}{1 - y_0(x - x_0)}.
~$$

This answer can demonstrate *blow-up*. That is, in a finite range for $x$ values, the $y$ value can go to infinity. For example, if the initial conditions are $x_0=0$ and $y_0 = 1$, then $y(x) = 1/(1-x)$ is only defined for $x \geq x_0$ on $[0,1)$, as at $x=1$ there is a vertical asymptote.


## Separable equations

We've seen equations of the form $y'(x) = f(x)$ and $y'(x) = g(y)$ both solved by integrating. The same tricks will work for equations of the form $y'(x) = f(x) \cdot g(y)$. Such equations are called *separable*.

Basically, we equate up to constants

$$~
\int \frac{dy}{g(y)} = \int f(x) dx.
~$$

For example, suppose we have the equation

$$~
\frac{dy}{dx} = x \cdot y(x), \quad y(x_0) = y_0.
~$$

Then we can find a solution, $y(x)$ through:

$$~
\int \frac{dy}{y} = \int x dx,
~$$

or

$$~
\log(y) = \frac{x^2}{2} + C
~$$

Which yields:

$$~
y(x) = e^C e^{\frac{1}{2}x^2}.
~$$

Substituting in $x_0$ yields a value for $C$ in terms of the initial information $y_0$ and $x_0$.


## Symbolic solutions

Differential equations are classified according to their type. Different types have different methods for solution, when a solution exists.

The first-order initial value equations we have seen can be described generally by $y'(x) = F(y,x), \quad y(x_0)=x_0$. The equation is *linear* if the function $F$ is linear in $y$; it is *autonomous* if $F(y,x) = G(y)$ (a function of $y$ alone); it is *separable* if $F(y,x) = G(y)H(x)$.

As seen, separable equations are approached by moving the "$y$" terms to one side, the "$x$" terms to the other and integrating. This also applies to autonomous equations then. There are other families of equation types that have exact solutions, and techniques for solution, summarized at this [Wikipedia page](http://tinyurl.com/zywzz4q).

Rather than go over these various families, we demonstrate that `SymPy` can solve many of these equations symbolically.


The `solve` function in `SymPy` solves equations for unknown
*variables*. As a differential equation involves an unknown *function*
there is a different function, `dsolve`. The basic idea is to describe
the differential equation using a symbolic function and then call
`dsolve` to solve the expression.

Symbolic functions are defined by `SymFunction` (or the macro `@symfuns`):

````julia

using CalculusWithJulia   # loads `SymPy`
using Plots
u = SymFunction("u")
````


````
u
````






We will use this to sole the following, known as the *logistic equation*:

$$~
u'(x) = a u(1-u), \quad a > 0
~$$

Before beginning, we look at the form of the equation. When $u=0$ or
$u=1$ the rate of change is $0$, so we expect the function might be
bounded within that range. If not, when $u$ gets bigger than $1$, then
the slope is negative and when $u$ gets less than $0$, the slope is
positive, so there will at least be a drift back to the range
$[0,1]$. Let's see exactly what happens. We define some variables,
restricting `a` to be positive:



````julia

@vars x y
@vars a positive=true
````


````
(a,)
````






Our equation can be specified by an expression equaling $0$:

````julia

eqn = u'(x) - a * u(x) * (1 - u(x))
````


````
d       
-a⋅(1 - u(x))⋅u(x) + ──(u(x))
                     dx
````





In the above, we evaluate the symbolic function at the variable `x`
through the use of `u(x)` in the expression. Finally, we call `dsolve`
to find a solution (if possible):

````julia

out = dsolve(eqn)
````


````
1      
u(x) = ────────────
           -a⋅x    
       C₁⋅ℯ     + 1
````





This answer - to a first-order equation - has one free constant,
`C_1`, which can be solved for from an initial condition. We can see
that when $a > 0$, as $x$ goes to positive infinity the solution goes
to $1$, and when $x$ goes to negative infinity, the solution goes to $0$
and otherwise is trapped in between, as expected.

Suppose that $u(0) = 1/2$. Can we solve for $C_1$ symbolically? We can use `solve`, but first we will need to get the symbol for `C_1`:

````julia

rhs(x) = x.rhs        # convenience to extract right hand side of an equation
eq = rhs(out)    # just the right hand side
C1 = first(setdiff(free_symbols(eq), (x,a))) # fish out constant
c1 = solve(eq(x=>0) - 1//2, C1)
````


````
1-element Array{Sym,1}:
 1
````





And we plug in with:

````julia

eq(C1 => c1[1])
````


````
1    
─────────
     -a⋅x
1 + ℯ
````





That's a lot of work. The `dsolve` function also allows initial conditions to be specified. In this case, ours is $x_0=0$ and $y_0=1/2$. The extra arguments passed are the function (`u(x)`) and tuples of conditions of the form `(SymFunction, x0, y0)` to the `ics` argument:

````julia

x0, y0 = 0, 1//2
dsolve(eqn, u(x), ics=(u, x0, y0))
````


````
1    
u(x) = ─────────
            -a⋅x
       1 + ℯ
````





##### Example: Hooke's law


In the first example, we solved for position, $x(t)$, from an assumption of constant acceleration in two steps. The equation relating the two is a second-order equation: $x''(t) = a$, so two constants are generated. That a second-order equation could be reduced to two first-order equations is not happy circumstance, as it can always be done. Rather than show the technique though, we demonstrate that `SymPy` can also handle some second-order ODEs.

Hooke's law relates the force on an object to its position via $F=ma = -kx$, or $x''(t) = -(k/m)x(t)$.

Suppose $k > 0$. Then we can solve, similar to the above, with:

````julia

@vars k m positive=true
eqn = u''(x) + k/m*u(x)
dsolve(eqn)
````


````
⎛√k⋅x⎞         ⎛√k⋅x⎞
u(x) = C₁⋅sin⎜────⎟ + C₂⋅cos⎜────⎟
             ⎝ √m ⎠         ⎝ √m ⎠
````





Here we find two constants, as anticipated, for we would guess that
two integrations are needed in the solution.

Suppose the spring were started by pulling it down to a bottom and
releasing. The initial position at time $0$ would be $a$, say, and
initial velocity $0$. Here we get the solution specifying initial
conditions on the function and its derivative (expressed through
`u'`):

````julia

@vars a positive=true
dsolve(eqn, x, ics = ((u, 0, -a), (u', 0, 0)))
````


````
⎛√k⋅x⎞
u(x) = -a⋅cos⎜────⎟
             ⎝ √m ⎠
````






We get that the motion will follow
$u(x) = -a \cos(\sqrt{k/m}x)$. This is simple oscillatory behavior. As the spring stretches, the force gets large enough to pull it back, and as it compresses the force gets large enough to push it back. The amplitude of this oscillation is $a$ and the period $2\pi/\sqrt{k/m}$. Larger $k$ values mean shorter periods; larger $m$ values mean longer periods.


##### The pendulum

The simple gravity [pendulum](http://tinyurl.com/h8ys6ts) is an idealization of a physical pendulum that models a "bob" with mass $m$ swinging on a massless rod of length $l$ in a frictionless world governed only by the gravitational constant $g$. The motion can be described by this differential equation for the angle, $\theta$, made from the vertical:

$$~
\theta''(t) + \frac{g}{l}\sin(\theta(t)) = 0
~$$

Can this second-order equation be solved by `SymPy`?

````julia

@vars g l positive=true
eqn = u''(x) + g/l*sin(u(x))
dsolve(eqn)
````


````
Error: PyError ($(Expr(:escape, :(ccall(#= /Users/verzani/.julia/packages/P
yCall/zqDXB/src/pyfncall.jl:43 =# @pysym(:PyObject_Call), PyPtr, (PyPtr, Py
Ptr, PyPtr), o, pyargsptr, kw))))) <class 'NotImplementedError'>
NotImplementedError('solve: Cannot solve g*sin(u(x))/l + Derivative(u(x), (
x, 2))')
  File "/Users/verzani/.julia/conda/3/lib/python3.7/site-packages/sympy/sol
vers/ode.py", line 646, in dsolve
    x0=x0, n=n, **kwargs)
  File "/Users/verzani/.julia/conda/3/lib/python3.7/site-packages/sympy/sol
vers/deutils.py", line 244, in _desolve
    raise NotImplementedError(dummy + "solve" + ": Cannot solve " + str(eq)
)
````





That long error message can be summarized: no easy answer is forthcoming for this equation. In general, for the first-order initial value problem characterized by $y'(x) = F(y,x)$, there are conditions ([Peano](http://tinyurl.com/h663wba) and [Picard-Lindelof](http://tinyurl.com/3rbde5e)) that can guarantee the existence (and uniqueness) of equation locally, but there may not be an accompanying method to actually find it. This particular problem has a solution, but it can not be written in terms of elementary functions.

However, as [Huygens](https://en.wikipedia.org/wiki/Christiaan_Huygens) first noted, if the angles involved are small, then we approximate the solution through the linearization $\sin(\theta(t)) \approx \theta(t)$. The resulting equation for an approximate answer is just that of Hooke:


$$~
\theta''(t) + \frac{g}{l}\theta(t) = 0
~$$

Here, the solution is in terms of sines and cosines, with period given by $T = 2\pi/\sqrt{k} =  2\pi\cdot\sqrt{l/g}$. The answer does not depend on the mass, $m$, of the bob nor the amplitude of the motion, provided the small-angle approximation is valid.

If we pull the bob back an angle $a$ and release it then the initial conditions are $\theta(0) = a$ and $\theta'(a) = 0$. This gives the solution:

````julia

eqn = u''(x) + g/l * u(x)
dsolve(eqn, u(x), ics=((u, 0, a), (u', 0, 0)))
````


````
⎛√g⋅x⎞
u(x) = a⋅cos⎜────⎟
            ⎝ √l ⎠
````






##### Example: Hanging cables

A chain hangs between two supports a distance $L$ apart. What shape
will it take if there are no forces outside of gravity acting on it?
What about if the force is uniform along length of the chain, like a
suspension bridge? How will the shape differ then?

Let $y(x)$ describe the chain at position $x$, with $0 \leq x \leq L$,
say. We consider first the case of the chain with no force save
gravity. Let $w(x)$ be the density of the chain at $x$, taken below to be a constant.

The chain is in equilibrium, so tension, $T(x)$, in the chain will be
in the direction of the derivative. Let $V$ be the vertical component
and $H$ the horizontal component. With only gravity acting on the
chain, the value of $H$ will be a constant. The value of $V$ will vary
with position.

At a point $x$, there is $s(x)$ amount of chain with weight $w \cdot s(x)$. The tension is in the direction of the tangent line, so:

$$~
\tan(\theta) = y'(x) = \frac{w s(x)}{H}.
~$$

In terms of an increment of chain, we have:

$$~
\frac{w ds}{H} = d(y'(x)).
~$$

That is the ratio of the vertical and horizontal tensions in the increment are in balance with the differential of the derivative.


But $ds = \sqrt{dx^2 + dy^2} = \sqrt{dx^2 + y'(x)^2 dx^2} = \sqrt{1 + y'(x)^2}dx$, so we can simplify to:


$$~
\frac{w}{H}\sqrt{1 + y'(x)^2}dx =y''(x)dx.
~$$

This yields the second-order equation:

$$~
y''(x) = \frac{w}{H} \sqrt{1 + y'(x)}.
~$$

This equation can be solved exactly, though with some work, as a direct approach fails:

````julia

u = SymFunction("u")
@vars x w H positive=true
out = dsolve(u''(x) - w/H * sqrt(1 + u'(x)^2))
````


````
⎛     w⋅x⎞
            H⋅cosh⎜C₂ + ───⎟
                  ⎝      H ⎠
u(x) = C₁ + ────────────────
                   w
````





To help `SymPy` out we break the problem into steps. For the first step we solve for the derivative.
Let $u = y'$, then we have $u'(x) = (w/H)\sqrt{1 + u(x)^2}$:

````julia

out = dsolve(u'(x) - w/H * sqrt(1 + u(x)^2))
````


````
⎛     w⋅x⎞
u(x) = sinh⎜C₁ + ───⎟
           ⎝      H ⎠
````





So $y'(x) = \sinh(C_1 + w \cdot x/H)$. This can be solved by direct
integration as there is no $u(x)$ term on the right hand
side. Repurposing `u` to represent `y` now, we have the equation and solution:

````julia

eqn = u'(x) - rhs(out)
out1 = dsolve(eqn)
````


````
⎛     w⋅x⎞
            H⋅cosh⎜C₁ + ───⎟
                  ⎝      H ⎠
u(x) = C₂ + ────────────────
                   w
````





The shape is a hyperbolic cosine, known as the catenary.


If the chain has a uniform load, sufficient to make the weight of the chain negligible, then how does the above change? Then the vertical tension comes from $Udx$ and not $w ds$, so the equation becomes instead:

$$~
\frac{Udx}{H} = d(y'(x)).
~$$

This $y''(x) = U/H$, a constant. So it's answer will be a parabola.



##### Example: projectile motion in a medium


The first example describes projectile motion without air resistance. If we use $(x(t), y(t))$ to describe position at time $t$, the functions satisfy:

$$~
x''(t) = 0, \quad y''(t) = -g.
~$$

That is, the $x$ position - where no forces act - has $0$ acceleration, and the $y$ position - where the force of gravity acts - has constant acceleration, $-g$, where $g=9.8m/sec^2$ is the gravitational constant. These equations can be solved to give:

$$~
x(t) = x_0 + v_0 \cos(\alpha) t, \quad y(t) = y_0 + v_0\sin(\alpha)t - \frac{1}{2}g \cdot t^2.
~$$


Furthermore, we can solve for $t$ from $x(t)$, to get an equation describing $y(x)$. Here are all the steps:

````julia

@vars x0 y0 v0 alpha g real=true
@vars x y t
u = SymFunction("u")
a1 = dsolve(u''(t) + 0, u(t), ics=((u, 0, x0), (u', 0, v0 * cos(alpha))))
a2 = dsolve(u''(t) - g, u(t), ics=((u, 0, y0), (u', 0, v0 * sin(alpha))))
ts = solve(x - rhs(a1), t)[1]
y = simplify(rhs(a2)(t => ts))
````


````
2       2       2        2                  
g⋅(x - x₀)  + 2⋅v₀ ⋅y₀⋅cos (α) + v₀ ⋅(x - x₀)⋅sin(2⋅α)
──────────────────────────────────────────────────────
                        2    2                        
                    2⋅v₀ ⋅cos (α)
````





Though messy, it can be seen that the answer is a quadratic polynomial in $x$ yielding the familiar
parabolic motion for a trajectory.


In a resistive medium, there are drag forces at play. If this force is
proportional to the velocity, say, with proportion $\gamma$, then the
equations become:

$$~
x''(t) = -\gamma x'(t), \quad y''(t) = -\gamma y'(t) -g, \quad x(0) = x_0, y(0)=y_0, x'(0) = v_0\cos(\alpha), y'(0) = v_0 \sin(\alpha).
~$$


We now attempt to solve these:

````julia

@vars gamma
u = SymFunction("u")
a1 = dsolve(u''(t) + gamma * u'(t),     u(t), ics=((u, 0, x0), (u', 0, v0 * cos(alpha))))
a2 = dsolve(u''(t) + gamma * u'(t) + g, u(t), ics=((u, 0, y0), (u', 0, v0 * sin(alpha))))
ts = solve(x - rhs(a1), t)[1]
y = simplify(rhs(a2)(t => ts))
````


````
⎛       v₀⋅cos(α)       ⎞                 
   
                            g⋅log⎜───────────────────────⎟                 
   
    g⋅x           g⋅x₀           ⎝-γ⋅x + γ⋅x₀ + v₀⋅cos(α)⎠                 
   
─────────── - ─────────── - ────────────────────────────── + x⋅tan(α) - x₀⋅
tan
γ⋅v₀⋅cos(α)   γ⋅v₀⋅cos(α)                  2                               
   
                                          γ                                
   

        
        
        
(α) + y₀
````






This gives $y$ as a function of $x$.

There are a lot of symbols. Lets simplify by using constants $x_0=y_0=0$:

````julia

y = y(x0 => 0, y0 => 0)
````


````
⎛   v₀⋅cos(α)    ⎞           
              g⋅log⎜────────────────⎟           
    g⋅x            ⎝-γ⋅x + v₀⋅cos(α)⎠           
─────────── - ─────────────────────── + x⋅tan(α)
γ⋅v₀⋅cos(α)               2                     
                         γ
````






What is the trajectory? We see
that the `log` function part will have issues when
$-\gamma x + v_0 \cos(\alpha) = 0$.

If we fix some parameters, we can plot.

````julia

v_0, gam, alp = 200, 1/2, pi/4
soln = y(v0=>v_0, gamma=>gam, alpha=>alp, g=>9.8)
plot(soln, 0, v_0 * cos(alp) / gam - 1/10, legend=false)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/odes_20_1.png)



We can see that the resistance makes the path quite non-symmetric.

## Visualizing a first-order initial value problem

The solution, $y(x)$, is known through its derivative. A useful tool to visualize the solution to a first-order differential equation is the [slope field](http://tinyurl.com/jspzfok) (or direction field) plot, which at different values of $(x,y)$, plots a vector with slope given through $y'(x)$.The `vectorfieldplot` of the `CalculusWithJulia` package can be used to produce these.



For example, in a previous example we found a solution to  $y'(x) = x\cdot y(x)$. Suppose $x_0=1$ and $y_0=1$. Then a direction field plot is drawn through:

````julia

@vars x y
x0, y0 = 1, 1
F(y, x) = y*x

plot(legend=false)
vectorfieldplot!((x,y) -> [1, F(y,x)], xlims=(x0, 2), ylims=(y0-5, y0+5))

f(x) =  y0*exp(-x0^2/2) * exp(x^2/2)
plot!(f,  linewidth=5)
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/odes_21_1.png)



In general, if the first-order equation is written as $y'(x) = F(y,x)$, then we plot a "function" that takes $(x,y)$ and returns an $x$ value of $1$ and a $y$ value of $F(y,x)$, so the slope is $F(y,x)$.

````
CalculusWithJulia.WeaveSupport.Alert(L"The order of variables in $F(y,x)$ i
s conventional with the equation $y'(x) = F(y(x),x)$.
", Dict{Any,Any}(:class => "info"))
````






The plots are also useful for illustrating solutions for different initial conditions:


````julia

p = plot(legend=false)
vectorfieldplot!((x,y) -> [1,F(y,x)], xlims=(x0, 2), ylims=(y0-5, y0+5))
for y0 in -4:4
  f(x) =  y0*exp(-x0^2/2) * exp(x^2/2)
  plot!(f, x0, 2, linewidth=5)
end
p
````


![](/var/folders/k0/94d1r7xd2xlcw_jkgqq4h57w0000gr/T/odes_23_1.png)



Such solutions are called [integral
curves](https://en.wikipedia.org/wiki/Integral_curve).
These graphs illustrate the fact that the slope field is tangent to the graph of any
integral curve.



## Questions




##### Question

Using `SymPy` to solve the differential equation

$$~
u' = \frac{1-x}{u}
~$$

gives

````julia

u = SymFunction("u")
@vars x
dsolve(u'(x) - (1-x)/u(x))
````


````
2-element Array{Sym,1}:
 Eq(u(x), -sqrt(C1 - x^2 + 2*x))
  Eq(u(x), sqrt(C1 - x^2 + 2*x))
````





The two answers track positive and negative solutions. For the initial condition, $u(-1)=1$, we have the second one is appropriate: $u(x) = \sqrt{C_1 - x^2 + 2x}$. At $-1$ this gives: $1 = \sqrt{C_1-3}$, so $C_1 = 4$.

This value is good for what values of $x$?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$[-1, 4]$"
, L"$[-1, \infty)$", L"$[1-\sqrt{5}, 1 + \sqrt{5}]$", L"$[-1, 0]$"], 3, "",
 nothing, [1, 2, 3, 4], LaTeXStrings.LaTeXString[L"$[-1, 4]$", L"$[-1, \inf
ty)$", L"$[1-\sqrt{5}, 1 + \sqrt{5}]$", L"$[-1, 0]$"], "", false)
````






##### Question

Suppose $y(x)$ satisfies

$$~
y'(x) = y(x)^2, \quad y(1) = 1.
~$$

What is $y(3/2)$?

````
CalculusWithJulia.WeaveSupport.Numericq(2.0, 0.001, "", "[1.999, 2.001]", 1
.999, 2.001, "", "")
````





##### Question

Solve the initial value problem

$$~
y' = 1 + x^2 + y(x)^2 + x^2 y(x)^2, \quad y(0) = 1.
~$$

Use your answer to find $y(1)$.

````
CalculusWithJulia.WeaveSupport.Numericq(-1.6386248637145369, 0.001, "", "[-
1.63962, -1.63762]", -1.6396248637145368, -1.637624863714537, "", "")
````





##### Question

A population is modeled by $y(x)$. The rate of population growth is generally proportional to the population ($k y(x)$), but as the population gets large, the rate is curtailed $(1 - y(x)/M)$.

Solve the initial value problem

$$~
y'(x) = k\cdot y(x) \cdot (1 - y(x)/M),
~$$

when $k=1$, $M=100$, and $y(0) = 20$. Find the value of $y(5)$.

````
CalculusWithJulia.WeaveSupport.Numericq(97.37555469386476455991468766438340
038789284601247847344470307065781047878026761, 0.001, "", "[97.374550000000
00000000000000000000000000000000000000000000000000000000000000043, 97.37654
999999999999999999999999999999999999999999999999999999999999999999999974]",
 97.37455469386476455989387098267167870275990291870177056384369565781047878
026761, 97.3765546938647645599355043460951220730257891062551763255624456578
1047878026761, "", "")
````






##### Question

Solve the initial value problem

$$~
y'(t) = \sin(t) - \frac{y(t)}{t}, \quad y(\pi) = 1
~$$

Find the value of the solution at $t=2\pi$.

````
CalculusWithJulia.WeaveSupport.Numericq(-1, 0, "", "[-1.0, -1.0]", -1, -1, 
"", "")
````






##### Question

Suppose $u(x)$ satisfies:

$$~
\frac{du}{dx} = e^{-x} \cdot u(x), \quad u(0) = 1.
~$$

Find $u(5)$ using `SymPy`.

````
CalculusWithJulia.WeaveSupport.Numericq(2.700027756117370154212487619404962
557343246917590398528605527399056158996408393, 0.001, "", "[2.6990299999999
99999999999999999999999999999999999999999999999999999999999999996, 2.701029
999999999999999999999999999999999999999999999999999999999999999999999997]",
 2.699027756117370154191670937693240872210303823813695647746152399056158996
408393, 2.70102775611737015423330430111668424247619001136710140946490239905
6158996408393, "", "")
````





##### Question

The differential equation with boundary values

$$~
\frac{r^2 \frac{dc}{dr}}{dr} = 0, \quad c(1)=2, c(10)=1,
~$$

can be solved with `SymPy`. What is the value of $c(5)$?

````
CalculusWithJulia.WeaveSupport.Radioq(LaTeXStrings.LaTeXString[L"$10/9$", L
"$8/9$", L"$3/2$", L"$9/10$"], 1, "", nothing, [1, 2, 3, 4], LaTeXStrings.L
aTeXString[L"$10/9$", L"$8/9$", L"$3/2$", L"$9/10$"], "", false)
````






##### Question

The example with projectile motion in a medium has a parameter
$\gamma$ modeling the effect of air resistance. If `y` is the
answer - as would be the case if the example were copy-and-pasted
in - what can be said about `limit(y, gamma=>0)`?

````
CalculusWithJulia.WeaveSupport.Radioq(["The limit is a quadratic polynomial
 in `x`, mirroring the first part of that example.", "The limit does not ex
ist, as there is a singularity, as seen by setting `gamma=0`.", "The limit 
does not exists, but the limit to `oo` gives a quadratic polynomial in `x`,
 mirroring the first part of that example."], 1, "", nothing, [1, 2, 3], ["
The limit is a quadratic polynomial in `x`, mirroring the first part of tha
t example.", "The limit does not exist, as there is a singularity, as seen 
by setting `gamma=0`.", "The limit does not exists, but the limit to `oo` g
ives a quadratic polynomial in `x`, mirroring the first part of that exampl
e."], "", false)
````

