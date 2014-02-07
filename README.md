hydrolp-a
=========

This is a linear programming optimization model of the operations and storage of a small part of the Manitoba Hydro generation system. The model was written in the zimpl modelling language (http://zimpl.zib.de/) and I'm publishing it here under the MIT license. This software comes with no guarantees and no claims that it is fit for any purpose.

The MBH system is fed from the Nelson, Saskatchewan, Churchill, and Red Rivers (see schematic) but the largest storage is Lake Winnipeg. The plants with the largest power capacity are on the Lower Nelson River (feeding into Hudson's Bay) and are on a long DC link which can have more appreciable losses in hot seasons. There are some tricky hydraulic and operational bits around JPEG (the outlet of Lake Winnipeg) and the East Channel, and of course operations are trickier still with the appearance of surface and frazil ice. Of course, this little model doesn't consider those detailed operational issues...

This model basically is a mass-balance network model with storage at some nodes and convolution delays (using linear unit impulse response functions) on some channels (all open channel flow). To stay linear, all plants have fixed H/K factors (ie mw/kcfs does not change with head) and all storage constraints are linear (represented in ft, after conversion from volume). There are no large heads in this system so the linear storage assumption may be fair - there are likely larger nonlinearities in the turbine curves (ie the fixed H/K assumption), but this demo model doesn't try any tricks to deal with that...

This is an LP with a minimize-spill objective. It would be nice to make it a QP and maximize energy or revenue, but directly using a quadratic objective in lp_solve may be tricky. I may revisit this in the future. If you can make this model QP and work with lp_solve drop me a note or pull-request.

The model is hardcoded to load the historical streamflow record from specific columns of the historical.csv file. The model loads the whole time horizon and determines optimal operations for the objective function and constraints, this means that if you add more data it will likely take longer to produce a soution. You could alternately change the data to be for a different timestep-size and make any other asociated timestep-integration changes in the model to deal with longer time horizons.

Beginer's instructions that you can copy-paste into a Ubuntu terminal window:

* sudo apt-get install zimpl                            # install the zimpl modelling language
* sudo apt-get install lp-solve                         # install the solver
* zimpl -t mps MBHModel.zpl                             # convert the model into mps format
* lp_solve -fmps -presolve MBHModel.mps > MBHModel.sln  # solve the model (takes some time)

The results are in the MBHModel.sln file as a pair of columns for variable name vs value. It might take some reworking to get the output into an easier form to review, but that's part of a modelling system toolchain, not covered here.

I hope you have as much fun exploring this model as I've had making it. Cheers!

orthopteroid
