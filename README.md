# Self-organization-of-a-biogeomorhic-landscape-contorlled-by-plant-life-history-traits
This repositroy provides the vegetation-model code used for the Nature Geoscience publication "Self-organization of a biogeomorphic landscape controlled by plant life-history traits". The code is based on the TELEMAC model environment, for fruther information please refer to http://www.opentelemac.org/.

*****************************
        veg_model.f 
*****************************
The fortran-user-file contains altered fortran subroutines facilitating the incorporation of vegetation-growth in TELEMAC2D.
The modified telemac subroutiunes can be compiled and linked to the exisiting TELEMAC2d model environment by adding it to the keyword FORTRAN FILE, see "cas-example.txt".
For further information please refer to http://opentelemac.com/.

The vegetation module was tested for the release V7P2R0, downloadable on http://opentelemac.com/index.php/download.
The model code is freely available but will not be further maintained, for questions regarding the TELEMAC model environemnt please refer to http://opentelemac.com.
The vegetation model was developed together with Nicolas Claude, Veerle verschoren.


Please find below an overview of the altered subroutines:
Areas altered in the original subroutines are indicated by "!###>CS"
----------------------------
1. Module Vegetation
----------------------------
Defines vegetation parameters used in subroutines below.
The area where vegetation can grow within the model domain is given as the Variable: VEG defined in the Geometry file.
The variable VEG in the Geometry file was defined using freely available blue-kenue-sofware package https://www.nrc-cnrc.gc.ca/eng/solutions/advisory/blue_kenue_index.html.

----------------------------
2. Subroutine DIFSOU
----------------------------
Uses the existing subroutine, which prepares the source terms in the diffusion equation for the trace to model lateral diffusion of vegetation biomass.

Vegetation is modeled as exisitng Tracer (T1), set in the steering-file(see below).

- calculates vegetation growth using a logistic growth-function(Line 280)
- calculates random settlement(Line 289) as expliciat source terms in the diffusion equation (no advection)
- calculates die-off related to velocity and inundation =(Line 356)

----------------------------
3. Subroutine DRAGFO
----------------------------
Uses the existing subroutine, which adds the drag force of vertical strutures in the momentum equation.

The vegetation biomass calculated in DIFSOU is now recalculated in a dragforce created by vegetation, which is then added when solving the momentum equation.

- drag-force calucalution follows the approach of Baptist(2007), using different formulations for emerged and submerged scenarios(line 747)
- drag-force re-calculation is based on chosing the Manning-equation as the law for bottom friction(if a different law is chosen re-calculation needs to be adapted)

----------------------------
3. Subroutine TELEMAC2D
----------------------------

Calculates averaged inundation and max. velocity over the defined coupling period (line 3199, 3325)


*****************************
       cas_example.txt
*****************************
Shows the additional keywords needed in the steering file to activate the vegetation development functions;
The example-cas file does not contain additional model settings, since these need to be set by the user.

VERTICAL STRUCTURES: YES 				--> activates the use of the DRAGFO-subroutine
FORTRAN FILE       :'veg_model.f'			--> activates the Veg_model.f and all including subroutines
VARIABLES FOR GRAPHIC PRINTOUTS   	     : 'T1'	--> saves output of Tracer T1(used as 'Vegetation') in the resuls files

NUMBER OF TRACERS                            : 1	--> adds tracer which will be used for diffusion of vegetation in DIFSOU-subroutine
NAMES OF TRACERS                             :'VEGETATION'
DIFFUSION OF TRACERS                         : YES
ADVECTION OF TRACERS                         : NO
MASS-LUMPING ON H                            : 1.
INITIAL VALUES OF TRACERS                    : 0
COEFFICIENT FOR DIFFUSION OF TRACERS         : user defined


-----------------------------
	References
-----------------------------
Baptist, M. J., et al. "On inducing equations for vegetation resistance." Journal of Hydraulic Research 45.4 (2007): 435-450.
