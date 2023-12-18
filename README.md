# collo

This is the repo for collocation integrator for ODEs (ordinary differential equations).

The integrator offers adjustable order, single-step, implicit method for solving ODEs : *y' = f(t, y)*. 
Here *y* is a *n* dimensional vector that is represented by **Eigen::Matrix**.


You can find details about collocation integrators in these books:
*Preston C. Hammer and J W Hollingsworth. “Trapezoidal methods of approximating solutions of differential equations”. В: Mathematics of Computation 9 (1955), с. 92—96.
*Ernst Hairer, Christian Lubich and Gerhard Wanner. “Geometric numerical integration illustrated by the Störmer–Verlet method”. В: Acta Numerica 12 (2003), с. 399—450.

##Getting started

You can define *f(t, y)* as a function object with two arguments:  time *t* and vector *y*.
For better performance use **auto** as type of second argument. Or you can use helper alias **collo::state_vector_t<some_float_type, *n*>**.

Include **<collo/lobatto.hpp>** or **<collo/gauss.hpp>** and then you can create integrator with functions **make_Lobatto** or **make_Gauss**. Example:

> `auto lobatto = make_Lobatto<some_float_type, *n*, *s*, type_of_*f*>(*y_0*, *t_0*, *h*);`

Parameter *s* determines how many intermediate points used for calculation of result on every step. Order of  Its value impact accuracy of integrator.
Arguments *y_0* and *t_0* are initial vector and time. Argument *h* is a time step.

###Methods of integrator. 
Use `do_step()` to integrate a single step with a fixed step size. <br>

* `steps()` returns number of calculated steps.
* `time()` returns current timepoint.
* `iternum()` returns number of iterations of main implicit method loop on the last step.
*  `state()` returns current state *y*.
*  ` force()` can be used to get access to *f* and change it if needed.
*  ` poly(some_float_type t)` can be used to get intermediate interpolated vector *y* with argument between 0 and 1. Other values of argument will give extrapolation with collocation polynom.
*  `poly_node(size_t i)` can be used to get vector *y* at collocation node with number i.




##Compilation

Note: You can improve performance of the integrator if you compile your code with option '-march=native'. This feature provided by 'Eigen' library and modern cpu's vectorization. Like this:

`git clone https://github.com/shvak/collo.git`<br>
 `cd collo`<br>
 `mkdir build`<br>
 `CXXFLAGS="-O3 -march=native" cmake -S . -B build`<br>
 `cmake --build build`<br>

Run simple tests:

` ctest --test-dir build --output-on-failure`<br>

##Examples

`build/examples/isochrone/isochrone`<br>
`build/examples/kepler/kepler`<br>
`build/examples/lotka-volterra/lotka-volterra`<br>
