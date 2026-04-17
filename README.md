Repository for the research paper, "District-scale life cycle costing of climate-neutral retrofits, based on automatic envelope detection workflows and LOD3 3D city model".

Journal: Energy & Buildings, Vol. 359, 15 May 2026.
Authors: Suppa, A.R., Bottero, M.C., Corrado, V., & Dell'Anna, F.
DOI: [https://doi.org/10.1007/s12273-025-1301-3](https://doi.org/10.1016/j.enbuild.2026.117289)

This repo provides a custom Grasshopper script (used within Rhino3d software) to build roof geometry, by first segmenting building footprints into those with four or more than four vertices. The latter are L-shapes or complex building shapes, while the former are simple square or rectangle shapes. The script then builds roof surfaces as follows:
1. For hip roofs with 4 vertices, these are further split into those that are approximately square (with a side-to-side ratio within range 0.9 to 1.1), and those that are not within the squareness tolerance.
   a. For the square hip roofs, these do not require a roof ridgeline to approximate the roof surfaces, but rather require four triangles which arrive at the center of the square/rectangle. The center is first transposed upward to its correct height, according to GIS/LiDAR data, and the triangle shapes representing the hip slopes are created directly using Grasshopper surface tools.
   b. For the non-square hip roofs with four vertices, a roof ridge is approximated using the Dragonfly tool “straight skeleton”, producing a tree-branch-like polyline within each footprint. This tool follows the algorithm of Felkel & Obrdrzalek (1998) and is intended to create thermal zones for energy models, though we observe that the central portion of the straight skeleton approximates a building’s roof ridgeline. Accordingly, our script isolates the central polyline by detecting line segments connected to the building footprint, as depicted below. The ridge polyline is transposed upward using the eaves-to-peak height from GIS/LiDAR data, and the four triangle shapes are created directly using Grasshopper surface tools.
2. For hip roofs with greater than four vertices, the process is nearly identical as (1b) above, except that the complex roof shapes are approximated using the "Patch Surface" tool within Grasshopper.
3. For gable roofs, both with four and greater than four vertices, the process is very similar, though we first extend the approximated ridge to the building outline before transposing upward. Then, for gable roofs with four vertices we use Grasshopper surface tools to directly obtain the gable slope shapes.
4. For gable roofs with greater than four vertices, the complex shape is again approximated using the "Patch Surface" tool within Grasshopper. <br>

<img width="823" height="540" alt="image" src="https://github.com/user-attachments/assets/e9fb073d-7f6f-45ff-b814-ff270840a763" /><br>


The tool was used within the above-referenced article primarily to derive roof surface areas for retrofit quantity estimation, for incorporation in a life cycle costing model. Because of this focus on retrofit costs, we note that the triangle-shaped ends of gable roofs are actually part of the exterior wall, so these must be separated from the gable roof calculation, as there are different unit costs associated with roof and wall retrofits. For the 4-vertex gable roofs, the triangle-shaped ends are not included in the roof area, and these areas are separately added to wall area calculations during post-processing. For the gable roofs with greater than 4 vertices, we approximated the triangle-shaped ends using GIS data and subtracted these from the gable roof areas during post-processing.

The tool could be further developed to incorporate the derived 3D roof geometries into energy models.


Requirements:
1. [Rhino software](https://www.rhino3d.com/)
2. Urbano plug-in](https://urbano.io) - used to import SHP files with geometry and metadata from GIS.
3. [Dragonfly plug-in](https://www.ladybug.tools/dragonfly.html) - used for the "Straight Skeleton" tool to approximate roof ridgelines.
