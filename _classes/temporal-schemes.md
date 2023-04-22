---
title: "Temporal schemes"
weight: 4
---

It is mendatory to define a time integration scheme in order to perform a TRUST calculation. Solving a stationary problem is currently not possible (work in progress in a private application but not yet finalized). 

TRUST implements a large selection of time schemes where the use selection is kept as a choice for the user. It is possible to perform completely pure explicit or even pure implicit integration. The code also allows to employ a semi-implicit time integration. All the implemented classes derive from the base class `Schema_Temps_base`. Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classSchema__Temps__base.html)** to see the Doxygen documentation of this class.

The basic point that **should be** taken into account is to pay attention to the time-step and the stability criteria. For example, if one selects an explicit time scheme, both the Courant–Friedrichs–Lewy (CFL) and the Von-Neumann criteria must be respected in order to obtain stable results. This constraint can be relaxed when employing an implicit integrator. 

In cases where the diffusion time step is more critcal than the convective one, TRUST offers the possibility to implicitly treat the diffusion operator (the convective remains explicit). In such a case, respecting the CFL criterion is sufficient. This can play an important role in accelerating the computational time in some cases. 

The stability of the time scheme can be controled in a TRUST's data file by the parameter `facsec` and `facsec_max` (please have a look to the TRUST documentation for a detailed discription). Keep in mind that the non-linear term of the Navier-Stokes equation is not fully implicited, although if one uses a pure implicit time scheme. For this reason, we recommend you to use the `facsec` parameter with moderation !

Here is the list of the time integration schemes available in the platform.

# Explicit schemes

- **Euler Scheme**

	The C++ class is `Scheme_euler_explicit`. This is the Euler explicit scheme of first order.
	
- **Runge Kutta Series**
	
	- The C++ class is `Runge_kutta_ordre_2`. This is a low-storage Runge-Kutta scheme of second order that uses 2 integration points. The method is presented **[here](https://www.sciencedirect.com/science/article/pii/0021999180900339)** by Williamson (case 1).

	- The C++ class is `Runge_kutta_ordre_3`. This is a low-storage Runge-Kutta scheme of third order that uses 3 integration points. The method is presented **[here](https://www.sciencedirect.com/science/article/pii/0021999180900339)** by Williamson (case 7).
	
	- The C++ class is `Runge_kutta-_ordre_4_d3p`. This is a low-storage Runge-Kutta scheme of third order that uses 3 integration points. The method is presented **[here](https://www.sciencedirect.com/science/article/pii/0021999180900339)** by Williamson (case 17).
	
	- The C++ class is `Runge_kutta_rationnel_ordre_2`. This is the Runge-Kutta rational scheme of second order. The method is described in the note: Wambeck - Rational Runge-Kutta methods for solving systems of ordinary differential equations, **[here](https://link.springer.com/article/10.1007/BF02252381)**.  Although rational methods require more
computational work than linear ones, they can have some other properties, such as a stable behaviour with explicitness, which make them preferable. The CFD application of this RRK2 scheme is described in this **[note](https://link.springer.com/content/pdf/10.1007%2F3-540-13917-6_112.pdf)**. 

	- The C++ class is `Runge_kutta_ordre_2_classique`. This is a classical Runge-Kutta scheme of second order that uses 2 integration points.

	- The C++ class is `Runge_kutta_ordre_3_classique`. This is a classical Runge-Kutta scheme of third order that uses 3 integration points.

	- The C++ class is `Runge_kutta_ordre_4_classique`. This is a classical Runge-Kutta scheme of fourth order that uses 4 integration points.
 
	- The C++ class is `Runge_kutta_ordre_4_classique_3_8`. This is a classical Runge-Kutta scheme of fourth order that uses 4 integration points and the 3/8 rule.

	- The C++ class is `Schema_Adams_Bashforth_order_2`. This is the Adams Bashforth second order scheme. 

	- The C++ class is `Schema_Adams_Bashforth_order_3`. This is the Adams Bashforth third order scheme.


# Implicit schemes

- **Euler Scheme**

	The C++ class is `Schema_Euler_Implicite`. This is the Euler implicit scheme of first order.
	
- **Crank Nicholson**

	The C++ class is `Sch_CN_iteratif`. This is the Crank-Nicholson method of second order accuracy. A mid-point rule formulation is used (Euler-centered scheme). The time derivative at the mid-level is calculated iteratively with a simple under-relaxations method. Since the method is implicit, neither the cfl nor the fourier stability criteria must be respected. The time step is calculated in a way that the iterative procedure converges with the less iterations as possible.

- **Adams Moulton Series**

	Two available C++ classes are available: `Schema_Adams_Moulton_order_2` and `Schema_Adams_Moulton_order_3`.
	
- **Backward Differentiation Series**

	Two available C++ classes are available: `Schema_Backward_Differentiation_order_2 ` and `Schema_Backward_Differentiation_order_3 `.

# Semi-Implicit schemes

- **Predictor Corrector**

	The C++ class is `Schema_predictor_corrector`. This is the predictor-corrector scheme (second order). It is more accurate and economic than
MacCormack scheme. It gives best results with a second ordre convective scheme like quick, centre (VDF).

- **Crank Nicholson**

	The C++ class is `Sch_CN_EX_iteratif`. It describes a Crank-Nicholson (CN) method of second order accuracy but here, for scalars, because of instablities encountered when dt > dt\_CFL, the Crank Nicholson scheme is not applied to scalar quantities. Scalars are treated according to Euler-Explicite scheme at the end of the CN treatment for velocity flow fields (by doing p Euler explicite under-iterations at dt <= dt\_CFL). Parameters
are the sames (but default values may change) compare to the `Sch_CN_iterative` scheme plus a relaxation keyword: niter_min (2 by default), niter_max (6 by default), niter_avg (3 by default), facsec_max (20 by default), seuil (0.05 by default)

- **Leap Frog**

	The C++ class is `leap_frog`.  This is the leap-frog scheme.