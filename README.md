# LibShell

This code implements the discrete shell energy, and its derivatives and Hessian.

## Kinematics

The shell's current pose is represented as a triangle mesh, as well as, optionally, some per-edge normal directors. The rest state is specified with per-face first and second fundamental forms; these can be computed from the current pose, or from a separate mesh (with identical combinatorics) in a rest pose, or procedurally specified, etc.

## Bending Options

Three options are implemented for how the bending energy is discretized, all based on Grinspun et al's discrete shape operator:

* MidedgeAngleSinFormulation: bending energy is roughly sin(theta/2) for edge turning angle theta.

* MidedgeAngleTanFormulation: energy is roughly tan(theta/2) instead. The main difference of this formulation from the previous one is that the bending energy diverges for 180-degree bent hinges.

* MidedgeAverageFormulation: eschews the normal directors of Grinspun et al completely, instead assuming that the normal direction on an edge is always the mean of the neighboring face normals.

For more details see:

* Grinpsun et al "Computing discrete shape operators on general meshes"; 

* Weischedel et al "A discrete geometric view on shear-deformable shell models";

* Chen et al "Physical simulation of environmentally induced thin shell deformation".

## Material Model

Both a St. Venant-Kirchhoff and Neo-Hookean material model are implemented; you select these independently of the second fundamental form discretization by passing in a `MaterialModel` to the elastic energy computation. Each material model assumes uniform Lamé parameters over the entire surface (but you can specify different thicknesses for each triangle). 

See the example program for the formulas that convert Young's modulus and Poisson's ratio to Lamé parameters. Note that the 2D formulas are *not* the same as the 3D ones found on e.g. Wikipedia.

## Compile

Compile this project using the standard cmake routine:

    mkdir build
    cd build
    cmake ..
    make

This procedure will build both the library and an example program, which performs a few iterations of a static solve on an rest-flat bunny mesh.

## Dependencies

The library itself depends only on Eigen (set the environment variable `EIGEN3_INCLUDE_DIR` to point to your Eigen folder). The example program includes a viewer which uses libigl.

## Compiling on Windows

Due to poor interoperation of the Eigen library with the MSVC compiler, Release mode compilation of the derivative code on Windows can take forever (over 8 hours). To solve this issue add EIGEN_STRONG_INLINE=inline to your preprocessor macros when building libshell.
