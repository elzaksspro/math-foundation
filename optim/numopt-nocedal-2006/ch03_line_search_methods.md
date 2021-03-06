# ch03: Line Search Methods

* Each iteration of a line search method
  * computes a search direction $p_k$ and
  * then decides how far to move along that direction.
* the iteration: $x_{k+1} = x_k + \alpha_k p_k$
  * $\alpha_k$: step length
* the search direction has the form: $p_k = - B_k^{-1} \nabla f_k$
  * $B_k$ is a symmetric and nonsingular matrix.
  * steepest descent: $B_k$ is the identity matrix
  * Newton's method:  $B_k$ is the exact Hessian $\nabla^2 f(x_k)$
  * quasi-Newton methods: $B_k$ is an approximation to the Hessian that
    is updated at every iteration by means of a low-rank formula

# 3.1 step length
* a tradeoff.
  * to choose $\alpha_k$ to give a substantial reduction of f
  * but at the same time we do not want to spend too much time making the choice
* practical strategies perform an inexact line search to identify a step length that
  achieves adequate reductions in f at minimal cost.
* ensure that the step length α achieves sufficient decrease but is not too short.
* Typical line search algorithms
  * try out a sequence of candidate values for α,
  * stopping to accept one of these values when certain conditions are satisfied.
* The line search is done in two stages:
  * A bracketing phase finds an interval containing desirable step lengths, and
  * a bisection or interpolation phase computes a good step length within this interval.
* effective step lengths ** need not lie** near minimizers of the univariate function
  defined in (3.3).

## the wolfe conditions
* the curvature condition ensures that the slope of $\phi$ at
  $\alpha_k$ is greater than $c_2$ times the initial slope $\phi(0)$
* the wolfe conditions consists of
  * the sufficient decrease (Armijo) condition, equ (3.7a)
  * curvature conditions, equ (3.7b)
* strong Wolfe conditions (cf regular/weak Wolfe condition)
  * modify the curvature condition to force αk to lie in at least a broad neighborhood of
    a local minimizer or stationary point of $\phi$.
  * no longer allow the derivative $\phi'(\alpha_k)$ to be too positive.
    * Hence, we exclude points that are far from stationary points of $\phi$
* the wolfe conditions are scale-invariant in a broad sense:
  * multiplying the objective function by a constant or
    making an affine change of variables does not alter them
* In practice, $c_1$ is chosen to be quite small, say
  * $c_1 = 10^{-4}
* Typical values of $c_2$ are
  * 0.9 when the search direction pk is chosen by a Newton or quasi-Newton method, and
  * 0.1 when pk is obtained from a nonlinear conjugate gradient method.

## the goldstein conditions
* often used in Newton-type methods
  * but are NOT well suited for quasi-Newton methods that
    maintain a positive definite Hessian approximation.

## sufficient decrease and backtracking
* backtracking approach:
  * can dispense with the extra condition (3.6b) and
  * use just the sufficient decrease condition to terminate the line search procedure
* WELL SUITED for Newton methods
  * but is LESS APPROPRIATE for quasi-Newton and conjugate gradient methods.

# 3.2 convergence of line search methods
TODO

# 3.3 rate of convergence
## newton’s method
* the search is given by
  * $p_k^N = - \nabla^2 f_k^{-1} \nabla f_k$

## quasi-newton methods
* search direction has the form
  * $p_k = - B_k^{-1} \nabla f_k$

# 3.4 newton’s method with hessian modification
* Line Search Newton with Modification

# 3.5 step-length selection algorithms
* Line search (step-length search) procedures can be classified according to
  the type of derivative information they use.
  * that use only function values can be inefficient since,
    to be theoretically sound, they need to continue iterating until
    the search for the minimizer is narrowed down to a small interval.
  * that use knowledge of gradient information allows us to determine
    whether a suitable step length has been located, as stipulated, for example,
    by the Wolfe conditions (3.6) or Goldstein conditions (3.11)
* All line search procedures require
  * an initial estimate α0 and
  * generate a sequence {αi } that
    * either terminates with a step length satisfying the conditions specified by
      the user (for example, the Wolfe conditions) or
    * determines that such a step length does not exist.
* Typical procedures consist of two phases:
  * a bracketing phase that finds an interval [ā, b̄] containing acceptable step lengths, and
  * a selection phase that zooms in to locate the final step length, usually
    * reduces the bracketing interval during its search for the desired
      step length and
    * interpolates some of the function and derivative information gathered on
      earlier steps to guess the location of the minimizer.

## interpolation
* to generate a decreasing sequence of values αi such that each value αi is not too much smaller
  than its predecessor αi−1
* cubic interpolation
  * provides a good model for functions with significant changes of curvature
  * usually produces a quadratic rate of convergence of the iteration (3.59) to the minimizing value of α.


## initial step length
* For Newton and quasi-Newton methods,
  * the step $\aplha_0 = 1$ should always be used as the initial trial step length.
  * This choice
    * ensures that unit step lengths are taken whenever they satisfy the termination conditions and
    * allows the rapid rate-of-convergence properties of these methods to take effect.
* For methods that do not produce well scaled search directions,
  such as the steepest descent and conjugate gradient methods,
  * important to use current information about the problem and the algorithm to make the initial guess.
  * A popular strategy is to assume that
    the first-order change in the function at iterate $x_k$ will be the same as that
    obtained at the previous step

## a line search algorithm for the wolfe conditions
* Wolfe (or strong Wolfe) conditions are among the most widely applicable and useful termination conditions
* The algorithm has two stages. T
  * stage-1:
    * begins with a trial estimate α1 , and
    * keeps increasing it until it finds either an acceptable step length or
      an interval that brackets the desired step lengths.
  * stage-2:
    * calling a function called zoom (Algorithm 3.6, below), which
      successively decreases the size of the interval until an
      acceptable step length is identified.
* how much more expensive it is to require the strong Wolfe conditions
  instead of the regular Wolfe conditions.
  * for a “loose” line search (with parameters such as c1 = 10−4 and c2 = 0.9),
    both strategies require a similar amount of work.
* The strong Wolfe conditions have the advantage that by decreasing c2 we
  can directly control the quality of the search, by forcing the accepted value of α to lie closer
  to a local minimum.
  * This feature is important in steepest descent or nonlinear conjugate gradient methods, and
  * therefore a step selection routine that enforces the strong Wolfe conditions has wide applicability.

## notes and references
* Some line search methods (see Goldfarb [132] and Moré and Sorensen [213]) compute
a direction of negative curvature, whenever it exists, to prevent the iteration from converging
to nonminimizing stationary points
* Another strategy for implementing a line search Newton method when the Hessian
contains negative eigenvalues is to compute a direction of negative curvature and use it to
define the search direction
