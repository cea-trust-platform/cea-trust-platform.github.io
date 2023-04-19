---
title: "Spatial schemes"
date: 2019-02-28T15:15:34+10:00
weight: 3
---

TRUST implements a wide number of spatial schemes that can be employed to discretize the convective term. However, these schemes are **dependent** of the employed discretization and are thus summarized in what follows.

**Attention:** The diffusion term is more or less a Laplacien operator and is thus always discretized by a centered difference scheme.

**Attention:** TRUST allows the user to neglect the operator contribution. This can be done by using the C++ alias `Negligeable` in the concevtion and/or diffusion bloc.

# Finite Volume Difference (VDF) Schemes

- **Upwind scheme**

	The C++ alias is `Amont`. This is a first order upwind scheme.

- **Centered scheme**

	The C++ aliases are either `Centre` or `Centre4`. They correspond respectively to a second and fourth order cetered schemes.

- **QUICK scheme**

	The C++ alias is `Quick`. This is the third order Quadratic Upstream Interpolation for Convective Kinematics (Quick) scheme.

# Finite Element Volume (VEF) Schemes

- **Upwind scheme**

	The C++ alias is `Amont`. This is a first order upwind scheme.

- **Centered scheme**

	The C++ aliases are either `Centre` or `KCentre`. This is a second order cetered scheme.

- **QUICK scheme**

	The C++ alias is `KQuick`. This is the third order Quadratic Upstream Interpolation for Convective Kinematics (Quick) scheme.
	
- **EF-Stab scheme**

	The C++ alias is `EF_Stab`. This scheme is an upwind/centered mixed schemes. The behavior is controlled by a parameter, alpha, where the scheme behaves as a pure upwind with alpha = 1 and centered with alpha = 0.
	
- **MUSCL scheme**

	The C++ alias is `Muscl`. This is the second order Monotonic Upstream-centered Scheme for Conservation Laws (MUSCL) scheme.

# PolyMAC-series Schemes

- **Upwind scheme**

	The C++ alias is `Amont`. This is a first order upwind scheme.

- **Centered scheme**

	The C++ alias is `Centre`. This is a second order cetered scheme.
	
- **EF-Stab scheme**

	The C++ alias is `EF_Stab`. This scheme is an upwind/centered mixed schemes. The behavior is controlled by a parameter, alpha, where the scheme behaves as a pure upwind with alpha = 1 and centered with alpha = 0.
