# MIFoS Simulation Data
This repository contains simulation data from Computational Fluid Dynamics simulations on programmatically altered geometries. Each row contains the surface data and key results corresponding to a single pixel change for a given geometry.  This dataset was created with the goal of being able to use surface data to predict changes in drag/lift due to geometric modifications.

The paper written that details this work, *Machine Learning Driven Interpretation of Computational Fluid Dynamics Simulations to Develop Student Intuition*, was submit to the [Computer Applications in Engineering Education](https://onlinelibrary.wiley.com/journal/10990542) journal on May 25, 2019 and is under review. 

The remainder of the readme file details the creation of this data set.

## Shape Generation
Thirty-one ‘core’ geometries were identified as options for providing a diverse set of fluid dynamics simulations and resultant surface data.  Each shape was created by using specific vertex locations (polygons), or equations to define vertices, that were then connected using a series of lines.  All shape heights were normalized so that they shared a characteristic length scale and were then converted into 2D geometry black and white images programmatically. Normalizing the shape height prior to image creation also meant that all shapes had min and max pixel locations that corresponded to the bottom and top of their respective images.  The core geometries included circles, ovals, polygons (3-9 sides), rhomboids, parallelograms, parabolas (both filled and empty), squircles, teardrops, and naca airfoils, all of which can be seen in the following figure.  A maximum length of 5x that of the characteristic height was chosen for all geometries.

![Thirty-one core geometries](/images/Figure-A-1.jpg)

**Figure 1: Thirty-one core geometries used as starting point to create diverse simulation results.**

To further expand on the available geometries and broaden the expected range of surface results, core geometries were rotated, relative to the fluid flow direction, providing additional unique geometries. Each core geometry was rotated to multiple angles, between 2 and 7 angles depending on the geometry, resulting in a total of 164 different geometries relative to the direction of the fluid flow. This rotation can be seen in Figure 2 for core geometries 3 and 29.  Core geometry 3, the triangle, was rotated a total of 3 times while core geometry 29 was rotated a total of 6 times.  While there is no discernible change in size for the triangle shape, the airfoil becomes considerably smaller when rotated vertically.  This is an artifact of maintaining a single uniform height across all simulated geometries; the goal being to maintain the same Reynolds number for all simulations.  

![Rotated core geometries](/images/Figure-A-2.jpg)

**Figure 2. Rotated core geometries 3 (Left) and 29 (Right). Inverted color used for clarity.**

The thirty-one core geometries and all of their rotations provided the comparison cases for the data set, specifically, hundreds of pixel variations were derived from each main shape. A pixel variant was created through the addition or subtraction of a single pixel from one of the core geometries or their rotations.  Figure 3 illustrates a two pixel variant cases derived from Geometry 29, a NACA airfoil.  The addition of a pixel to a geometry means that one pixel along the outer surface becomes part of the geometry.  The subtraction of a pixel from a geometry means that one pixel along the inner surface becomes part of the flow domain. 

![Pixel variant creation](/images/Figure-A-3.jpg)

**Figure 3. Pixel variants created from adding a pixel and subtracting a pixel from the original geometry.  Black is part of the flow domain, white is part of the object geometry.**

Each surface pixel, both interior and exterior, was inverted and saved resulting in hundreds of ‘pixel variants’ for each rotated core geometry.  43,418 pixel variants were created based on the simple addition or subtraction of pixels along the surface of each core geometry or one of its rotations. Special care was taken to not change pixels on the top or bottom of the shape so that the characteristic height of each shape was uniform across all of the core geometries and their pixel variants.  Figure 4 illustrates all of the added (green) or subtracted (red) pixel options for core geometry 29.  Additional considerations were made to ensure that separate islands or shapes were not created by subtracting pixels, meaning that all geometries and pixel variants used were each a single island/blob of connected pixels.

![Pixel variant examples](/images/Figure-A-4.jpg)

**Figure 4. Rotated Core geometry 29 - angle 2 (Left), and all 317 of the Pixel Variant options shown as red (subtract) or green (add) pixels (Right).**

After creating all geometries and pixel variants, each resulting image was processed to create outlines that could be used to imprint into a CFD flow domain.  Shapes were outlined as represented in their respective images, with the possibility of creating all sides exposed to black pixels (see Figure 5).  Additionally, a secondary set of outlines were created that were offset from the actual pixel outline by 0.0001m.  This was done to allow for calculation and output of integrated solution variables from Ansys Fluent as not all flow integrated solution variables are available directly on a surface. Thus, by integrating solution variables along lines slightly offset from the original surface lines (offset in the respective face’s normal direction) it maximized the number of CFD solution variables that could be added to the data set.

![Pixel variant outline](/images/Figure-A-5.jpg)

**Figure 5. The resulting outline from the original Core geometry 29 image.**


## Simulation information
Each pixel variant was centered flow domain (see eventually linked paper for geometric convergence study) which was discretized with a programmatically generated mesh (see eventually linked paper for mesh convergence study). Each meshed geometry was then used to run an external flow simulation with a Reynolds number of 1. This was achieved by using density, viscosity, inlet velocity, and shape height values of 1. 

## Dataset compilation
To create a single matrix for testing and testing purposes, the data obtained from running all of the aforementioned pixel variant simulations was compiled and each formatted into a row vector format.  The following data was collected for each pixel variant
1. Boolean signifying if the pixel for the current case was added or subtracted
1. Four booleans representing which pixel faces were exposed to fluid flow (see Figure E-1)
1. Core geometry or rotated geometry drag components (x-y-z) corresponding to
  1. Pressure drag
  1. Viscous drag
  1. Total drag 
1. 27 integrated variables for 
  1. Face 1 (if exposed to fluid flow in original core or rotated geometry)
  1. Face 2 (if exposed to fluid flow in original core or rotated geometry)
  1. Face 3 (if exposed to fluid flow in original core or rotated geometry)
  1. Face 4 (if exposed to fluid flow in original core or rotated geometry)
1. Pixel variant drag/lift-delta components (x-y-z) corresponding to
  1. Pressure drag difference from original geometry
  1. Viscous drag difference from original geometry
  1. Total drag  difference from original geometry

![Pixel variant examples](/images/Figure-E-1.jpg)

**Figure 6. Populated surface data (or lack thereof) based on face exposure.**

Examining Figure 6, the red pixel was a subtraction change to a core geometry. This means that the core geometry included this pixel, but the resulting pixel variant geometry did not.  The green pixel was an addition change to the same core geometry meaning that this pixel was not included in the core geometry, but was part of the resulting pixel variant.  The face data that was populated for each pixel variant data vector only included surface data for external pixel faces of original core or rotated geometries - the same faces exposed to the fluid flow around the core or rotated geometry. Thus the entry for the red (subtracted) pixel in Figure 6 had data values for pixel faces 1 & 2, but no data for faces 3 & 4. The entry for the green (added) pixel in Figure 6 was the exact opposite; populated data for faces 3 & 4, but no data for faces 1 & 2. 


## Missing data

Not all of the simulations were completed for various reasons (mainly random errors from Ansys Meshing and Fluent).  These rows have NA entries for all of their data and should be ignored.


