---
title: "Solvers"
weight: 8
---

Linear matricial systems appear basically with a form of **AX = B** at two stages in the code resolution algorithm: when dealing with the Elleptic pressure Poisson equation at first, and when dealing with the implicit resolution at second (either diffusion implicit or a full implicit time integration scheme).

The platform TRUST allows the use of a wide range of solvers/preconditioners in order to solve such linear systems. Ttwo types of solvers are mainly available: either home-coded ones or called from external libraries, for example from the open source **[PETSc](https://petsc.org/release/install/license/#doc-license)** or **[rocALUTION](https://rocalution.readthedocs.io/en/master/intro.html#license)** libraries. As stated previously, TRUST has been ported recently to support GPU acceleration (NVidia/AMD). The solvers are the main candidates that benefit from this new advantage.

The base class for the solvers is `SolveurSys_base`. Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classSolveurSys__base.html)** to see its Doxygen documentation. If an iterative solver is desired, a pre-conditioner from the base class `Precond_base` can be used. Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classPrecond__base.html)** to see the Doxygen documentation of the `Precond_base` class.

In cases where the user asks to treat implicitly the diffusion operator in an explicit time integration scheme, a Preconditioned Conjugate Gradient (GCP) solver will be used (by default) to solve the implicit matrix. However, it is possible to select a specific solver. This can be done by the `Parametre_equation` and the `Parametre_diffusion_implicite ` classes. These instances are optional and can be inserted in the bloc of each equation. Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classParametre__equation__base.html)** to see the Doxygen documentation of this class.

If a pure implicit scheme is used in a TRUST calculation, an implicit solver **must** be defined too. This is done via the keyword `Solveur` that reads a solver inherited from the base class `Solveur_Implicite_Base`. In such case, a solver from `SolveurSys_base` will be read inside this bloc. Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classSolveur__Implicite__Base.html)** to see the Doxygen documentation of the `Solveur_Implicite_Base` class.

# Linear Solvers

**Direct home-coded solvers**

`Cholesky`: Cholesky direct method.

**Iterative home-coded solvers**

- `GCP`: Preconditioned Conjugate Gradient.

- `GMRES`: Gmres method (for non symetric matrix).

- `GEN`: Generic solver.

**External solvers**

- `PETSc`: Several solvers via PETSc API are available: 
	
	- `GCP` : Preconditioned Conjugate Gradient.

	- `PIPECG`: Pipelined Conjugate Gradient.

	- `GMRES`: Generalized Minimal Residual.

	- `BICGSTAB`: Stabilized Bi-Conjugate Gradient.

	- `IBICGSTAB`: Improved version of previous one for massive parallel computations, only a single global reduction operation instead of the usual 3 or 4.

	- `CHOLESKY_OUT_OF_CORE`: Same as the previous one but with a written LU decomposition of disk (save RAM memory but add an extra CPU cost during Ax=B solve).

	- `CHOLESKY_SUPERLU`: Parallelized Cholesky from SUPERLU_DIST library (less CPU and RAM efficient than the previous one).

	- `CHOLESKY_PASTIX`: Parallelized Cholesky from PASTIX library.

	- `CHOLESKY_UMFPACK`: Sequential Cholesky from UMFPACK library (seems fast).

	- `CLI`: Command Line Interface. Should be used only by advanced users, to access the whole solver/preconditioners from the PETSC API. To find all the available options, run your calculation with the -ksp_view -help options (Krylov Method Options).

- `AMGX`: Solver via AmgX API.

- `PETSc_GPU`: Solver via PETSc API for GPU.

- `rocALUTION`: Solver via rocALUTION API.

# Pre-Conditioners

**Home-coded pre-conditioners**

- `ILU`: Can be only used with the generic GEN solver.

- `SSOR`: Symmetric successive over-relaxation algorithm.

- `SSOR_Bloc`: Bloc symmetric successive over-relaxation algorithm.

**External pre-conditioners**

If the **CLI** solver is used from PETSc, the user has acces to the following pre-conditioners:

- **NULL { }**: No preconditioner used.

- **BLOCK\_JACOBI\_ICC { level k ordering natural \| rcm }**: Incomplete Cholesky factorization for symmetric matrix with the PETSc implementation. The integer k is the factorization level (default value, 1). In parallel, the factorization is done by block (one per processor by default). The ordering of the local matrix is natural by default, but rcm ordering, which reduces the bandwith of the local matrix, may interestingly improves the quality of the decomposition and reduces the number of iterations.

- **SSOR { omega double }**: Symmetric Successive Over Relaxation algorithm. omega (default value, 1.5) defines the relaxation factor.

- **EISENTAT { omega double }**: SSOR version with Eisenstat trick which reduces the number of computations and thus CPU cost.

- **SPAI { level nlevels epsilon thresh }**: Spai Approximate Inverse algorithm from Parasails Hypre library. Two parameters are available, nlevels and thresh.

- **PILUT { level k epsilon thresh }**: Dual Threashold Incomplete LU factorization. The integer k is the factorization level and epsilon is the drop tolerance.
	
- **DIAG { }**: Diagonal (Jacobi) preconditioner.
	
- **BOOMERAMG { }**: Multigrid preconditioner (no option is available yet, look at CLI command and
Petsc documentation to try other options).
	
	
# Implicit Solvers

Here are the available C++ class aliases that can be used in the situation where the time scheme is the implicit scheme. These Solvers allow the equation diffusion and convection operators to be set as implicit terms.

**Linear Solvers**

- `Solveur_lineaire_std`: Standard linear solver.

	
**Non-Linear Solvers**

- `Simple`: Semi-Implicit Method for Pressure Linked Equations (SIMPLE) type algorithm, see **[CT Shaw](https://onlinelibrary.wiley.com/doi/pdf/10.1002/fld.1650120106)**.

- `Simpler`: Semi-Implicit Method for Pressure Linked Equations Revised (SIMPLER) method for incompressible systems, see **[DS Jang](https://www.tandfonline.com/doi/abs/10.1080/10407788608913517)**.

- `Piso`: Pressure Implicit with Split Operator (PISO), a method to solve the implicit Navier-Stokes equation, see **[RI Issa](https://www.sciencedirect.com/science/article/pii/0021999186900999)**.

- `Implicite`: Similar to PISO, but as it looks like a simplified solver, it will use fewer timesteps. But it may run faster because the pressure matrix is not re-assembled and thus provides CPU gains.

- `SETS`: Stability-Enhancing Two-Step (SETS) solver which is useful for a multiphase problem, see **[JH Mahaffy](https://www.sciencedirect.com/science/article/pii/0021999182900195)**.

- `ICE`: Implicit Continuous-fluid Eulerian solver which is useful for a multiphase problem. Robust
pressure reduction resolution. See **[FH Harlow](https://www.sciencedirect.com/science/article/pii/0021999171900027)**.

- `Solveur_U_P`: Similar to simple.

# Hints

Use the following when possible

**TRUST home-coded solvers**
	
- Symmetric matrix (e.g. pressure solver)

	- Use GCP with SSOR preconditioning.
	
- Non-symmetric matrix (e.g. implicit solver)

	- Use GMRES or Bi-CGSTAB with diagonal preconditioning.

**Integrated PETSc solvers**

- Symmetric matrix

	- Use Parallelized Cholesky (If enough memory available AND constant matrix).

	- Otherwise, use BI-CGSTAB with block Jacobi.

- Non-symmetric matrix
	
	- Use GMRES or BI-CGSTAB with diagonal preconditioning (Generally faster than previous TRUST versions).

# Selected Examples

**Pressure solver examples**
	
	Solveur_pression PETSc GCP { precond SSOR { omega 1.6 }  seuil 1.e-12 impr   }
		
	Solveur_pression PETSc Cholesky { }
	
	solveur Gen { seuil 1e-8 solv_elem BiCGStab precond ILU { type 2 filling 20 }  }

**Diffusion implicit solver example**

	Parametre_equation Parametre_diffusion_implicite
	{
		Solveur PETSc GCP { precond block_jacobi_ilu { level 0 } rtol 1.e-13 impr }
	}

**Implicit solver example**
		
	Solveur Implicite
	{
		Solveur PETSc CLI
		{
			-pc_type hypre
			-pc_hypre_type boomeramg
			-pc_hypre_boomeramg_strong_threshold 0.8
			-pc_hypre_boomeramg_agg_nl 4
			-pc_hypre_boomeramg_agg_num_paths 5
			-pc_hypre_boomeramg_max_levels 25
			-pc_hypre_boomeramg_coarsen_type PMIS
			-pc_hypre_boomeramg_interp_type ext+i
			-pc_hypre_boomeramg_P_max 2
			-pc_hypre_boomeramg_truncfactor 0.5
			-ksp_type fgmres
		}
	}
	
	Solveur Sets
	{
		criteres_convergence { alpha 1e-5 pression 1. vitesse 1e-5 temperature 1e8 k 1e-5 omega 1e-5 }
		iter_min 2
		Solveur PETSc CLI { }
		seuil_convergence_implicite 1e30
	}
		    
	Solveur Implicite 
	{
		seuil_convergence_implicite 1e+0
		seuil_convergence_solveur  1e-4
		Solveur Gen { seuil 1e-8 solv_elem BiCGStab precond ILU { type 2 filling 20 }  }
		# Solveur GMRES { diag seuil 1e-8 impr } #
	}