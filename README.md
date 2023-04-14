# Optogenetic_PostHoc_Validator

This is a GUI-assisted package for post-hoc quantification of brain areas which expressed optogenetic construct AND were illuminated by light (or were the souurce of collected light) during in-vivo experiments. It is a modification of previously described IgorPro tool (see Baleisyte et al., 2022, Cell Reports; https://doi.org/10.1016/j.celrep.2022.110850). The current version written in Jython for ImageJ/FIJI has improved functionality and is more user-friendly. 

Functionality:

 - accepting TIFF image stacks (RGB or multi-channel) containing images of the post-hoc brain sections with the optic fiber tracks
 - fitting the cylindrical model(s) of >=1 optic fiber(s) into the visible fiber tracks using the GUI-specified X-Y-Z translations and rotations 
 - estimation of the illuminated brain tissue volume below the fiber tip ("light cone") based on the fiber parameters (as described in Aravanis et al., 2007 J Neural Eng; https://doi.org/10.1088/1741-2560/4/3/S02)
 - determination of the brain areas expressing optogenetic construct using interactive intensity thresholding
 - calculation of the overlap areas between the expression area, "light cone" illuminated area, and the atlas ROIs, in all combinations
 - all generated regions of interest such as thresholding results or results of overlap are stored in ImageJ RoiManager format
 - all the parameters/configuration are stored in the .json project file
 
 Prerequisites:
 
 - as the input, needs a pre-aligned or atlas-registered stack of brain images (registration using ABBA tool is recommended, see https://github.com/BIOP/ijp-imagetoatlas)
 - atlas outlines can be contained in the TIFF stack as overlay (as directly exported in the latest ABBA versions), or loaded via ImageJ RoiManager
 - image stack scaling such as pixel dimensions, units, and pixel origins for X, Y, Z dimensions are directly considered
 
 Installation:
 - the main script "fibers_cones.py" can be directly opened in the ImageJ script editor and executed from there
 - a compiled .jar file for the plugin will be co-supplied in the future
 
 Usage:
 - GUI is largely self-explanatory
 - a project file (.json) keeping all the details and customization can be newly created, saved, and re-loaded
 - to keep the preferred settings from start, one could use a pre-existing .json file as a template. One only needs to manually change the path and the name of the image stack, and to reset the thresholding flag to "False"
 - the original image stack is never over-written. To store the graphical output, the stack can be "Saved as" from the ImageJ menu, or, alternatively, overlay can be exported via the RoiManager (Image->Overlay->To ROI Manager) in ImageJ
 - Image dimsnsion, pixel size and units have to be set properly (ImageJ->Image->Properties) before using the tool. X and Y values as well as origins are usually pre-set by the export routine from ABBA; Z-step has to be manually checked. Pay attention to the textual info about the image dimensions below the GUI. It may be more convenient to set the Z-origin value to 0.0 in ImageJ->Image->Properties dialog, but leave X and Y origins as they are. If you do not see the fiber image right away, check the coordinates entered in the GUI vs the pixel offsets (see the next point below).
 - X-Y-Z translational coordinates for the fibers can be easily guessed by pointing the assumed fiber tip locations with the ImageJ cursor on the images. ImageJ reports the cursor coordinates in the status line; these (watch the units!) can be directly used for entering into the respective GUI fields
 - the surfaces of the fiber(s) and of the light cone(s) are rendered by populating them with dots at specific density ("Fiber density dpi" in dots/mm2). To save computational time, it is highly recommended to position the fibers at low settings (such as default 0.02), and when done, re-calculate high resolution output at higher settings (e.g. 0.1-0.15)
 - another parameter relevant for rendering the fiber and the "light cone" cross-sections is "Tolerance along Z", changing which may affect (improve or degrade) the quality of the outlines. Make small changes at a time, if needed.
 - after analysis is complete and all overlays are computed (see Menu "Analysis"), two provided Python scripts (threshold_vs_atlas_processing.py and cones_vs_threshold_vs_atlas_processing.py) can be used as the starting point to further analyse the output .csv files of the main script, for the overlays of thresholded areas with atlas, and for the overlays of thresholded & illuminated areas with the atlas ROIs, respectively. The scripts produce the summed and normalized results over all the planes of the image stack and allow for batch-processing over multiple projects. 
