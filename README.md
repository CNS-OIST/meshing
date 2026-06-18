# Mesh utility collections for neuron morphology reconstruction

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
    - Main Output: `.swc` morphology file where branch intersections are labeled using user-defined tags
* [SWCTetMesher](https://github.com/CNS-OIST/SWCTetMesher): Tetrehedral mesh generator from SWC morphology data.
    - Main Input: `.swc` morphology file
    - Main Output: `.mesh` tetrahedral mesh file of the morphology. (Optional: `.off` surface mesh of the morphology)

### For surface mesh morphology
* [CollisionResolver](https://github.com/CNS-OIST/CollisionResolver): Detect and resolve overlaps between watertight surface meshes by voxel ownership, so densely-packed segmented organelles become non-overlapping (gap-separated) and meshable. Also checks/repairs non-manifold inputs.
    - Main Input: `.off` watertight surface meshes (possibly overlapping/non-manifold).
    - Main Output: `.off` non-overlapping, gap-separated watertight surface meshes.
* [MultiCompMesher](https://github.com/CNS-OIST/MultiCompMesher): Multi-component mesh generation and labeling from watertight surface boundaries.
    - Main Input: 
        1. `.off` watertight surface boundary meshes.
        2. user-defined text file with component signatures.
    - Main Output: `.mesh` Tetrahedral mesh file of the morphology with labeled components.

### For STEPS tetrahedral meshing
* [STEPSMeshPipeline](https://github.com/CNS-OIST/STEPSMeshPipeline): End-to-end pipeline turning segmented surfaces into a labelled tetrahedral mesh sized and validated to the STEPS mesh criteria (Hepburn et al. 2012). Resolves overlaps automatically (bundles CollisionResolver) and tetrahedralises (wraps MultiCompMesher), then exports Gmsh `.msh` and a mesh-quality report.
    - Main Input: 
        1. `.off` segmented surface boundary meshes (overlaps resolved on the fly).
        2. reaction-diffusion parameters (rate `k`, diffusion `D`, concentration) for STEPS-criteria sizing — or typical neuroscience defaults.
    - Main Output: Gmsh `.msh` labelled tetrahedral mesh meeting the STEPS size/shape criteria, plus a quality report (figures + verdict).

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
* [GAMer](https://github.com/ctlee/gamer): surface mesh improvement library developed to condition surface meshes derived from noisy biological imaging data. Also provides tetrahedral mesh generation functionality using TetGen backend.
    - Main Input: surface mesh files
    - Main Output: repaired and optimized surface meshes
* [Gmsh](http://gmsh.info/doc/texinfo/gmsh.html): Tetrahedral mesh generator from CAD geometry or surface boundaries.
    - Main Input: surface morphology file
    - Main Output: `.msh` tetrahedral mesh file, can also export to other formats
* [TetGen](http://wias-berlin.de/software/tetgen/): Tetrahedral mesh generator from polyhedral surface boundaries.
    - Main Input: surface morphology file
    - Main Output: text-based tetrahedral mesh file
* [fTetWild](https://github.com/wildmeshing/fTetWild): Robust floating-point tetrahedral meshing from triangle surfaces / soups, tolerant of imperfect input (self-intersections, gaps) via a user-set envelope, with element-quality optimization. Supersedes the original [TetWild](https://github.com/Yixin-Hu/TetWild); Python bindings via the `wildmeshing` package.
    - Main Input: surface mesh file (or triangle soup)
    - Main Output: tetrahedral mesh (Gmsh `.msh` / medit `.mesh`).
* [VolRoverN](https://cvcweb.oden.utexas.edu/cvcwp/software/volumerover-neuron/): Pipeline solution for neuron reconstruction from `RECONSTRUCT` series files to surface/volume mesh. Also provides automatic surface mesh repairing and optimization functionality.
    - Main Input: `RECONSTRUCT` series files, or `.off` surface mesh
    - Main Output: surface/volume meshes of the reconstruction

### Mesh format conversion
* [meshio](https://github.com/nschloe/meshio): Python library and command-line tool to read, write, and convert between many mesh formats — Gmsh `.msh`, medit `.mesh`, VTK/VTU, `.off`/`.obj`/`.stl`/`.ply`, Abaqus `.inp`, XDMF, and more. (Used by STEPSMeshPipeline for the `.mesh` → Gmsh `.msh` conversion.)
    - Main Input: a mesh file in any supported format
    - Main Output: the same mesh in another supported format (`meshio convert in.ext out.ext`)

### Mesh processing & repair (Python libraries)
General-purpose libraries for loading, analysing, repairing, and visualising
meshes (rather than morphology-specific tools).
* [trimesh](https://github.com/mikedh/trimesh): Triangle-mesh library — load/export many formats, watertightness & winding checks and repair (merge vertices, drop degenerate/duplicate faces, fix normals/winding, fill holes), voxelisation, ray/proximity queries, and booleans. (Used by CollisionResolver.)
* [PyVista](https://github.com/pyvista/pyvista): Pythonic VTK interface for mesh processing, analysis, and 3D visualisation — clipping/slicing, collision detection, decimation, and plotting. (Used by CollisionResolver for collision detection and figures.)
* [PyMeshFix](https://github.com/pyvista/pymeshfix): Python wrapper of MeshFix — repair non-manifold / non-watertight surface meshes (remove self-intersections and degeneracies, fill holes) into a clean watertight manifold.
* [PyMeshLab](https://github.com/cnr-isti-vclab/PyMeshLab): Python bindings to [MeshLab](http://www.meshlab.net/) — extensive mesh filters including isotropic remeshing, cleaning, hole filling, and repair.

