# Utilities Collections for reconstruction of neuron morphology

## A Quick Lookup Table: What tool should I use to open `.xxx` file?
|Format|Content|View/Edit Tool|
|:---:|:---:|:---:|
|`.swc`|Point tracing morphology data|[neuTube](https://www.neutracing.com/)|
|`.nrn`|Instructions for NEURON morphology construction|[NEURON](https://neuron.yale.edu/neuron/)|
|`.stl`|Triangular surface mesh|[Meshlab](http://www.meshlab.net/)|
|`.off`|Polygonal surface mesh|[Meshlab](http://www.meshlab.net/)|
|`.obj`|Polygonal surface mesh|[Meshlab](http://www.meshlab.net/)|
|`.ply`|Polygonal surface mesh|[Meshlab](http://www.meshlab.net/)|
|`.blend`|Surface model constructed using Blender|[Blender](https://www.blender.org/)|
|`.geo`|Gmsh geometry file|[Gmsh](http://gmsh.info/doc/texinfo/gmsh.html)|
|`.msh`|Tetrahedral volume mesh|[Gmsh](http://gmsh.info/doc/texinfo/gmsh.html)|
|`.mesh`|Finite element mesh|[Gmsh](http://gmsh.info/doc/texinfo/gmsh.html)|
|`.poly`|TetGen geometry|[TetGen](http://wias-berlin.de/software/tetgen)|
|`.smesh`|TetGen geometry|[TetGen](http://wias-berlin.de/software/tetgen)|

## Mesh utilities From OIST
A collection of meshing utilities for neuron morphologies, developed by [the Computational Neuroscience Unit at Okinawa Institute of Science and Technology Graduate University, Japan](https://groups.oist.jp/cnu).

### For SWC morphology
* [SWCIntersectDetect](https://github.com/CNS-OIST/SWCIntersectDetect): Branch intersection detection for SWC/NEURON morphology.
    - Main Input: `.swc` morphology file
    - Main Output: `.swc` morphology file where branch intersections are labelled using user-defined tag
* [SWCTetMesher](https://github.com/CNS-OIST/SWCTetMesher): Tetrehedral mesh generator from SWC morphology data.
    - Main Input: `.swc` morphology file
    - Main Output: `.mesh` tetrahedral mesh file of the morphology. (Optional: `.off` surface mesh of the morphology)

### For surface mesh morphology
* [MultiCompMesher](https://github.com/CNS-OIST/MultiCompMesher): Multi-component mesh generation and labelling from watertight surface boundaries.
    - Main Input: 
        1. `.off` watertight surface boundary meshes.
        2. user-defined text file with component signatures.
    - Main Output: `.mesh` Tetrahedral mesh file of the morphology with labelled components.

### For STEPS mesh annotation
* [polyhedronROI](https://github.com/CNS-OIST/STEPS_PolyhedronROI): Create Region of Interest (ROIs) annotations in [STEPS](http://steps.sourceforge.net/) using watertight surface boundaries and labeling signatures.
    - Main Input: 
        1. `.off/.stl/.obj` watertight surface boundary meshes.
        2. `stepe.geom.Tetmesh` object from `STEPS`.
    - Main Output: The same `stepe.geom.Tetmesh` object with ROI added.

## Mesh utilities from other parties

### For SWC morphology
* [AnaMorph](https://github.com/NeuroBox3D/AnaMorph): Framework for creating 3d neuronal morphologies from point/diameter descriptions.
    - Main Input: `.swc` morphology file
    - Main Output: `.obj` watertight, manifold surface mesh of the morphology
* [CTNG](https://senselab.med.yale.edu/ModelDB/showmodel.cshtml?model=146950#tabs-2): A tool for constructing a 3d tesselation of a neuron's surface from
point-diameter data.
    - Main Input: `.swc` morphology file
    - Main Output: text-based file with watertight, manifold surface mesh of the morphology

### For surface mesh morphology
* [GAMer](https://github.com/ctlee/gamer): surface mesh improvement library developed to condition surface meshes derived from noisy biological imaging data. Also provide tetrahedral mesh generation functionality using TetGen backend.
    - Main Input: surface mesh files
    - Main Output: repaired and optimized surface meshes
* [Gmsh](http://gmsh.info/doc/texinfo/gmsh.html): Tetrahedral mesh generator from CAD geometry or surface boundaries.
    - Main Input: surface morphology file
    - Main Output: `.msh` tetrahedral mesh file, can also export to other formats
* [TetGen](http://wias-berlin.de/software/tetgen/): Tetrahedral mesh generator from polyhedral  surface boundaries.
    - Main Input: surface morphology file
    - Main Output: text-based tetrahedral mesh file
* [TetWild](https://github.com/Yixin-Hu/TetWild): Robust Tetrahedral Meshing from polyhedral  surface boundaries.
    - Main Input: surface mesh file
    - Main Output: `gmsh` tetrahedral mesh.
* [VolRoverN](https://cvcweb.oden.utexas.edu/cvcwp/software/volumerover-neuron/): Pipeline solution for neuron reconstruction from `RECONSTRUCT` series files to surface/volume mesh. Also provide automatic surface mesh repairing and optimization functionality.
    - Main Input: `RECONSTRUCT` series files, or `.off` surface mesh
    - Main Output: surface/volume meshes of the reconstruction

