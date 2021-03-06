.. _user-example:

User model example
==================

The myusermodel.py file in the examples directory contains a basic program
that interacts with the user or musr model in SPEX. It reads the input energy
grid and model parameters from SPEX, calculates a spectrum or transmission, and
returns the calculated values by writing a text file. The user can adapt this
example to his/her own needs.


**Imports**

The following imports are mandatory::

  import math                         # Import math module
  import numpy                        # Import numpy module
  from pyspextools.model import User  # Import the user module from pyspextools.model


**Initialisation**

The user module from pyspextools.model needs to be initialized first. This will read in
the input file from SPEX automatically and initialize the arrays::

    usr=User()
 
 
**Calculation**

This step needs the creativity of the user. The module reference page :ref:`usermodel` provides an overview of the
parameters of the user module that can be used in the program. At least, the output spectrum or transmission
needs to be calculated and written to the 'usr.sener' output array. This array will be passed back to SPEX after
the calculation::

    # For each energy bin in the provided energy grid
    # calculate the spectrum in photons/s/bin:
    for i in numpy.arange(usr.neg):
      usr.sener[i]=1.- usr.par[0]*math.exp(-usr.eg[i])
      usr.wener[i]=0.
      
In this case, a simple exponential function is returned as an example, but this
function can be as simple or complicated as needed. The unit for the 'usr.sener' array is
'photons/s/bin' and this example function also uses the first model input parameter 'user.par[0]'
from SPEX and the bin energy `usr.eg` (see :ref:`usermodel` for a full description).

    **NOTE:** There is a difference between the index of the array and the parameter number in SPEX.
    The parameter number in SPEX is equal to the array index + 1, since Python starts counting
    at 0 and SPEX at 1.

The 'usr.wener' array can be used to optimize the calculations in SPEX.
If Delta E = average photon energy within the bin (keV) minus the bin 
centroid then wener = sener * Delta E.


**Write output**

The result needs to be handed back to SPEX through a text file, which can be
written by calling the 'usr.write_spc()' function::

    # Write the calculated spectrum to the output file: 
    usr.write_spc()

Usage
-----

When the program is ready, copy it to a convenient location and make it 
executable using the shell command::

  linux:~> chmod u+x myusermodel.py
  
In SPEX, this program can be loaded into the 'user' or 'musr' model using 
the following commands::
  
  SPEX> com user
  SPEX> par 1 1 exec av ./myusermodel.py
  
The './' is used if the program is in the current working directory. Otherwise,
please use the full path to the program.

The 'user' and 'musr' model also needs the number of parameters that the 
model needs. Set it using::

  SPEX> par 1 1 npar v 1
  
You can set your model parameters in a similar way. Have fun!
       
            



