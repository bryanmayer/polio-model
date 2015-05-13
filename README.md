# Polio-SIV-Model -- Overview:

This is the code to run the SIV (susceptible-infected-vaccined) model from the 2013 American Journal of Epidemiology publication: "Successes and shortcomings of polio eradication: a transmission modeling analysis."  

The details of the model have been written up in the [AJE paper](https://github.com/bryanmayer/Polio-SIV-Model/blob/master/Publication/Am.J.Epidemiol.-2013-Mayer-1236-45.pdf) and its [web supplement](https://github.com/bryanmayer/Polio-SIV-Model/blob/master/Publication/Web_Material.pdf).  The structure of the model is [here](https://github.com/bryanmayer/Polio-SIV-Model/blob/master/Model_Diagram.pdf "Model").

To run the model, a working version of numpy and scipy are required.  pylab is also used but is not necessary.

# run_model.py 

This file will run the model and output a figure showing the prevalence over time (in Output_figures).  Parameters can be edited in the load_parameters.py file (see next section)
This can be run in bash using:

    >>> python run_model.py


This is a very simple implementation of the model.  The initial values (Code/Input_data/example_initial.txt) are given for a contact rate of 160.  If you change the contact rate  (absent vaccination) then the compartment populations will change: there will be oscillatory behavior before a new steady state is reached.  To account for this, the model can be simulated without vaccine first, then the steady state values can be saved as the new initial value (change storeOut to 1 in runModel.py, this will store Code/Input_data/new_initial_values.txt). You can either edit the run_model.py file to point to the new initial value text file or change its name to example_initial.txt.

Alternatively, a large pre-vaccination period can be chosen (a large vaccine_start_date, usually greater than 100 years to be safe) to allow for steady state to be reached.

The results from the paper are created by running large batch runs over range of values of several parameters and looking at key results.  The simulations are fairly slow, sometimes taking up to 5 minutes.  

#load_parameters.py
Here you can edit the parameters:

The simulation length -- ND (years)

The main transmission parameters:  
1) contact rate -- c (R_0 ~ c\delta)  
2) the waning rate -- roBetaRate (paper uses 0.04, 0.07, and 0.1)  
3) the relationship of OPV to WPV -- relative contagiousness (epsilon) and relative recovery (gamma).  For the paper we assumed epsilon = 1/gamma but that isn't necessary.

The vaccination parameters:  
1) vaccinate -- whether there is a vaccination program (True or False)  
2) vaccination rate -- vaccRate (per year)  
3) year vaccination starts in the model -- vaccine_start_yr.  May want to be late if changing contact rate without updating initial values  
4) time to reach full vaccination rate -- vaccine_ramp_yrs.  We assume that vaccination program starts at vaccine_start_yr but doesn't reach the full vaccRate until after vaccine_ramp_yrs.  It linearly increases from 0 to vaccRate in this time.  

Other parameters are listed, it is not recommended that they are edited

#Code Folder
Contains some setup code, model code (ODE function), and the model class files to run the model.

## load_demographic.py
The age compartment and vital dynamic setup

## load_waning.py
Implementation of the waning function (see [web supplement](https://github.com/bryanmayer/Polio-SIV-Model/blob/master/Publication/Web_Material.pdf)) 

## polio_model_class.py
This class object is how I decided to store the model object.  A lot of code here is to restructure the population array (3 x n x m) into a vector to be used by odeint. In hindsight, using an object is computationally inefficient.

## polio_model.py
This file contains the function to be solved by the ode solver.  Equation handling is done using linear algebra instead of loops.

##Input_data folder
Contains initial value file and age-specific death rates for vital dynamics.

# Output_figures folder
Figures generated from the run_model.py.  At this point just the prevalance figure and the total population figure (which is currently commented out in the run_model.py file)

# Figures folder
TODO -- the figures from the manuscript and the R code used to create them.


#main_notebook.ipynb

TODO- This is still a work in progress
