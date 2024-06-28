# Optogenetic_PostHoc_Validator

This is a GUI-assisted tool for post-hoc quantification of brain areas which expressed fluorescent construct AND were illuminated by light (or were the souurce of collected light) during in-vivo optogenetic or imaging experiments. It is a modification of previously described tool for IgroPro (see Baleisyte et al., 2022, Cell Reports; https://doi.org/10.1016/j.celrep.2022.110850). The current version written in Jython for ImageJ/FIJI has improved functionality and is more user-friendly. 

Functionality:

 - accepting TIFF stacks (RGB or multi-channel) containing images of the post-hoc brain sections
 - fitting the cylindrical model(s) of >=1 optic fiber(s) into the fiber tracks using the GUI-controlled X-Y-Z translations and rotations 
 - estimation of the illuminated brain tissue volume below the fiber tip ("light cone") based on the fiber parameters (see Aravanis et al., 2007 J Neural Eng; https://doi.org/10.1088/1741-2560/4/3/S02)
 - determination of the brain areas expressing fluorescent optogenetic construct by intensity thresholding
 - calculation of the overlap areas between the expression area, the illuminated area, and the anatomical ROIs (e.g. from an atlas)
 - resulting overlap areas are stored in ImageJ RoiManager format
 - all the parameters/configuration are stored in the .json project file
 
 Prerequisites:
 
 - as the input, needs an aligned stack of brain images (highly recommended is atlas registration using ABBA tool, see: https://github.com/BIOP/ijp-imagetoatlas)
 - atlas outlines can be contained in the input TIFF stack as an overlay (e.g. as exported by the latest ABBA versions), or can be loaded via ImageJ RoiManager
 - image stack scaling such as pixel dimensions, units, and pixel origins for X, Y, Z dimensions have to be set properly
 
 Installation:
 
 - the main script "fibers_cones.py" can be directly opened in the ImageJ script editor and run directly from there
 - a compiled .jar file for the plugin will be co-supplied in the future
 
 Usage:
 
 - GUI is largely self-explanatory
 - a project file (.json) storing the model details and customization settings can be saved and re-loaded
 - to apply the preferred settings from start, one could use a pre-existing .json file as a template. One only needs to manually change the path and the name of the image stack in the text editor, and to reset the thresholding flag to "False"
 - the original image stack is never over-written. To store the graphical output as is, it can be "Saved as" from the ImageJ menu. Alternatively, overlay can be exported into ImageJ RoiManager (Image->Overlay->To ROI Manager)
 - pixel size and units of the image stack have to be set properly (ImageJ->Image->Properties) before using the tool. X and Y values as well as origins are usually pre-set by the export routine in ABBA; Z-step has to be manually checked. Please pay attention to the textual info about the image dimensions at the bottom of the GUI. It may be more convenient to set Z-origin value to 0.0 in ImageJ->Image->Properties. If you do not see the fiber image right away, please check the coordinates entered in the GUI vs the pixel offsets (see the next point)
 - initial guesses for X-Y-Z translational coordinates of the fiber tips can be easily made by pointing at the image with a cursor: ImageJ will report the  coordinates in the status line. These can be directly entered into the respective GUI fields (but mind the units!)
 - the surfaces of the fiber(s) and of the light cone(s) are rendered by populating them with randomly scattered dots at specific density (parameter "Fiber rendering dpi", in dots/μm2). To save time, it is recommended to position the fibers at low settings (such as default 0.02), and when satisfied with positioning, re-calculate high resolution output at higher settings (e.g. 0.1-0.15/μm2)
 - another parameter relevant for rendering the fiber and the "light cone" cross-sections is "Tolerance along Z" (in % of Z-step distance), changing which will affect (improve or degrade) the quality of the outlines. Make small changes at a time, if needed
 - after analysis is complete and all overlays are computed (see Menu "Analysis"), three Python scripts (threshold_vs_atlas_processing.py, cones_vs_atlas_processing.py, and cones_vs_threshold_vs_atlas_processing.py) can be used as a starting point to further analyse the .csv output files of the main script for the overlays of thresholded areas or the light cones with the atlas ROIs, and for the overlays of thresholded & illuminated areas with the atlas ROIs, respectively. The scripts calculate the summed and normalized results over all the planes of the image stack, batching over multiple image stacks
