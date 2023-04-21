---
title: "Post-Processing"
weight: 7
---

Do you know that CFD refers to Colorful Fluid Dynamics ? OK, it is not exactly that but ... it can be !

Moreover, do you know also that the best moments you spend after running a simulation are those where you visualize your results. With TRUST, this is surely possible. However, keep in mind that you should explicitly define what you want to visualize, which fields, probes, statistics, which format and at what frequency. 

All this is done via the TRUST C++ classes: `Post_processing` or `Post_processings` (aliases `Postraitements` or `Postraitements`. The difference between both classes is that the first creats one post-processing object, while the other can create N object if you ask for N post-processing blocs. These keywords should be placed at the end of the `Problem` bloc.

For both cases (whatever which class you use, `Post_processing` or `Post_processings`) you can define the following.

# Probes

This is done by the C++ class `Sonde` or `Sondes` (list of probes). You can request spatial/temporal variation/evolution of any field (should be known by TRUST). The values can be extracted at a single point in space, at a segment of points or in a plane.

**Attention:** The probe coordinates should be given in Cartesian coordinates (X, Y, Z), including axi-symmetrical cases.

# Define New Fields

This is done by the C++ class `Definition_champ`. In this bloc you can create new complex fields for advanced postprocessing.

# Write Fields

This is dome by the C++ class `Fields` or `Champs`. Here you can specify the frequency by the keywor `dt_post`. For example, the following syntax is used to visualize the pressure (at center of the elements) each 1000 seconds.

    Post_processing
    {
        Fields dt_post 1000
        {
            pression elem
        }
    }

It is also possible to precise the format of the output files. This is done by the keywoed `Format` where three formats are available: `Lml`, `Lata` and `Med`. The default format is `Lml`. Results can be visualized by **[VisIt](https://visit-dav.github.io/visit-website/index.html)**, **[Salome](https://www.salome-platform.org/?lang=fr)** (Paravis module), **[Paraview](https://www.paraview.org/)**. It is possible to use **[TecPlot](https://www.tecplot.com/)** too !

**Note:** It is possible to convert files written with the `Lata` format into `Lml` and `Med` format. This is done by the Class `Lata_to_other`. The syntax is the following

	lata_to_other med NOM_DU_CAS NOM_DU_CAS
or 

	lata_to_other lml NOM_DU_CAS NOM_DU_CAS

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
        Format lml
        fields dt_post 1.3
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