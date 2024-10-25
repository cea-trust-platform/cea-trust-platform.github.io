---
title: "Post-Processing"
weight: 7
---

Do you know that CFD refers to Colorful Fluid Dynamics ? OK, it is not exactly that but ... it can be !

Moreover, do you know also that the best moments you spend after running a simulation are those where you visualize your results. With TRUST, this is surely possible. However, keep in mind that you should explicitly define what you want to visualize, which fields, probes, statistics, which format and at what frequency.

All this is done via the TRUST C++ classes: `Post_processing` or `Post_processings` (aliases `Postraitement` or `Postraitements`). The difference between both classes is that the first one creates a single post-processing object, while the other can creates N objects if you ask for N post-processing blocks. These keywords should be placed at the end of the `Problem` block. Click **[here](https://cea-trust-platform.github.io/TRUST_Doxygen.github.io/html/classPostraitement__base.html)** to see the Doxygen documentation of the `Postraitement` class.

In both cases (whatever which class you use, `Post_processing` or `Post_processings`) you can define the following.

# Probes

This is done by the C++ class `Sonde` or `Sondes` (list of probes). You can request spatial/temporal variation/evolution of any field (should be known by TRUST). The values can be extracted at a single point in space, at a segment of points or in a plane.

**Attention:** The probe coordinates should be given in Cartesian coordinates (X, Y, Z) even in axisymmetric cases.

# Define New Fields (Advanced Fields)

This is done by the C++ class `Definition_champs`. In this block, you can create new complex fields for advanced post-processings. For example, post-process custom quantities not directly available from the datafile (average of a field, error between TRUST and analytical solution, ...).

# Write Fields

This is done by the C++ class `Fields` or `Champs`. Here you can specify the frequency by the keyword `dt_post`. For example, the following syntax is used to post-process (and therefore visualize) the pressure (at center of the elements) each 1000 **physical seconds** of the simulation.

    Post_processing
    {
        Fields dt_post 1000
        {
            pression elem
        }
    }

It is also possible to specify the output format of files. This is done by the keyword `Format`, where four formats are available: `lml`, `lata`, `med` and `cgns`. The default format is `lml`, used for the non-regression verification only. We recommend the `cgns` format for post-processing. Results can be visualized by **[VisIt](https://visit-dav.github.io/visit-website/index.html)**, **[Salome](https://www.salome-platform.org/?lang=fr)** (Paravis module), **[Paraview](https://www.paraview.org/)** and **[TecPlot](https://www.tecplot.com/)**.

**Note:** It is possible to convert files written in the `lata` format into `lml` and `med` format. This is done by the class `lata_to_other`. The syntax is the following

	lata_to_other med NOM_DU_CAS NOM_DU_CAS
or

	lata_to_other lml NOM_DU_CAS NOM_DU_CAS

**Remark 1:** By default, fields are post-processed on the whole domain. You can choose to post-process fields on a boundary or on a specific domain region.

**Remark 2:** Remember fields location for each discretization! If you specify in post-processing block for a field a different location than where it is computed, values will be interpolated.

# List of Existing and Pre-defined Fields

Here is a list of post-processable fields, but it is not the only ones!

| **Physical values**                        | **Keyword for field_name**       | **Unit**                 |
|:------------------------------------------:|:--------------------------------:|:------------------------:|
| Velocity                                   | Vitesse or Velocity              | $m.s^{−1}$               |
| Velocity residual                          | Vitesse_residu                   | $m.s^{−2}$               |
| Kinetic energy per elements                | Energie\_cinetique\_elem         | $kg.m^{-1}.s^{-2}$       |
| Total kinetic energy                       | Energie\_cinetique\_totale       | $kg.m^{-1}.s^{-2}$       |
| Vorticity                                  | Vorticite                        | $s^{−1}$                 |
| Density                                    | Masse\_volumique                 | $kg.m^{-3}$              |
| Mixture density (Multiphase only)          | Masse\_volumique\_melange        | $kg.m^{-3}$              |
| Pressure in incompressible flow (P/ρ + gz) | Pression                         | $Pa.m^3.kg^{−1}$         |
| Pressure in incompressible flow (P+ρgz)    | Pression\_pa or Pressure         | $Pa$                     |
| Pressure in compressible flow              | Pression                         | $Pa$                     |
| Hydrostatic pressure (ρgz)                 | Pression\_hydrostatique          | $Pa$                     |
| Totale pressure                            | Pression_tot                     | $Pa$                     |
| Pressure gradient                          | Gradient\_pression               | $m.s^{−2}$               |
| Velocity gradient                          | gradient\_vitesse                | $s^{−1}$                 |
| Temperature                                | Temperature                      | $C$ or $K$               |
| Temperature residual                       | Temperature\_residu              | $C.s^{−1}$ or $K.s^{−1}$ |
| Temperature variance                       | Variance\_Temperature            | $K^2$                    |
| Temperature dissipation rate               | Taux\_Dissipation\_Temperature   | $K^2.s^{−1}$             |
| Temperature gradient                       | Gradient\_temperature            | $K.m^{-1}$               |
| Heat exchange coefficient                  | H\_echange\_Tref                 | $W.m^{−2}.K^{−1}$        |
| Turbulent viscosity                        | Viscosite\_turbulente            | $m^2.s^{−1}$             |
| Turbulent dynamic viscosity                | Viscosite\_dynamique\_turbulente | $kg.m.s^{−1}$            |
| Turbulent kinetic                          | Energy                           | $K m^2.s^{−2}$           |
| Turbulent dissipation rate                 | Eps                              | $m^3.s^{−1}$             |
| Constituent concentration                  | Concentration                    | -                        |
| Constituent concentration residual         | Concentration\_residu            | -                        |
| Component velocity along X                 | VitesseX                         | $m.s^{-1}$               |
| Component velocity along Y                 | VitesseY                         | $m.s^{-1}$               |
| Component velocity along Z                 | VitesseZ                         | $m.s^{-1}$               |
| Mass balance on each cell                  | Divergence\_U                    | $m^3.s^{−1}$             |
| Irradiancy                                 | Irradiance                       | $W.m^{−2}$               |
| Q-criteria                                 | Critere_Q                        | $s^{−1}                  |
| Distance to the wall (Y+)                  | Y\_plus                          | -                        |
| Friction velocity                          | U\_star                          | $m.s^{−1}$               |
| Void fraction                              | Alpha                            | -                        |
| Cell volumes                               | Volume\_maille                   | $m^3$                    |
| Source term in non Galinean referential    | Acceleration\_terme\_source      | $m.s^{−2}$               |
| Stability time steps                       | Pas\_de\_temps                   | $s$                      |
| Volumetric porosity                        | Porosite\_volumique              | -                        |
| Distance to the wall                       | Distance\_Paroi                  | $m$                      |
| Volumic thermal power                      | Puissance\_volumique             | $W.m^{-3}$               |
| Local shear strain rate                    | Taux\_cisaillement               | $s^{−1}$                 |
| Cell Courant number (VDF only)             | Courant\_maille                  | -                        |
| Cell Reynolds number (VDF only)            | Reynolds\_maille                 | -                        |
| Viscous force                              | Viscous\_force                   | $kg.m^2.s^{-1}$          |
| Pressure force                             | Pressure\_force                  | $kg.m^2.s^{-1}$          |
| Total force                                | Total\_force                     | $kg.m^2.s^{-1}$          |
| Viscous force along X                      | Viscous\_force\_x                | $kg.m^2.s^{-1}$          |
| Viscous force along Y                      | Viscous\_force\_y                | $kg.m^2.s^{-1}$          |
| Viscous force along Z                      | Viscous\_force\_z                | $kg.m^2.s^{-1}$          |
| Pressure force along X                     | Pressure\_force\_x               | $kg.m^2.s^{-1}$          |
| Pressure force along Y                     | Pressure\_force\_y               | $kg.m^2.s^{-1}$          |
| Pressure force along Z                     | Pressure\_force\_z               | $kg.m^2.s^{-1}$          |
| Total force along X                        | Total\_force\_x                  | $kg.m^2.s^{-1}$          |
| Total force along Y                        | Total\_force\_y                  | $kg.m^2.s^{-1}$          |
| Total force along Z                        | Total\_force\_z                  | $kg.m^2.s^{-1}$          |

# Complete Post-Processing Example

Here is a complete post-processing example taken from the TRUST's `upwind` test case.

    Post_processing
    {
        Probes
        {
            sonde_pression pression periode 0.005 points 2 0.13 0.105 0.13 0.115
            sonde_vitesse vitesse periode 0.005 points 2 0.14 0.105 0.14 0.115
            sonde_vitesse_bis vitesse periode 0.005 position_like sonde_vitesse
            sonde_vitesse_ter vitesse periode 1e-5 position_like sonde_vitesse
        }

        Definition_champs
        {
            # Calcul du produit scalaire grad(Pression).Vitesse #
            pscal_gradP_vit Transformation {
                methode produit_scalaire
                sources {
                    Interpolation { localisation elem source refChamp { Pb_champ pb gradient_pression } } ,
                    Interpolation { localisation elem source refChamp { Pb_champ pb vitesse } }
                }
            }
            # Calcul du produit scalaire Vitesse.Vitesse #
            pscal_vit_vit_elem Transformation {
                methode produit_scalaire
                sources {
                    refChamp { Pb_champ pb vitesse } ,
                    refChamp { Pb_champ pb vitesse }
                }
                localisation elem
            }

            pscal_vit_vit_elem_interp Transformation {
                methode produit_scalaire
                sources {
                    Interpolation { localisation elem source refChamp { Pb_champ pb vitesse } } ,
                    Interpolation { localisation elem source refChamp { Pb_champ pb vitesse } }
                }
            }

            pscal_vit_vit_som Interpolation { localisation som sources_reference { pscal_vit_vit_elem_interp } }

            pscal_vit_vit_faces Interpolation { localisation faces sources_reference { pscal_vit_vit_elem_interp } }

            norme_transfo_vit Transformation {
                methode norme
                source Interpolation {
                    localisation elem
                    source refChamp { Pb_champ pb vitesse }
                }
            }

            vecteur_unitairex transformation { methode vecteur expression 2 1. 0. localisation faces }

            vecteur_transfo transformation { methode vecteur  expression 2 x t localisation faces }

            vecteur_source_elem Transformation {
                methode vecteur  expression 2 pression_natif_dom y
                source refChamp { Pb_champ pb pression }
                localisation elem
            }
            vecteur_source_faces Transformation {
                methode vecteur expression  2 pression_natif_dom y
                source refChamp { Pb_champ pb pression }
                localisation Faces
            }

            compo_vitessex Transformation {
                methode composante numero 0
                source refChamp { Pb_champ pb vitesse }
                localisation elem
            }

            compo_vitessex_elem Interpolation { localisation elem sources_reference { compo_vitessex } }

            # Calcul de la composante selon X du gradient de pression #
            compo_graPx Transformation {
                methode composante numero 0
                source refChamp { Pb_champ pb gradient_pression }
                localisation elem
            }
            # Interpolation de cette composante aux elements #
            compo_graPx_elem Interpolation { localisation elem sources_reference { compo_graPx } }
        }
        Format Lata
        fields dt_post 1.3 /* physical time */
        {
            pression elem
            vitesse elem
            masse_volumique elem
            gradient_pression elem
            pscal_gradP_vit elem
            pscal_vit_vit_elem
            pscal_vit_vit_elem_interp
            pscal_vit_vit_som
            norme_transfo_vit elem
            vecteur_unitairex elem
            vecteur_transfo elem
            vecteur_source_elem
            compo_vitessex_elem
            compo_graPx_elem
        }
    }

    liste_de_postraitements {
        test1 Post_processing {
            Definition_champs {
                test_nom reduction_0D {
                    methode weighted_average
                    sources { refchamp { pb_champ pb pression } }
                }
            }
            format lata
            fields dt_post 1.3
            {
                pression elem
                test_nom
            }
        }

        test2 Post_processing {
            format lata
            fields dt_post 1.3e9
            {
                pression elem
            }
        }
    }
