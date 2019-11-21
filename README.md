# MIFoS Simulation Data
This repository contains simulation data from Computational Fluid Dynamics simulations on programmatically altered geometries. Each row contains the surface data and key results corresponding to a single pixel change for a given geometry.  This dataset was created with the goal of being able to use surface data to predict changes in drag/lift due to geometric modifications.

The paper written that details this work, *Machine Learning Driven Interpretation of Computational Fluid Dynamics Simulations to Develop Student Intuition*, was submit to the [Computer Applications in Engineering Education](https://onlinelibrary.wiley.com/journal/10990542) journal on May 25, 2019 and is under review. 

The remainder of the readme file details the creation of this data set.

## Shape Generation
Thirty-one ‘core’ geometries were identified as options for providing a diverse set of fluid dynamics simulations and resultant surface data.  Each shape was created by using specific vertex locations (polygons), or equations to define vertices, that were then connected using a series of lines.  All shape heights were normalized so that they shared a characteristic length scale and were then converted into 2D geometry black and white images programmatically. Normalizing the shape height prior to image creation also meant that all shapes had min and max pixel locations that corresponded to the bottom and top of their respective images.  The core geometries included circles, ovals, polygons (3-9 sides), rhomboids, parallelograms, parabolas (both filled and empty), squircles, teardrops, and naca airfoils, all of which can be seen in the following figure.  A maximum length of 5x that of the characteristic height was chosen for all geometries.

![GitHub Logo](/images/Figure-A-1.jpg)