.. role:: py(code)
      :language: python

.. role:: bash(code)
      :language: bash


.. code-block:: python

                ## :: ::    :::            ##
          ##:::   :::::    :::   ::    :: :: :::##
       ## ::::     ::           ::   ::::::::::::::##
     ##::::::                       ::::::::: :::::: :##
   ## :    :::                    :::    ::    :::::::::##
  ##ggggg eeee ooo   n   n   ooo   m   m iiiii  cccc ssss##
 ##g     e    o   o  nn  n  o   o  m   m   i   c     s    ##
 ##g     eee o     o n n n o     o mm mm   i   c     sssss##
 ##g ggg eee o     o n  nn o     o m m m   i   c         s##
 ##g   g e    o   o  n   n  o   o  m   m   i   c        ss##
  ##gggg  eeee ooo   n   n   ooo   m   m iiiii  cccc ssss##
   ##  ::::::::        ::::::::::::  :       ::  ::   : ##
     ##  ::::              :::::::  ::     ::::::::  :##
       ## :::               :::::: ::       ::::::  ##
          ##:                ::::                ##
                ##                         ##
  
 
#########
Geonomics
#########


::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
A Python package for easy construction of individual-based, spatially explicit, forward-time, and highly customizable landscape-genomics simulations
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::


********
Provides
********

  1. :py:`Landscape`, :py:`GenomicArchitecture`, :py:`Trait`,
     :py:`Individual`, :py:`Population`, and :py:`Community` classes
  2. A :py:`Model` class that builds objects of the aforementioned classes 
     according to the scenario stipulated in a parameters file,
     then uses them to run numerous simulation iterations
  3. Classes to help the user to save data and basic statistics 
     from those iterations at all desired timesteps
  3. Classes to help the user model arbitrarily complex landscape- and 
     demographic-change scenarios
  4. Numerous visualization methods, to help the user design, run, explore, 
     present, and explain their models' behavior and results


*****
Intro
*****

Backward-time (i.e. coalescent) simulators abound.
But they are inadequate for simulation of many scenarios of 
interest, including: natural selection on traits with arbitrary genomic 
architectures; spatially variable natural selection; simulation of populations
distributed continuously and moving realistically across
complex landscapes; complex demographic change simultaneous with ongoing, 
often non-stationary environmental change; and coevolutionary interactions 
between multiple species or incipient species. Few existing forward-time 
simulators can model all of these phenomena, and the few that can often 
impose a high cost of entry (e.g. learning a new, non-standard programming
language in order to write one's desired simulations). Geonomics aims to fill 
this empty niche by combining ease of use with broad extensibility. 
If it succeeds at doing this, Geonomics should prove uniquely useful
for a wide range of purposes, from intro-level educational use to
high-quality theoretical, methodological, empirical, and
applied research.

Geonomics is written in Python, a full-fledged scripting language 
that is relatively easy to learn (and fun!). So it can be pretty quick for a
new user to get up to speed and start doing useful work. For work with
Geonomics, this turnaround time should be even quicker. Geonomics aims to
require minimal Python knowledge (yet maintain high extensibility for
interested, expert users). Essentially, anyone should be able to build their
own, arbitrarily complex Geonomics models as long as they know how to install
the package, open a Python console, call Python functions, and edit some
default values in a pre-packaged script. 

Geonomics will allow you to:

  - create a :py:`Landscape` with any number of :py:`Layers` in it; 
  - create any number of :py:`Population`\s living on that
    :py:`Landscape`, each of which is composed of a bunch of 
    independent :py:`Individual`\s, and each of which will have a bunch of
    parameters dsecribing what it's like and how it lives;
  - optionally give the :py:`Individual`\s of any :py:`Population`\s
    genomes, which can optionally determine phenotypes for any number 
    of :py:`Trait`\s (all of this is controlled by the
    :py:`GenomicArchitecture` that you would create for
    the :py:`Population`\s);
  - simulate any number of timsesteps of the evolution of those
    :py:`Population`\s on that :py:`Landscape`, where each timestep can include
    movement, mating, mortality (by density-dependence and optionally also by
    natural selection), and demographic, life-history, or
    environmental changes

-------------------------------------------------------------------------------

==================
Object-orientation
==================

For a more technical understanding of the model, it may be helpful to 
understand the concept of **object-oriented programming**.  Here is a very
brief tutorial for the unacquainted:

Python is a very handy language for object-oriented programming, 
and this is the primary programming paradigm in which Geonomics is written. 
Essentially, object-orientation involves: 

  1. Defining certain types of data structures, or **classes** (e.g.
     :code:`Car`), and assigning them various behaviors, or **methods**
     (e.g. :code:`honk`, :code:`turn`, :code:`brake`);
  2. Using particular data values to create individual instances, or 
     **objects** belonging to those classes (e.g. :code:`my_1986_jeep`, or
     :code:`batmobile`);
  3. Instructing those **objects** to carry out their behaviors by 'calling' 
     their **methods** (e.g. :code:`my_1986_jeep.honk()` might return "Beepity
     beep!", wheras :code:`batmobile.honk()` might
     return "<Batman theme song>"). 
     
Geonomics defines a number of **classes**
(such as the :py:`Landscape`, :py:`Layer`,
:py:`Popualtion`, :py:`GenomicArchitecture`, and :py:`Trait` classes mentioned
above. The user will use the values they specfiy in a parameters file to
create **objects** belongining to these classes. Then the user will call
key **methods** that belong to these **objects**, to get them
to carry out certain behaviors (which are what constitute the simulation).

The subsequent documentation will present the **classes** definined in
Geonomics and their key **methods**. It will explain exactly what those methods
do, and how what they do fits into the overall structure and function of 
Geonomics models.


-------------------------------------------------------------------------------

===============
Getting started
===============

**For the beginner**, we recommend the following steps:
  1. Review the following three sections ('Model organization', 'Data
     structures and classes', and 'Operations'), to get a general
     undertsanding of the logic, components, and necessary and optional
     behaviors of a Geonomics model;
  2. Skim the subsequent section ('Parameters'), to understand the structure
     of a Geonomics parameters file;
  3. Pip-install Geonomics (:bash:`$ pip install geonomics`);
  4. Open Python and run :py:`import geonomics as gnx`;
  5. Use the :py:`gnx.make_parameters_file` function, to begin
     creating template parameters files that they can tweak as desired;
  6. Use the :py:`gnx.make_model` function and then the
     :py:`Model.walk` or :py:`Model.run` methods to instantiate and run
     the models they've parameterized;
  7. Use the various :py:`Model.plot` methods to visualize
     the behavior and results of their models.

**For the `impatient` beginner**, as soon as Geonomics has been
installed, you should be able to run the following code:

.. code-block:: python

     import geonomics as gnx

     gnx.run_default_model()

This will load the Geonomics package, create a default Geonomics
parameters file in your current working directory, 
then use that file to instantiate and run a :code:`Model` using the default
parameter values.


-------------------------------------------------------------------------------

=================
The documentation
=================

Finally, some brief notes about the structure and style of this documentation: 

  It is designed to be read from from the top down; information generally 
  becomes increasingly detailed as you scroll down). However, given the 
  interrelationships between all the components of a Geonomics 
  :py:`Model`, there are inevitably places where you'll run
  into material that relates to material from another section.
  To the extent possible, we attempt to cross-reference rather than duplicate
  information.

  We assume, throughout, that Genomics has been imported :py:`as gnx` and
  that Numpy has been imported :py:`as np`.


Merry modeling!


-------------------------------------------------------------------------------

-------------------------------------------------------------------------------

******************
Model organization
******************

  ::

       <<DIAGRAM HERE>>

   +-------+        +-------+ 
   | stuff | -----> | stuff | 
   +-------+        +-------+ 
       ^                |     
       |                v     
   +-------+        +-------+ 
   | stuff | <----- | stuff | 
   +-------+        +-------+ 


-------------------------------------------------------------------------------

***************************
Data structures and classes
***************************

The following sections discuss the structure and function of the key
Geonomics classes. Users will interface with these classes more or less
directly when running Geonomics models, so a fundamental understanding of how 
they're organized and how they work will be useful.

=======================================
:py:`Landscape` and :py:`Layer` objects
=======================================

One of the core components of a Geonomics model is the land. The land is
modeled by the :py:`Landscape` class. This class is an 
integer-keyed :py:`dict` composed of numerous instances of the
class :py:`Layer`. Each :py:`Layer` represents a separate 
environmental variable (or 'layer', in GIS terminology),
which is modeled a 2d Numpy array (or raster; in
attribute 'rast'), of identical dimensions to each 
other :py:`Layer` in the :py:`Landscape`
object, and with the values of its environmental variable 'e' constrained to
the interval [0 <= e <= 1]. Each :py:`Layer` can be initialized from its own
parameters subsection within the 'land' parameters section of a Geonomics
parameters file. 

For each :py:`Population` (see section ':py:`Individuals`
and :py:`Populations`', below), the different :py:`Layer`
layers in the :py:`Landscape` can be used to model habitat 
viability, habitat connectivity, or variables imposing spatially varying
natural selection. :py:`Landscape` and :py:`Layer` objects
also contain some metatdata (as public attributes), including
the resolution (attribute 'res'), upper-left corner ('ulc'),
and projection ('prj'), which default to 1, (0,0), and None but
will be set otherwise if some or all of the :py:`Layer` layers are read in from
real-world GIS rasters.


-------------------------------------------------------------------------------

===========================================================
Genomes, :py:`GenomicArchitecture`, and :py:`Trait` objects
===========================================================

:py:`Individual` objects (see section ':py:`Individuals`
and :py:`Populations`', below) can optionally be assigned genomes.
If they are, each :py:`Individual`'s genome is modeled as a 
2-by-L Numpy array (where 2 is the ploidy, currently fixed at
diploidy, and L is genome length) containing 0s and 1s (because
Geonomics strictly models diallelic SNPs). 

The parameter L, as well as numerous other genomic parameters (including 
locus-wise starting frequencies of the 1 alleles; locus-wise dominance effects;
locus-wise recombination rates; and genome-wide mutation rates for neutral, 
globally deleterious, and adaptive loci), are controlled by the 
:py:`GenomicArchitecture` object pertaining to the :py:`Population` to which an 
:py:`Individual` belongs. (For the full and detailed list of attributes in a 
:py:`GenomicArchitecture` object, see its class documentation, below.)
The genomes of the initial :py:`Individual`\s 
in a simulation, as well as those of 
:py:`Individual`\s in subsequent generations, are either drawn
or recombined, and are mutated, according to the values stipulated 
by the :py:`GenomicArchitecture` of
their :py:`Population`. The user can create a population with a 
:py:`GenomicArchitecture` and with corresponding
genomes by including a 'genome' subsection in that
population's section of the Geonomics parameters file (and 
setting the section's various parameters to their desired values). 

Geonomics can model :py:`Individual`\s' phenotypes.
It does this by allowing the 
user to create an arbitrary number of distinct :py:`Trait`\s
for each :py:`Population`. Each trait is
represented by a :py:`Trait` object, which 
maps genomic loci onto that trait, maps effect sizes ('alpha') onto those loci,
and sets the trait's polygenic selection
coefficient ('phi'). An :py:`Individual`'s
phenotype for a given trait is calculated as the 'null phenotype' plus a 
weighted sum of the products of its 'effective genotypes' at all loci 
underlying that :py:`Trait` and the effect sizes (i.e. 'alpha') of those loci:

.. math::

   z_{i,t} = null\_genotype + \sum_{l = 0}^{n} \alpha_{t,l} g_{i,l}

where :math:`z_{i,t}` is the phenotype of :py:`Individual` i for trait t, 
:math:`g_{i, l}` is the genotype of the :py:`Individual` at that locus, and 
:math:`\alpha_{t,l}` is the effect size of that locus for that trait.

The 'null phenotype' refers determines what would be the phenotypic value that
an :py:`Individual` who is homozygyous for
the 0 allele at all loci for a trait.
For monogenic traits the null phenotype is 0 and the effect size is fixed at 
0.5 (such that individuals can have phenotypes of 0, 0.5, or 1); 
for polygenic traits the null phenotype is 0.5 and effect sizes can be fixed 
at or distributed around a mean value (which is controlled in the 
parameters file).

The 'effective genotype' refers to how the genotype is calculated based on the 
dominance at a locus, as indicated by the following table of genotypes:

+--------------------+------------------+------------------+
| Biallelic genotype |   Codominant     |     Dominant     |
+====================+==================+==================+
|      0 : 0         |        0         |        0         |
+--------------------+------------------+------------------+
|      0 : 1         |       0.5        |        1         |
+--------------------+------------------+------------------+
|      1 : 1         |        1         |        1         |
+--------------------+------------------+------------------+

(For the full and detailed list of attributes in a :py:`Trait` object, 
see its class documentation, below.)

Note that for maximal control over the :py:`GenomicArchitecture`
of a :py:`Population`, the user can set the value of the 'gen_arch_file' 
parameter in the parameters file to the name of a separate CSV file 
stipulating the locus numbers, starting 1-allele frequencies, dominance 
effects, traits, and inter-locus recombination rates (as columns) of 
all loci (rows) in the :py:`GenomicArchitecture`;
these values will override any other values provided in the 'genome' 
subsection of the population's parameters.


-------------------------------------------------------------------------------

===============================================================
:py:`Individual`, :py:`Population`, and :py:`Community` objects
===============================================================

Being that Geonomics is an individual-based model, individuals serve as 
the fundamental units (or agents) of all simulations. They are represented by
objects of the :py:`Individual` class.
Each :py:`Individual` has an index (saved 
as attribute 'idx'), a sex (attribute 'sex'), an age (attribute 'age'), 
an x,y position (in continuous space; attributes 'x' and 'y'), and a 
:py:`list` of environment values (attribute 'e'), extracted from the 
:py:`Individual`'s current cell on each :py:`Layer`
of the :py:`Landscape` on which the :py:`Individual` lives.

The :py:`Population` class is an :py:`OrderedDict`
(defined by the :py:`collections` 
package) containing all :py:`Individaul`\s, (with 
their 'idx' attributes as keys). If a :py:`Population`
has a :py:`GenomicArchitecture` then the :py:`Individual`\s
in the :py:`Population` will also each have genomes (attribute 'genome'),
and the :py:`GenomicArchitecture` includes :py:`Trait`\s
then each individual will also have a :py:`list` of 
phenotype values (one per :py:`Trait`; attribute 'z') and a 
single fitness value (attribute 'fit'). (These attributes all otherwise 
default to :py:`None`.)

Each :py:`Population` also has a number of other attributes of interest. Some 
of these are universal (i.e. they are created regardless of the 
parameterization of the :py:`Model` to which a :py:`Population` inheres). These 
include: the :py:`Population`'s name (attribute 'name'); its current density 
raster (a Numpy array attribute called 'N'); and the number of births,
number of deaths, and terminal population size of each timestep (which are 
:py:`list` attributes called 'n_births', 'n_deaths', and 'Nt'). If the 
:py:`Population` was parameterized with a
:py:`GenomicArchitecture` then that will 
be created as the 'gen_arch' attribute (otherwise this attribute will be 
:py:`None`).

All of the :py:`Population`\s in a :py:`Model`
are collected in the :py:`Model`'s 
:py:`Community` object. The :py:`Community` class
is simply an integer-keyed :py:`dict` 
of :py:`Population`\s. For the time being, the :py:`Community` object allows a 
Geonomics :py:`Model` to simulate multiple :py:`Population`\s simultaneously on 
the same :py:`Landscape`, but otherwise affords no additional functionality
of interest. However, its implementation will facilitate the potential 
future development of methods for interaction between :py:`Population`\s. 
(e.g. to simulate coevolutionary, speciation, or hybridization scenarios).


-------------------------------------------------------------------------------

===================
:py:`Model` Objects
===================

Objects of the :py:`Model` class serve as the main interface between the user 
and the Geonomics program. (While it is certainly possible for a user 
to work directly with the :py:`Landscape`
and :py:`Population` or :py:`Community` objects to 
script their own custom models, the typical user should find that the 
:py:`Model` object allows them accomplish their goals with minimal toil.)
The main affordance of a :py:`Model` object is the :py:`Model.run` method, 
which, as one could guess, will run the :py:`Model`. The typical workflow 
for creating and running a  :py:`Model` object is as follows:

  1. Create a template paramters file containing the desired sections, 
     by calling :py:`gnx.make_parameters_file` with all revelant arguments;
  2. Define the scenario to be simulated, by opening and editing that 
     parameters file (and optionally, creating/editing corresponding 
     files, e.g. genomic-architecture CSV files;
     or raster or numpy-array files to be used as :py:`Layer`\s);
  3. Instantiate a :py:`Model` object from that parameters file, by calling 
     :py:`mod = gnx.make_model('/path/to/params_filename.py')`;
  4. Run the :py:`Model`, by calling :py:`mod.run()`.

For detailed information on usage of these functions, see their docstrings.
When a :py:`Model` is run, it will:

  1. Run the burn-in (until the mininmal burn-in length stipulated in the 
     parameters file and the built-in stationarity statistics 
     determine that the burn-in is complete);
  2. Run the main model for the stipulated number of timesteps;
  3. Repeat this for the stipulated number of iterations (retaining or 
     refreshing the first run's initial :py:`Landscape` and :py:`Population` 
     objects as stipulated).

The :py:`Model` object offers one other method, however, :py:`Model.walk`, 
which allows the user to run a model, in either 'burn' or 'main' mode, 
for an arbitrary number of timesteps within a single iteration (see its 
docstring for details). This is particularly useful for running 
Geonomics within an interactive Python session. Thus, :py:`Model.walk` is 
primarily designed for passively running numerous iterations of a :py:`Model`, 
to generate data for analysis, whereas :py:`Model.walk` is primarily designed
for the purposes of learning, teaching, or debugging the package, or 
developing, exploring, introspecting, or visaulizing particular :py:`Model`\s. 


-------------------------------------------------------------------------------

=================
Secondary classes
=================

The typical user will not need to access or interact with the following 
classes in any way. They will, however, parameterize them in the 
parameters file by either leaving or altering their default values. Geonomics 
sets generally sensible default parameter values wherever possible, 
but for some scenarios they may not be adequate, and for some parameters 
(e.g. the window-width used by the _DensityGridStack; see below), there is 
no "one-size-fits-most" option. Thus, it is important that the user
have a basic acquaintance with the purpose and operation of these classes.

----------------------
:py:`_MovementSurface`
----------------------

The :py:`_MovementSurface` class allows Geonomics
to model a :py:`Population`'s 
realistic movement across a spatially varying landscape. It does this by 
creating an array of circular probability distributions (i.e. VonMises 
distributions), one for each cell on the :py:`Landscape`, from which 
:py:`Individual`\s choose their directions each time they move. To create the
:py:`_MovementSurface` for a :py:`Population`,
the user must indicate the :py:`Layer` 
that should be used to create it (i.e. the :py:`Layer` that represents 
landscape permeability for that :py:`Population`). The :py:`_MovementSurface`'s 
distributions can be **simple (i.e. unimodal)**, such that the 
maximum value of the distribution at each cell will point toward the
maximum value in the 8-cell neighborhood; this works best for permeability 
:py:`Layer`\s with shallow, monotonic gradients, because the differences 
between permeability values of neighboring cells can be minor (e.g. a 
gradient representing the directionality of a prevalent current). 
Alternatively, the distributions can be **mixture (i.e. multimodal)**
distributions, which are weighted sums of 8 unimodal distributions, one 
for each neighboring cell, where the weights are the relative cell 
permeabilities (i.e. the relative probabilities that an :py:`Individual` would 
move into each of the 8 neighboring cells); this works best for non-monotonic, 
complex permeability :py:`Layer`\s (e.g. a DEM of a mountainous region that is 
used as a permeability :py:`Layer`). 
(The :py:`Landscape` is surrounded by a margin of 0-permeability 
cells before the :py:`_MovementSurface` is calculated, such 
that :py:`Landscape` edges are treated 
as barriers to movement.) The class consists 
principally of a 3d Numpy array (x by y by z, where x and y are the 
dimensions of the :py:`Landscape` and z is the length of the vector of values 
used to approximate the distributions in each cell.

-----------------------
:py:`_DensityGridStack`
-----------------------

The :py:`_DensityGridStack` class implements an algorithm for rapid estimating 
an array of the local density of a :py:`Population`. The density is estimated 
using a sliding window approach, with the window-width determining the 
neighborhood size of the estimate. The resulting array has a spatial 
resolution equivalent to that of the :py:`Landscape`, and is used in all
density-dependent operations.

-------------
:py:`_KDTree`
-------------

The :py:`_KDTree` class is just a wrapper around :py:`scipy.spatial.cKDTree`. 
It provides an optimized algorithm (the kd-tree) for finding 
neighboring points within a given search radius.
This class is used for all neighbor-searching operations (e.g. mate-search).

-------------------------
:py:`_RecombinationPaths`
-------------------------

The :py:`_RecombinationPaths` class contains a large (and customizable) 
number of :py:`bitarray`\s, each of which indicates the genome-length 
diploid chromatid numbers (0 or 1) for a
recombinant gamete produced by an :py:`Individual` of a given :py:`Population` 
(henceforth referred to as 'recombination paths'). These recombination 
paths are generated using the genome-wide recombination rates specified by 
the :py:`Population`'s :py:`GeonomicArchitecture`. They are generated during 
construction of the :py:`Model`, then drawn randomly as needed (i.e.
each time an :py:`Individual` produces a gamete). This provides a 
reasonable trade-off between realistic modelling of recombination and runtime.

----------------------------------------------------
:py:`_LandscapeChanger` and :py:`_PopulationChanger`
----------------------------------------------------

These classes manage all of the landscape changes and demographic changes 
that were parameterized for the :py:`Landscape` and
:py:`Population` objects to which they inhere. 
The functions creating these changes are defined at the outset, 
then queued and called at their scheduled timesteps.

----------------------------------------------
:py:`_DataCollector` and :py:`_StatsCollector`
----------------------------------------------

These classes manage all of the data and statistics that should be collected 
and written to file for the :py:`Model` object to which they inhere 
(as determined by the parameters file used the create the :py:`Model`). 
The types of data to be collected, or statistics to be calculated, as 
well as the timesteps at which and methods by which they're 
collected/calculated and determined at the outset, then the 
appropriate functions called at the appropriate timesteps.


-------------------------------------------------------------------------------

**********
Operations
**********

The following sections discuss the mechanics of core Geonomics operations. 
The material here is inevitably intertwined with some of the material in 
the "Data structures and classes" section. To the extent possible, we 
attempt to cross-reference rather than duplicate information (with 
the exception of this sentence).

======================
Movement and Dispersal
======================

Movement is optional, such that turning off movement will allow the user 
to simulate sessile organisms (which will reproduce and disperse, 
but not move after dispersal; this distinction is of course irrelevant 
for a :py:`Population` with a maximum age of 1). For :py:`Population`\s 
with movement, :py:`Individual`\s can
move by two distinct mechanisms. **Spatially random movement**
is the default behavior; in this case, :py:`Individual`\s 
move to next locations that are determined by a random distance drawn 
from a Wald distribution and a random direction drawn from a uniform 
circular (i.e. Von Mises) distribution.  As with most distributions used 
in Geonomics, the parameters of these distributions have sensible 
default values but can be customized in a :py:`Model`'s parameters file 
(see section 'Parameters', below). 

The alternative movement mechanism that is available is 
**movement across a permeability surface**,
using a :py:`_MovementSurface` object.
To parameterize a :py:`_MovemementSurface` for a :py:`Population`, the user 
must create a template parameters file that includes the 
necessary parameters section for the population (i.e. 
the user must set 'movement' to :py:`True` and 'movement_surface' to :py:`True` 
in the population's arguments to the :py:`gnx.make_parameters_file` 
function (see the docstring for that function for details and an example). 
:py:`Individual`\s move to next locations determined by a random distance drawn 
from a Wald distribution and a random direction drawn from the distribution 
at the  :py:`_MovementSurface` cell in which which the :py:`Individual`\s 
are currently located. For details about :py:`_MovementSurface` creation, see 
section ':py:`_MovementSurface`' above, or the class' docstring.

Dispersal is currently implemeneted identically to spatially random movement 
(with the caveat that the an offspring's new location is determined 
relative its parents' centroid). But the option to use a 
:py:`_MovementSurface` for dispersal will be offered soon.


-------------------------------------------------------------------------------

============
Reproduction
============

Each timestep, for each :py:`Population`, all pairs of individuals within 
a certain distance of each other (i.e. the mating radius, 
which is set in the parameters file) are identified.
These pairs are subsetted if necessary (i.e. if the :py:`Population` 
requires that :py:`Individual`\s be above a certain reproductive age, 
or that they be of opposite sexes, in order to mate; these values 
can also be changed from their defaults in the parameters file). 
Remaining pairs mate probabilistically (according to a Bernoulli 
random draw with probability equal to the :py:`Population`'s birth 
rate, which is also set in the parameters file).

Pairs that are chosen to mate will produce a number of new 
offspring drawn from a Poisson distribution (with lambda set in the 
parameters file). For each offspring, sex is chosen probablistically 
(a Bernoulli random draw with probability equal to the :py:`Population`'s 
sex ratio), age set to 0, and location chosen by dispersal from 
the parents' centroid (see section 'Movement and Dispersal'). For 
:py:`Population`\s that have genomes, offspring genomes will be a 
fusion of two recombinant genomes from each of the two parents (where 
each recombinant is indexed out a parent's genome using a recombination 
path; see section ':py:`_RecombinationPaths`'). For :py:`Population`\s 
with :py:`Trait`\s in their
:py:`GenomicArchitecture`\s, offspring phenotypes are 
determined at birth. Mutations are also drawn and introduced at this 
point (see section 'Mutation for details).


-------------------------------------------------------------------------------

=========
Mortality
=========

Mortality can occur as a combination of two factors: **density dependence** 
and **natural selection**. Each :py:`Individual` has a death decision drawn 
as a Bernoulli random variable with 
:math:`P(d_{i}) = 1 - P(s_{i_{dens}})P(s_{i_{fit}})`, where :math:`P(d_{i})` 
is the probability of death of :py:`Individual` :math:`i`, and 
:math:`P(s_{i_{dens}})` and :math:`P(s_{i_{fit}})` are the probabilities of 
survival of :py:`Individual` :math:`i` given density-dependence and 
fitness. The probability of density-dependent death is contingent on an 
:py:`Individual`'s x,y location
(i.e. the cell in which they're currently located. 
And an :py:`Individual`'s probability of survival due to fitness 
is just equal to the product of their absolute fitness (:math:`\omega`) 
for each of the :py:`Individual`'s :math:`m` :py:`Trait`\s. 
Thus the equation for an :py:`Individual`'s probability of death becomes:

.. math::
   P(d_{i}) = 1 - (1 - P(d_{x,y})) \prod_{p = 1}^{m}\omega_{i,p}

The following two sections explain in detail the implementation and 
calculation of the two halves of the right side of this equation.

------------------
Density dependence
------------------

Density dependence is implemented using a spatialized form of the class 
logistic growth equation (:math:`\frac{\mathrm{d}
N_{x,y}}{\mathrm{d}t}=rN_{x,y}(1-\frac{N_{x,y}}{K_{x,y}})`, 
where the x,y subscripts refer to
values for a given cell on the :py:`Landscape`).
Each :py:`Population` has a carrying-capacity raster (a 2d Numpy array; 
attribute 'K'), which is defined in the parameters file to be 
one of the :py:`Layer`\s in the :py:`Landscape`.
The comparison between this raster and 
the population-density raster calculated at each timestep serves as the 
basis for the spatialized logistic growth equation, because both 
equations can be calculated cell-wise for the entire extent of the 
:py:`Landscape` (using the :py:`Population`'s
intrinsic growth rate, the attribute 
'R', which is set in the parameters file).

The logistic equation returns an array of instantaneous population growth 
rates within each cell. We can derive from this the density-dependent 
probability of death at each cell by subtracting an array of the expected 
number of births at each cell, then dividing by the array of 
population density:

.. math::
   P(d_{x,y}) = E[N_{d;x,y}]/N_{x,y} = \frac{E[N_{b;x,y}] 
    - \frac{\mathrm{d}N_{x,y}}{\mathrm{d}t}}{N_{x,y}}

The expected number of births at each cell is calculated as a density 
raster of the number of succesful mating pairs, multiplied by the expected 
number of births per pair (i.e. the expectation of the Poisson 
distribution of the number of offspring per mating pair, which 
is just the distribution's paramater lambda). 

---------
Selection
---------

Selection on a :py:`Trait` can exhibit three regimes: **spatially divergent**, 
**universal**, and **spatially contingent**. **Spatially divergent** selection 
is the default behavior, and the most commonly used; in this form of 
selection, an :py:`Individual`'s fitness depends on the absolute difference 
between the :py:`Individual`'s phenotypic value and the environmental
value of the relevant :py:`Layer` (i.e. the :py:`Layer` that represents the 
environmental variable acting as the selective force) in the cell where 
the :py:`Individual` is located.

**Universal** selection (which can be toggled using the 'univ_adv' 
parameter with a :py:`Trait`'s section in the parameters file) occurs 
when a phenotype of 1 is optimal everywhere on the :py:`Landscape`. In other 
words, it represents directional selection on an entire :py:`Population`,
regardless of :py:`Individual`\s' spatial contexts. (Note that this can
be thought of as operating the same as spatially divergent selection,
but with the environmental variable driving natural selection being
represented by an array in which all cells are equal to 1.)

Under **spatially contingent** selection, the selection coefficient of a 
:py:`Trait` varies across space, such that the strength of selection 
is environmentally determined in some way. Importantly, this selection regime
is *not mutually exclusive* with the other two; in other words, 
selection on a certain :py:`Trait` be both spatially contingent 
and either spatially divergent or universal. Spatially contingent selection 
can be implemented by providing an array of values (equal in dimensions 
to the :py:`Landscape`) to the 'phi' value of a
:py:`Trait`, rather than a scalar 
value (which could be done within the parameters file itself, but may be 
more easily accomplished as a step between reading in a parameters file and 
instantiating a :py:`Model` object from it). (Note that non-spatailly
cotingent selection could in fact be thought of as a special case of
spatially contingent selection, but where the array of selection-coefficients
has the same value at each cell.)

All possible combinations of the three selection regimes of selection can all 
be thought of as special cases of the following equation for the fitness of 
:py:`Individual` :math:`i` for :py:`Trait` :math:`p` (:math:`\\omega_{i,p}`):

.. math::
   \omega_{i,p}= 1 - \phi_{p;x,y} (\mid e_{p;x,y} - z_{i;p} \mid)^{\gamma_{p}}

where :math:`\\phi_{p;x,y}` is the selection coefficient of trait 
:math:`p`; :math:`e_{p;x,y}` is the environmental variable of the 
relevant :py:`Layer` at :py:`Individual` :math:`i`'s x,y location
(which can also be thought of as the :py:`Individual`'s optimal 
phenotype); :math:`z_{i;p}` is :py:`Individual` :math:`i`'s (actual) 
phenotype for :py:`Trait` :math:`p`; and :math:`gamma_{p}` controls 
the curvature of the fitness function (i.e. how fitness decreases as
the absolute difference between an :py:`Individual`'s 
optimal and actual phenotypes increases; the default value of 1 causes 
fitness to decrease linearly around the optimal phenotypic value). 


-------------------------------------------------------------------------------

========
Mutation
========

Geonomics can model mutations of three different types: **neutral**, 
**deleterious**, and **trait** mutations. These terms don't map 
precisely onto the traditional population-genetic
lingo of "neutral", "deleterious", and "beneficial", but they 
are more or less analogous:

- **Neutral** mutations are the same conceptually in Geonomics as 
  they are in the field of population genetics in general: 
  They are mutations that have no effect on the fitness of
  the individuals in which they occur.
- **Deleterious** mutations in Geonomics are also conceptually the 
  same in Geonomics and in population genetics: They negatively impact 
  the fitness of the individuals in which they occur.
- **Trait** mutations are the place where the Geonomics concept and 
  the population-genetic concept diverge: In Geonomics, natural selection
  acts on the phenotype, not the genotype (although these concepts are 
  identical if a :py:`Trait` in monogenic), and it is (by default, 
  but not always; see section 'Selection', above) divergent. For this reason
  it would be a misnomer to call mutations that influence a given 
  :py:`Trait`'s phenotypes 'beneficial' -- even though that term is the closest
  population-genetic concept to this concept as it is employed in Geonomics -- 
  because the same mutant genotype in the same :py:`Individual`
  could have opposite effects on that :py:`Individual`'s fitness 
  in different environmental contexts (i.e. it could behave as
  a beneficial mutation is one region of the :py:`Landscape` 
  but as a deleterious mutation in another). 


-------------------------------------------------------------------------------

=======================
Population interactions
=======================

This functionality is not yet included available. But the Community class was 
created in advance recognition that this functionality could be desirable 
for future versions (e.g. to simulate coevolutionary, speciation, or 
hybridization scenarios).


-------------------------------------------------------------------------------

===========================================
:py:`Landscape` and :py:`Population` change
===========================================

For a given :py:`Layer`, any number of change events 
can be planned. 
In the parameters file, for each event, the user stipulates the initial
timestep; the final timestep; the end raster (i.e. the array 
of the :py:`Layer` that will exist after the event is complete, defined using
the **end_rast** parameter); and the 
interval at which intermediate changes will occur.  When the :py:`Model` is 
created, the stepped series of intermediate :py:`Layers` (and 
:py:`_MovementSurface` objects, if the :py:`Layer` that is changing serves 
as the basis of a :py:`_MovementSurface` for any :py:`Population`) will be 
created and queued, so that they will swap out accordingly at the appropriate 
timesteps.

For a given :py:`Population`, any number of demographic change events can 
also be planned. In the parameters file, for each event, the user 
stipulates the type of the event ('monotonic', 'cyclical', 'random', or 
'custom') as well as the values of a number of associated 
parameters (precisely which parameters depdends on the type of event chosen).
As with :py:`Landscape` change events, all necessary stepwise changes will be 
planned and queued when the :py:`Model` is created, and will be 
executed at the appropriate timesteps.

It is also possible to schedule any number of instantaneous changes 
to some of the life-history parameters of a :py:`Population` (e.g. birth rate; 
the lambda parameter of the Poisson distribution determining the number of 
offspring of mating events). This functionality is currently minimalistic, 
but will be more facilitated in future versions.


-------------------------------------------------------------------------------

*************
Visualization
*************

Each :py:`Population` has a wide variety of visualization methods 
(:py:`Population.plot`, :py:`Population.plot_fitness`, etc.),
which aim to help users design, run, explore, present,
and explain their models' behavior and results.
These methods can be called on a :py:`Population` at any time (e.g. as 
soon as the :py:`Population` has been created, or after the model has
run for any number of timesteps); but it is worth mentioning that some 
methods may be invalid depending on the point in model-time at 
which they're called (e.g.  :py:`Population.plot_genotype`, 
:py:`Population.plot_phenotype`, and :py:`Population.plot_fitness`
cannot be run for Populations that have not yet been burned in,
as they will not yet have genomes assigned) or 
the :py:`Population` on which they're called 
(e.g. the aforementioned methods cannot create plots for a :py:`Population` 
that has no :py:`GenomicArchitecture`; and likewise, the 
:py:`Population.plot_demographic_changes` method cannot be called for a 
:py:`Population` for which demographic changes were not parameterized).

The :py:`Landscape` object and its :py:`Layer`\s also
both have a :py:`plot` method.


-------------------------------------------------------------------------------

**********
Parameters
**********

In order to create and run a Geonomics :py:`Model`, you will need a valid
Geonomics parameters file. No worry though -- this is very easy to create!
To generate a new, template parameters file, you will simply call the
:py:`gnx.make_parameters_file` function, feeding it the appropriate
arguments (to indicate how many :py:`Population`\s and :py:`Layer`\s you
want to include in your :py:`Model`; which parameters sections you want
included in the file, both for those
:py:`Layer`\s and :py:`Population`\s and for
other components of the :py:`Model`; and the path and filename for your new
parameters file). Geonomics will then automatically create the file for you, 
arranged as you requested and saved where you requested.

When you then open that file, you will see the following:

.. code-block:: python

  #<your_filename>.py

  #This is a default parameters file generated by Geonomics
  #(by the gnx.params.make_parameters_file() function).
  
  
                        ## :: ::    :::            ##
                  ##:::   :::::    :::   ::    :: :: :::##
               ## ::::     ::           ::   ::::::::::::::##
             ##::::::                       ::::::::: :::::: :##
           ## :    :::                    :::    ::    :::::::::##
          ##ggggg eeee ooo   n   n   ooo   m   m iiiii  cccc ssss##
         ##g     e    o   o  nn  n  o   o  m   m   i   c     s    ##
         ##g     eee o     o n n n o     o mm mm   i   c     sssss##
         ##g ggg eee o     o n  nn o     o m m m   i   c         s##
         ##g   g e    o   o  n   n  o   o  m   m   i   c        ss##
          ##gggg  eeee ooo   n   n   ooo   m   m iiiii  cccc ssss##
           ##  ::::::::        ::::::::::::  :       ::  ::   : ##
             ##  ::::              :::::::  ::     ::::::::  :##
               ## :::               :::::: ::       ::::::  ##
                  ##:                ::::                ##
                        ##                         ##
  
  
  params = {
  
  ##############
  #### LAND ####
  ##############
      'land': {
  
      ##############
      #### main ####
      ##############
          'main': {
              # dimensions of the Landscape
              'dim':                      (20,20),

     #.
     #.
     #.

This is the beginning of a file that is really just a long but simple Python
script (hence the '.py' extension); this whole file just defines a single,
long, nested :py:`dict` (i.e. a Python 'dictionary') containing all of your
parameter values. It may look like a lot, but don't be concerned! For two
reasons:

  1. All the hard work is already done for you. You'll just need to change
     the default values where and how you want to, to set up your particular
     simulation scenario.
  2. You will probably leave a good number of the parameters defined in this
     file untouched. Geonomics does its best to set sensible default values
     for all its parameters. Though of course, you'll want to think clearly 
     nonetheless about whether the default value for each parameter 
     is satisfactory for your purposes.

Each parameter in the parameters value is preceded by a terse comment, to
remind you what the parameter does. But for detailed information about each
parameter, you'll want to refer to the following information.
What follows is a list of all of the Geonomics parameters (in the sections and
the top-to-bottom order in which they'll appear in your parameters files).
For each parameter, you will see a section with the following information:

  - a snippet of the context (i.e. lines of
    Python code) in which it appears in a parameters file; 
  - the valid Python data type(s) the parameter can take
  - the default value of the parameter
  - a ranking score, indicating how likely it is that you will want to reset
    this parameter (i.e. change it from its default value), and
    encoded as follows:

    - 'Y': almost certainly, *or* must be reset for your :py:`Model` to run
    - 'P': it is possible/probable that you will want to reset this
      parameter, but this will depend on your use and scenario
    - 'N': almost certainly not, *or* no need to reset because it should be
      set intelligently anyhow (Note: this does *not* mean that you cannot
      reset the parameter! if that is the case for any value then it does not
      appear in the parameters file)

  - other relevant, detailed information about the parameter, including
    an explanation of what it defines, how its value is used, where to look
    for additioanl information about parameters related to other Python 
    packages, etcetera
   

These section will be formatted as follows:


**<param_name>**

.. code-block:: python

              #brief comment about the parameter
              '<param_name>':               <default_param_value>,

<valid Python data type(s)>

default: <default value>

reset? <ranking>

  <Explanation of what the parameter defines, how its value is used,
  and any other relevant information.>


This section should serve as your primary point of reference
if you confront any uncertainty while creating your own parameters files.
We'll start with the section of parameters that
pertains to the :py:`Landscape` object.


====================
Landscape parameters
====================

----
Main
----

------------------------------------------------------------------------------

**dim**

.. code-block:: python

              # dimensions of the Landscape
              'dim':                      (20,20),

:py:`tuple`

default: :py:`(20,20)`

reset: P
  
  This defines the x and y dimensions of the :py:`Landscape`,
  in units of cells. As you might imagine, these values are used 
  for a wide variety of basic operations throughout Geonomics. Change the
  default value to the dimensions of the landscape you wish to simulate on.


------------------------------------------------------------------------------

**res**

.. code-block:: python

              # resolution of the Landscape
              'res':                      (1,1),

:py:`tuple`
  
default: :py:`(1,1)`

reset: N

  This defines the :py:`Landscape` resolution (or cell-size) in the x and y
  dimensions. This information is only used if GIS rasters of :py:`Landscape` 
  layers are to be written out as GIS raster files (as parameterized in the
  'Data' parameters). Defaults to the meaningless value (1,1), and this value
  generally needn't be changed in your parameters file, because it will 
  be automatically updated to the resolution of any GIS rasters that 
  are read in for use as :py:`Layers` (assuming they all share the same
  resolution; otherwise, an Error is thrown). 


------------------------------------------------------------------------------

**ulc**

.. code-block:: python

              # upper-left corner of the Landscape
              'ulc':                      (0,0),

:py:`tuple`

default: :py:`(0,0)`

reset: N

  This defines the upper-left corner (ULC) of the 
  :py:`Landscape` (in the units of
  some real-world coordinate reference system, e.g. decimal degrees, or
  meters). This information is only used if GIS rasters of 
  :py:`Landscape` layers are to be written out as GIS raster files. 
  Defaults to the meaningless value
  (0,0), and this value usually needn't be changed in your parameters file,
  because it will be automatically updated to match the ULC value 
  of any GIS rasters that are read in for use as :py:`Layers` (assuming 
  they all share the same ULC; otherwise, an Error is thrown).

        
------------------------------------------------------------------------------

**prj**

.. code-block:: python
              
              #projection of the Landscape
              'prj':                      None,

:py:`str`; (WKT projection string)

default: :py:`None`

reset: N

  This defines the projection of the :py:`Landscape`, as a
  string of Well Known Text (WKT). 
  This information is only used if GIS rasters of :py:`Landscape` layers are
  to be written out as GIS raster files. Defaults to :py:`None`, which is fine,
  because this value will be automatically updated to match the projection
  of any GIS rasters that are read in for us as :py:`Layers` (assuming they
  all share the same projection; otherwise, an Error is thrown)



------
Layers
------

------------------------------------------------------------------------------

**layer_<n>**

.. code-block:: python
     
      ################
      #### layers ####
      ################
          'layers': {
              #layer name (LAYER NAMES MUST BE UNIQUE!) 
              'layer_0': {

{:py:`str`, :py:`int`}

default: :py:`layer_<n>` 

reset? P

This parameter defines the name for each :py:`Layer`. (Note that unlike most
parameters, it is a :py:`dict` key, the value for which is a :py:`dict`
of parameters defining the :py:`Layer` being named.) As the capitalized
reminder in the parameters states, each :py:`Layer` must have a unique name
(so that a parameterized :py:`Layer` isn't overwritten in the
:py:`ParametersDict` by a second, identically-named :py:`Layer`; Geonomics
checks for unique names and throws an Error if this condition is not met.
:py:`Layer` names can, but needn't be, descriptive of what each 
:py:`Layer` represents. Example valid values include: 0, 0.1, 'layer_0', 1994,
'1994', 'mean_ann_tmp'. Names default to :py:`layer_<n>`,
where n is a series of integers starting from 0.



^^^^
Init
^^^^

There are four different types of :py:`Layers` that can be created. The
parameters for each are explained in the next four subsections.

""""""
random
""""""

------------------------------------------------------------------------------

**n_pts**

.. code-block:: python
    
                      #parameters for a 'random'-type Layer
                      'rand': {
                          #number of random points
                          'n_pts':                        500,

:py:`int`

default: 500

reset? P

This defines the number of randomly located, randomly valued points
from which the random :py:`Layer` will be interpolated. (Locations drawn
from uniform distributions between 0 and the :py:`Landscape` dimensions on
each axis. Values drawn from a uniform distribution between 0 and 1.)


------------------------------------------------------------------------------

**interp_method**

.. code-block:: python

                          #interpolation method ('linear', 'cubic', or 'nearest')
                          'interp_method':                'cubic',
                          },

{:py:`'linear'`, :py:`'cubic'`, :py:`'nearest'`}

default: :py:`'cubic'`

reset? N

This defines the method to use to interpolate random points to the array that
will serve as the :py:`Layer`'s raster. Whichever of the three valid values
is chosen (:py:`'linear'`, :py:`'cubic'`, or :py:`'nearest'`) will be passed
on as an argument to :py:`scipy.interpolate.griddata`. Note that the
:py:`'nearest'` method will generate a random categorical array, such as
might be used for modeling habitat types.


"""""""
defined
"""""""

------------------------------------------------------------------------------

**pts**

.. code-block:: python
   
                      #parameters for a 'defined'-type Layer 
                      'defined': {
                          #point coordinates
                          'pts':                    None,

nx2 :py:`np.ndarray`

default: :py:`None`

reset? Y

This defines the coordinates of the points that will be used to 
interpolate this :py:`Layer`. 


------------------------------------------------------------------------------

**vals**

.. code-block:: python

                           #point values
                           'vals':                  None,

{:py:`list`, 1xn :py:`np.ndarray`}

default: :py:`None`

reset? Y

This defines the values of the points that will be used to 
interpolate this :py:`Layer`. 


------------------------------------------------------------------------------

**interp_method**

.. code-block:: python

                          #interpolation method {'linear', 'cubic', 'nearest'}
                          'interp_method':                'cubic',
                          },

{:py:`'linear'`, :py:`'cubic'`, :py:`'nearest'`}

default: :py:`'cubic'`

reset? N

This defines the method to use to interpolate random points to the array that
will serve as the :py:`Layer`'s raster. Whichever of the three valid values
is chosen (:py:`'linear'`, :py:`'cubic'`, or :py:`'nearest'`) will be passed
on as an argument to :py:`scipy.interpolate.griddata`. Note that the
:py:`'nearest'` method will generate a random categorical array, such as
might be used for modeling habitat types.


""""
file
""""

------------------------------------------------------------------------------

**filepath**

.. code-block:: python
  
                      #parameters for a 'file'-type Layer 
                      'file': {
                          #</path/to/file>.<ext>
                          'filepath':                     '/PATH/TO/FILE.EXT',

:py:`str`

default: :py:`'/PATH/TO/FILE.EXT'`

reset? Y

This defines the location and name of the file that should be read in as the
raster-array for this :py:`Layer`. Valid file types include a '.txt' file
containing a 2d :py:`np.ndarray`, or any GIS raster file that can be read
by :py:`osgeo.gdal.Open`. In all cases, the raster-array read in from the
file must have dimensions equal to the stipulated dimensions of the
:py:`Landscape` (as defined in the **dims** parameter, above); otherwise,
Geonomics will throw an Error.


------------------------------------------------------------------------------

**scale_min_val**

.. code-block:: python

                          #minimum value to use to rescale the Layer to [0,1]
                          'scale_min_val':                None,

{:py:`float`, :py:`int`}

default: :py:`None`

reset? P

This defines the minimum value (in the units of the variable represented by
the file you are reading in) to use when rescaling the file's array to
values between 0 and 1. (This is done to satisfy the requirement that all
Geonomics :py:`Layer`\s have arrays in that interval). Defaults to :py:`None`
(in which case Geonomics will set it to the minimum value observed in this
file's array). But note that you should put good thought into
this parameter, because it *won't* necessarily be the minimum value
observed in the file; for example, if this file is being used
to create a :py:`Layer` that will undergo environmental change
in your `Model`, causing its real-world values to drop
below this file's minimum value, then you will probably want to set
this value to the minimum real-world value that will occur for this :py:`Layer`
during your :py:`Model` scenario, so that low values
that later arise on this `Layer` don't get truncated at 0.


------------------------------------------------------------------------------

**scale_max_val**

.. code-block:: python

                          #maximum value to use to rescale the Layer to [0,1]
                          'scale_max_val':                None,

{:py:`float`, :py:`int`}

default: :py:`None`

reset? P

This defines the maximum value (in the units of the variable represented by
the file you are reading in) to use when rescaling the file's array to
values between 0 and 1. (This is done to satisfy the requirement that all
Geonomics :py:`Layer`\s have arrays in that interval). Defaults to :py:`None`
(in which case Geonomics will set it to the maximum value observed in this
file's array). But note that you should put good thought into
this parameter, because it *won't* necessarily be the maximum value
observed in the file; for example, if this file is being used
to create a :py:`Layer` that will undergo environmental change
in your `Model`, causing its real-world values to increase
above this file's maximum value, then you will probably want to set
this value to the maximum real-world value that will occur for this 
:py:`Layer` during your :py:`Model` scenario, so that high values that 
later arise on this `Layer` don't get truncated at 1.

"""""
nlmpy
"""""

------------------------------------------------------------------------------

**function**

.. code-block:: python

                      #parameters for an 'nlmpy'-type Layer
                      'nlmpy': {
                          #nlmpy function to use the create this Layer
                          'function':                 'mpd',

:py:`str` that is the name of an :py:`nlmpy` function

default: :py:`'mpd'`

reset? P

This indicates the :py:`nlmpy` function that should be used to generate
this :py:`Layer`'s array. (:py:`nlmpy` is a Python package for
generating neutral landscape models; NLMs.) Defaults to :py:`'mpd'` (the
function for creating a midpoint-displacement NLM). Can be set to any other
:py:`str` that identifies a valid :py:`nlmpy` function, but then the
remaining parameters in this section must be changed to the parameters
that that function needs, and *only* those parameters 
(because they will be unpacked into this function,
i.e. passed on to it, at the time it is called.
(Visit the `Cheese Shop <https://pypi.org/project/nlmpy/>`_ for more 
information about the :py:`nlmpy` package and available functions).


------------------------------------------------------------------------------

**nRow**

.. code-block:: python

                          #number of rows (MUST EQUAL LAND DIMENSION y!)
                          'nRow':                     20,


:py:`int`

default: 20

reset? P

This defines the number of rows in the :py:`nlmpy` array that is created.
As the capitalized reminder in the parameters file mentions, this must be
equal to the y-dimension of the :py:`Landscape`; otherwise, an error
will be thrown. Note that this parameter (as for the remaining parameters in
this section, other than the **function** parameter) is valid for the
default :py:`nlmpy.mpd` function that is set by the
**function** parameter); if you are using a different :py:`nlmpy`
function to create this :py:`Layer` then this and the remaining parameters
must be changed to the parameters that that function needs, 
and *only* those parameters (because they will be unpacked into that function,
i.e. passed on to it, at the time it is called).


------------------------------------------------------------------------------

**nCol**

.. code-block:: python

                          #number of cols (MUST EQUAL LAND DIMENSION x!)
                          'nCol':                     20,


:py:`int`

default: 20

reset? P

This defines the number of columns in the :py:`nlmpy` array that is created.
As the capitalized reminder in the parameters file mentions, this must be
equal to the x-dimension of the :py:`Landscape`; otherwise, an error
will be thrown. Note that this parameter (as for the remaining parameters in
this section, other than the **function** parameter) is valid for the
default :py:`nlmpy.mpd` function that is set by the
**function** parameter); if you are using a different :py:`nlmpy`
function to create this :py:`Layer` then this and the remaining parameters
must be changed to the parameters that that function needs, 
and *only* those parameters (because they will be unpacked into that function,
i.e. passed on to it, at the time it is called).


------------------------------------------------------------------------------

**h**

.. code-block:: python

                          #level of spatial autocorrelation in element values
                          'h':                     1,


:py:`float`

default: 1

reset? P

This defines the level of spatial autocorrelation in the element values
of the :py:`nlmpy` array that is created.
Note that this parameter (and the remaining parameters in
this section, other than the **function** parameter) is valid for the
default :py:`nlmpy` function (:py:`nlmpy.mpd`, which is set by the
**function** parameter); but if you are using a different :py:`nlmpy`
function to create this :py:`Layer` then this and the remaining parameters
must be changed to the parameters that that function needs, 
and *only* those parameters (because they will be unpacked into that function,
i.e. passed on to it, at the time it is called).


^^^^^^
Change
^^^^^^

------------------------------------------------------------------------------

**end_rast**

.. code-block:: python

                  #land-change event for this Layer
                  'change': {
                      #end raster for event (DIM MUST EQUAL DIM OF LAND!)
                      'end_rast':         np.zeros((20,20)),

{2d :py:`np.ndarray`, :py:`str`}

default: :py:`np.zeros((20,20))`

reset? Y

This defines the end raster for a :py:`Landscape`-change event (i.e. the array 
this :py:`Layer` will change into over the course of the event). As the
capitalized reminder in the parameters file mentions, this raster must of
course have the same dimensions as the `Landscape` to which the `Layer` belongs;
otherwise, Genomics will throw an Error. Defaults a placeholder array
of zeros, so should be parameterized to meet your needs. Valid values are
a 2-d :py:`np.ndarray` object (stipulating the raster itself), or a :py:`str`
pointing to a file containing the raster (where valid files can be a '.txt'
file holding a 2-d :py:`np.ndarray` or any GIS raster files that can be
read by :py:`osgeo.gdal.Open`).


------------------------------------------------------------------------------

**start_t**

.. code-block:: python

                   #starting timestep of event
                   'start_t':          49,

:py:`int`

default: 49

reset? P

This indicates the timestep on which the :py:`Landscape`-change event
will begin. Defaults to 49, but should be set to suit your
specific scenario.


------------------------------------------------------------------------------

**end_t**

.. code-block:: python

                   #ending timestep of event
                   'end_t':          99,

:py:`int`

default: 99

reset? P

This indicates the timestep on which the :py:`Landscape`-change event
will finish. Defaults to 99, but should be set to suit your
specific scenario.


------------------------------------------------------------------------------

**n_steps**

.. code-block:: python

                   #number of stepwise changes in event
                   'n_steps':          5,

:py:`int`

default: 5

reset? P

This indicates the number of stepwise changes to use to model a
:py:`Landscape`-change event. The changes during a :py:`Landscape`-change event
are linearly interpolated (cellwise for the whole :py:`Layer`) to this
number of discrete, instantaneous :py:`Landscape` changes between
the starting and ending rasters. Thus, the fewer the number of 
steps, the larger, magnitudinally, each change will be. So generally, more
steps is 'better', as it will better approximate change that is continuous
in time. However, there is a potenitally significant memory trade-off here:
The whole series of stepwise-changed arrays is computed when the
:py:`Model` is created, then saved and used at the appropriate timestep
during each :py:`Model` run (and if the :py:`Layer` that is changing is used
by any :py:`Population` as a :py:`_MovementSurface` then each intermediate
:py:`_MovementSurface` is also calculated when the :py:`Model` is first
built. These objects take up memory, which may be limiting for larger
:py:`Model`\s and/or :py:`Landscape` objects. This will probably not be a
major issue, but is worth considering.


====================
Community parameters
====================

-----------
Populations
-----------


------------------------------------------------------------------------------

**pop_<n>**

.. code-block:: python
 
              #pop name (POPULATION NAMES MUST BE UNIQUE!) 
              'pop_0' :   {

{:py:`str`, :py:`int`}

default: :py:`pop_<n>` 

reset? P

This parameter defines the name for each :py:`Population`.
(Note that unlike most
parameters, it is a :py:`dict` key, the value for which is a :py:`dict`
of parameters defining the :py:`Layer` being named.) As the capitalized
reminder in the parameters states, each :py:`Population`
must have a unique name (so that a parameterized 
:py:`Population` isn't overwritten in the :py:`ParametersDict` by a
second, identically-named :py:`Population`; Geonomics
checks for unique names and throws an Error if this condition is not met.
:py:`Population` names can, but needn't be, descriptive of what each 
:py:`Population` represents. Example valid values include: 0, 'pop0',
'high-dispersal', 'C. fasciata'. Names default to 
:py:`pop_<n>`, where n is a series of
integers starting from 0.

^^^^
Init
^^^^

------------------------------------------------------------------------------

**N**

.. code-block:: python
  
                  'init': {
                      #starting population size
                      'N':                250,

:py:`int`

default: 250

reset? P

This defines the starting size of this :py:`Population`. Importantly, this
may or may not be near the stationary size of the :py:`Population` after
the :py:`Model` has burned in, because that size will depend on the
carrying-capacity raster (set by the **K** parameter), and on
the dynamics of specific a :py:`Model` (because of the interaction of
its various parameters).


------------------------------------------------------------------------------

**K_layer**

.. code-block:: python

                      #name of the carrying-capacity Layer
                      'K_layer':         'layer_0',

:py:`str`

default: 'layer_0'

reset? P

This indicates, by name, the :py:`Layer` to be used as the
carrying-capacity raster for a :py:`Population`. The values of this
:py:`Layer` should express the carrying capacity at each cell, in number
of :py:`Individual`\s. Note that the sum of the values of this :py:`Layer`
can serve as a rough estimate of the expected stationary 
:py:`Population` size; however, observed stationary size could vary
substantially depending on various other :py:`Model` parameters (e.g. birth
and death rates and mean number of offspring per mating event) as well
as on stochastic events (e.g. failure to colonize, or survive in, all
habitable portions of the :py:`Landscape`).


^^^^^^
Mating
^^^^^^

------------------------------------------------------------------------------

**repro_age**

.. code-block:: python

                  'mating'    : {
                      #age(s) at sexual maturity (if tuple, female first)
                      'repro_age':            0,

{:py:`int`, :py:`(int, int)`, :py:`None`}

default: 0

reset? P

This defines the age at which :py:`Individual`\s in the :py:`Population`
can begin to reproduce. If the value provided is a 2-tuple of different
numbers (and the :py:`Population` uses separate sexes), then the first
number will be used as females' reproductive age, the second as males'.
If the value is 0, or :py:`None`, :py:`Individual`\s are capable
of reproduction from time of time.


------------------------------------------------------------------------------

**sex**

.. code-block:: python
        
                      #whether to assign sexes
                      'sex':                  False,

:py:`bool`

default: False

reset? P

This determines whether :py:`Individual`\s will be assigned separate sexes
that are used to ensure only male-female mating events.


------------------------------------------------------------------------------

**sex_ratio**

.. code-block:: python
                        
                      #ratio of males to females
                      'sex_ratio':            1/1,


{:py:`float`, :py:`int`}

default: 1/1

reset? P

This defines the ratio of males to females (i.e. it will be converted to
a probability that an offspring is a male, which is used as the probability
of a Bernoulli draw of that offspring's sex). 


------------------------------------------------------------------------------

**distweighted_birth**

.. code-block:: python

                      #whether P(birth) should be weighted by parental dist
                      'distweighted_birth':  False,


#NOTE: I WILL PROBABLY GET RID OF THIS PARAMETER...


------------------------------------------------------------------------------

**R**

.. code-block:: python

                      #pop intrinsic growth rate
                      'R':                    0.5,

:py:`float`

default: 0.5

reset? P

This defines a :py:`Population`'s intrinsic growth rate, which is used
as the 'R' value in the spatialized logistic growth equation that
regulates population density (:math:`\frac{\mathrm{d}
N_{x,y}}{\mathrm{d}t}=rN_{x,y}(1-\frac{N_{x,y}}{K_{x,y}})`).


------------------------------------------------------------------------------

**b**

.. code-block:: python
                       
                      #pop instrinsic birth rate (MUST BE 0<=b<=1)
                      'b':                    0.2,

:py:`float` in interval [0, 1]

default: 0.2

reset? P

This defines a :py:`Population`'s intrinsic birth rate, which is
implemented as the probability that an identified potential mating
pair successfully produces offspring. Because this is a probability, as
the capitalized reminder in the parameters file mentions, this value must
be in the inclusive interval [0, 1].

NOTE: this may later need to be re-implemented to allow for spatial
variation in intrinsic rate (i.e.. expression of a birth-rate raster),
and/or for density-dependent birth as well as mortality


------------------------------------------------------------------------------

**n_births_dist_lambda**

.. code-block:: python

                      #expectation of distr of n offspring per mating pair
                      'n_births_distr_lambda':      1,

{:py:`float`, :py:`int`}

default: 1

reset? P

This defines the lambda parameter for the Poisson distribution from 
which a mating pair's number of offspring is drawn. Hence it is the
expected value for the number of offspring born in a
successful mating event.


------------------------------------------------------------------------------

**mating_radius**

.. code-block:: python

                      #radius of mate-search area
                      'mating_radius':        1

{:py:`float`, :py:`int`}

default: 1

reset? Y

This defines the radius within which an :py:`Indvidual` can find a mate.
This radius is provided to queries run on the :py:`_KDTree` object.


^^^^^^^^^
Mortality
^^^^^^^^^

------------------------------------------------------------------------------

**max_age**

.. code-block:: python
                        
                      #maximum age
                      'max_age':              1,

{:py:`int`, :py:`None`}

default: 1

reset? P

This defines the maximum age an individual can achieve before being
forcibly culled from the population. Defaults to 1 (which will create
a Wright-Fisher-like simulation, with discrete generations). Can be set
to any other age, or can be set to :py:`None` (in which case no maxmimum
age is enforced).


------------------------------------------------------------------------------

**d_min**

.. code-block:: python
        
                      #min P(death) (MUST BE 0<=d_min<=1)
                      'd_min':                     0.01,

:py:`float` in interval [0, 1]

default: 0.01

reset? N

This defines the minimum probabilty of death that an :py:`Individual`
can face each time its Bernoulli death-decision is drawn. Because this 
is a probability, as the capitalized reminder in 
the parameters file mentions, this value must be in the 
inclusive interval [0, 1].

------------------------------------------------------------------------------

**d_max**

.. code-block:: python

                      #max P(death) (MUST BE 0<=d_max<=1)
                      'd_max':                    0.99,

:py:`float` in interval [0, 1]

default: 0.99

reset? N

This defines the minimum probabilty of death that an :py:`Individual`
can face each time its Bernoulli death-decision is drawn. Because this 
is a probability, as the capitalized reminder in 
the parameters file mentions, this value must be in the 
inclusive interval [0, 1].


------------------------------------------------------------------------------

**density_grid_window_width**


.. code-block:: python

                  'mortality'     : {
                      #width of window used to estimate local pop density
                      'dens_grid_window_width':   None,

{:py:`float`, :py:`int`, :py:`None`}

default: None

reset? N

This defines the width of the window used by the :py:`_DensityGridStack`
to estimate a raster of local :py:`Population` densities. The user should
feel free to set different values for this parameter (which could be
especially helpful when calling :py:`Model.plot_density` to inspect the
resulting surfaces calculated at different window widths, if trying
to heuristically choose a reasonable value to set for a
particular simulation scenario). But be aware that choosing particularly
small window widths (in our experience, windows smaller than ~1/20th of
the larger :py:`Landscape` dimension) will cause dramatic increases in the 
run-time of the density calculation (which runs twice per timestep).
Defaults to :py:`None`, which internally will be set to 1/10th of the
larger :py:`Landscape` dimension; for many purposes this will work, but for
others the user may wish to control this.


^^^^^^^^
Movement
^^^^^^^^

------------------------------------------------------------------------------

**direction_distr_mu**

.. code-block:: python
 
                'movement': {
                     #mode of distr of movement direction
                     'direction_distr_mu':     1,

{:py:`int`, :py;`float`}

default: 1

reset? N

This is the :math:`\mu` parameter of the VonMises distribution
(a circularized Gaussian distribution) from which
movement directions are chosen when movement is random and isotropic 
(rather than
being determined by a :py:`_MovementSurface`; if a :py:`_MovementSurface`
is being usen this parameter is ignored). The :math:`\kappa` value
that is fed into this same distribution (**direction_distr_kappa**)
causes it to be very dispersed,
such that the distribution is effectively a uniform distribution on 
the unit circle (i.e. all directions are effectively equally probable).
For this reason, changing this parameter without changing the 
**direction_distr_kappa** value also, will make no change in the directions
drawn for movement.  If random, isotropic
movement is what you aim to model then there is probably little reason 
to change these parameters.


**direction_distr_kappa**

.. code-block:: python

                     #concentration of distr of movement direction
                     'direction_distr_kappa':  0,

{:py:`int`, :py:`float`}

default: 0

reset? N

This is the :math:`\kappa` parameter of the VonMises distribution
(a circularized Gaussian distribution) from which
movement directions are chosen when movement is random and isotropic 
(rather than
being determined by a :py:`_MovementSurface`; if a :py:`_MovementSurface`
is being usen this parameter is ignored). The default value of 0 will  
cause this distribution to be very dispersed, approximating a uniform
distribution on the unit circle and rendering the :math:`\mu`
value (**direction_distr_mu**) effectively meaningless. However, as this
parameter's value increases the resulting circular distributions will become
more concentrated around :math:`\mu`, making the value fed to
**direction_distr_mu** influential. If random, isotropic
movement is what you aim to model then there is probably little reason 
to change these parameters.


**distance_distr_mu**

.. code-block:: python

                     #mean of distr of movement distance
                     'distance_distr_mu':      0.5,

{:py:`int`, :py:`float`}

default: 0.5

reset? Y

This is the :math:`\mu` parameter of the Wald distribution used to draw
movement distances, expressed in units of raster-cells. This parameter and
**distance_distr_sigma** (the Wald distribution's :math:`sigma`) should be
set to reflect a distribution of movement distances that is appropriate
for your scenario.


**distance_distr_sigma**

.. code-block:: python

                     #variance of distr of movement distance
                     'distance_distr_sigma':   0.5,

{:py:`int`, :py:`float`}

default: 0.5 

reset? Y

This is the :math:`\sigma` parameter of the Wald distribution used to draw
movement distances, expressed in units of raster-cells. This parameter and
**distance_distr_mu** (the Wald distribution's :math:`mu`) should be
set to reflect a distribution of movement distances that is appropriate
for your scenario.


**dispersal_distr_mu**

.. code-block:: python

                     #mean of distr of dispersal distance
                     'dispersal_distr_mu':     0.5,

{:py:`int`, :py:`float`}

default: 0.5

reset? Y

This is the :math:`\mu` parameter of the Wald distribution used to draw
dispersal distances, expressed in units of raster-cells. This parameter and
**distance_distr_sigma** (the Wald distribution's :math:`sigma`) should be
set to reflect a distribution of dispersal distances that is appropriate
for your scenario.


**dispersal_distr_sigma**

.. code-block:: python

                     #variance of distr of dispersal distance
                     'dispersal_distr_sigma':  0.5,
                 
{:py:`int`, :py:`float`}

default: 0.5

reset? Y

This is the :math:`\sigma` parameter of the Wald distribution used to draw
dispersal distances, expressed in units of raster-cells. This parameter and
**distance_distr_mu** (the Wald distribution's :math:`mu`) should be
set to reflect a distribution of dispersal distances that is appropriate
for your scenario.


""""""""""""""""
_MovementSurface
""""""""""""""""

**layer**

.. code-block:: python

                     'move_surf'     : {
                         #move-surf Layer name
                         'layer':                'layer_0',

:py:`str`

default: :py:`'layer_0'`

reset? P

This indicates, by name, the :py:`Layer` to be used as to construct the
:py:`_MovementSurface` for a :py:`Population`. Note that this can also
be thought of as the :py:`Layer` that should serve as a
:py:`Population`'s permeability raster (because :py:`Individual`\s moving
on this :py:`_MovementSurface` toward the higher (if mixture distributions
are used) or highest (if unimodl distributions are used)
values in their neighborhoods). 


**mixture**

.. code-block:: python

                         #whether to use mixture distrs
                         'mixture':              True,

:py:`bool`

default: True

reset? P

This indicates whether the :py:`_MovementSurface` should be built using
VonMises mixture distributions or unimodal VonMises distributions. 
If True, each cell in the :py:`_MovementSurface` will have an approximate
circular distribution that is a
weighted sum of 8 unimodal VonMises distributions (one per cell in the 8-cell
neighborhood); each of those summed unimodal distributions will have as its 
mode the direction of the neighboring cell on which it is based and as its 
weight the relative permeability of the cell on which it is based 
(relative to the full neighborhood). If False, each cell in the
:py:`_MovementSurface` will have an approximated circular distribution 
that is a single
VonMises distribution with its mode being the direction of the maximum-valued
cell in the 8-cell neighborhood and its concentration determined by
**vm_distr_kappa**.


**vm_distr_kappa**

.. code-block:: python

                         #concentration of distrs
                         'vm_distr_kappa':       12,

{:py:`int`, :py:`float`}

default: 12 

reset? N

This sets the concentration of the VonMises distributions used to build
the approximated circular distributions in the :py:`_MovementSurface`.
The default value was chosen heuristically as one that provides a reasonable
concentration in the direction of a unimodal VonMises distribution's mode 
without causing VonMises mixture distributions built from an 
evenly weighted sum of distributions pointing toward the 
8-cell-neighborhood directions to have 8 pronounced modes. 
There will probably be little need to change the default value, but if
interested then the user could create :py:`Model`\s with various values
of this parameter and then use the :py:`Model.plot_movement_surface`
method to explore the influence of the parameter on the resulting
:py:`_MovementSurface`\s.


                   
------------------------------------------------------------------------------

^^^^^^^^^^^^^^^^^^^^
_GenomicArchitecture
^^^^^^^^^^^^^^^^^^^^

**gen_arch_file**

.. code-block:: python

                  'gen_arch': {
                      #/path/to/file.csv defining custom genomic arch
                      'gen_arch_file':            None,

{:py:`str`, :py:`None`}

default: {:py:`None`, :py:`'<your_model_name>_pop-<n>_gen_arch.csv'`

reset? P

This arguments indicates whether a custom genomic architecture file should
be used to create a :py:`Population`'s :py:`GenomicArchitecture`, and if so,
where that file is located. If the value is :py:`None`, no file will be
used and the values of this :py:`Population`'s other genomic
architecture parameters in the parameters file will be used to create
the :py:`GenomicArchitecture`. If the value is a :py:`str` pointing to a
custom genomic-architecture file 
(i.e. a CSV file with loci as rows and 'locus_num',
'p', 'dom', 'r', 'trait', and 'alpha' as columns stipulating the starting
allele frequencies, dominance values, inter-locus recombination rates,
trait names, and effect sizes of all loci). Geonomics will create an empty
file of this format for each :py:`Population` for which the 
'custom_genomic_architecture' argument is provided the value True when
:py:`gnx.make_parameters_file` is called (which will be saved as
'<your_model_name>_pop-<n>_gen_arch.csv'). 

Note that when Geonomics reads in a custom genomic architecture file
to create a :py:`Model`, it will check
that the length (i.e. number of rows) in this file is equal to the length
stipulated by the **L** parameter, and will also check that the first value
at the top of the 'r' column is 0.5 (which is used to implement independent
assortment during gametogenesis). If either of these checks fails,
Geonomics throws an Error.


**L**

.. code-block:: python
 
                      #num of loci
                      'L':                        1000,

:py:`int`

default: 1000

reset? P

This indicates the total number of loci that the genomes in a
:py:`Population` should have.


**l_c**

.. code-block:: python
                        
                      #num of chromosomes
                      'l_c':                      [750, 250],

:py:`list` of :py:`int`\s

default: :py:`[750, 250]`

reset? P

This indicates the lengths (in number of loci) of each of the chromosomes 
in the genomes in a :py:`Population`.  Note that the sum of this :py:`list`
must equal **L**, otherwise Geonomics will throw an Error. 
Also note that Geonomics models genomes as single **L** x 2
arrays, where separate chromosomes are delineated by points along
the genome where the recombination rate is 0.5;
thus, for a model where recombination rates are often at or near 0.5, this
parameter will have little meaning.

.. code-block:: python


                      #genome-wide per-base neutral mut rate (0 to disable)
                      'mu_neut':                  1e-9,
                      #genome-wide per-base deleterious mut rate (0 to disable)
                      'mu_delet':                 0,
                      #whether to save mutation logs
                      'mut_log':                  False,
                      #shape of distr of deleterious effect sizes
                      'delet_s_distr_shape':      0.2,
                      #scale of distr of deleterious effect sizes
                      'delet_s_distr_scale':      0.2,
                      #alpha of distr of recomb rates
                      'r_distr_alpha':            0.5,
                      #beta of distr of recomb rates
                      'r_distr_beta':             15e9,
                      #whether loci should be dominant (for allele '1')
                      'dom':                      False,
                      #whether to allow pleiotropy
                      'pleiotropy':               False,
                      #custom fn for drawing recomb rates
                      'recomb_rate_custom_fn':    None,
                      #number of recomb paths to hold in memory
                      'recomb_lookup_array_size': int(1e3),
                      #total number of recomb paths to simulate
                      'n_recomb_paths':           int(1e4),
                        




                      'gen_arch_file':            None,
                          #if not None, should point to a file stipulating a
                              #custom genomic architecture (i.e. a CSV with loci
                              #as rows and 'locus_num', 'p', 'dom', 'r', 'trait',
                              #and 'alpha' as columns, such as is created by
                              #main.make_parameters_file, when the custom_gen_arch
                              #arugment is True)
                      'mu_neut':                  1e-9,
                          #genome-wide neutral mutation rate, per base per generation
                              #(set to 0 to disable neutral mutation)
                      'mu_delet':                 0,
                          #genome-wide deleterious mutation rate, per base per generation
                              #(set to 0 to disable deleterious mutation)
                              #NOTE: these mutations will fall outside the loci involved in any traits
                              #being simulated, and are simply treated as universally deleterious, with the same
                              #negative influence on fitness regardless of spatial context
                      'mut_log':                  False,
                          #whether or not to store a mutation log; if true, will be saved as mut_log.txt
                          #within each iteration's subdirectory
                      'delet_s_distr_shape':      0.2,
                      'delet_s_distr_scale':      0.2,
                          #mean and standard deviation of the gamma distribution
                          #parameterizig the per-allele effect size of 
                          #deleterious mutations (std = 0 will fix all mutations
                          #for the mean value)
                      'r_distr_alpha':            0.5,
                          #alpha for beta distribution of linkage values
                              #NOTE: alpha = 14.999e9, beta = 15e9 has a VERY sharp peak on D = 0.4998333,
                              #with no values exceeding equalling or exceeding 0.5 in 10e6 draws in R
                      'r_distr_beta':             15e9,
                          #beta for beta distribution of linkage values
                      'dom':                      False,
                          #whether or not loci should be dominant 
                          #(if True, the 1 allele will be dominant at each locus;
                          #if False, all loci will be codominant; defaults to False)
                      'pleiotropy':               True,
                          #allow pleiotropy? (i.e. allow same locus to affect value of more than one trait?) false
                      'recomb_rate_custom_fn':    None,
                          #if provided, must be a function that returns a single recombination rate value (r) when called
                      'recomb_lookup_array_size': int(1e3),
                          #the size of the recombination-path lookup array to have
                              #read in at one time (needs to be comfortably larger than the anticipated totaly number of
                              #recombination paths to be drawn at once, i.e. than 2 times the anticipated most number of births at once)
                      'n_recomb_paths':           int(1e4),
                          #the total number of distinct recombination paths to
                              #generate at the outset, to approximate truly free recombination at the recombination rates specified
                              #by the genomic architecture (hence the larger the value the less the likelihood of mis-approximation artifacts)
  
                      'traits': {
        #########
                          #trait 0#
                          #########
                          0: {
                          #trait name; each trait must get a unique numeric or
                          #string name (e.g. 0, 'trt0', 'bill_length'); trait
                          #names default to serial integers from 0
                              'scape_num':        2,
                                  #the landscape numbers to be used for selection on this trait
                              'phi':              0.1,
                                  #phenotypic selection coefficient for this trait; can either be a
                                      #numeric value, or can be an array of spatialized selection
                                      #values (with dimensions equal to land.dims)
                              'n_loci':           1,
                                  #number of loci to be assigned to this trait
                              'mu':      1e-9,
                                  #mutation rate for this trait (if set to 0, or if genome['mutation'] == False, no mutation will occur)
                                      #(set to 0 to disable mutation for this trait)
                              'alpha_distr_mu' : 0,
                              'alpha_distr_sigma' : 0.5,
                                  #the mean and standard deviation of the normal distribution used to choose effect size
                                      #(alpha) for this trait's loci
                                      #NOTE: for mean = 0, std = 0.5, one average locus is enough to generate both optimum
                                      #genotypes; for mean = 0, std = 0.025, 10 loci should generate both (on average, but depends of course on
                                      #the random sample of alphas drawn); and so on linearly
                              'gamma':            1,
                                  #gamma exponent for the trait's fitness function (determines the shape of the
                                  #curve of fitness as a function of absolute difference between an individual's
                                  #phenotype and its environment; <1 = concave up, 1 = linear, >1 = convex up)
                              'univ_adv':      False
                                  #is the trait universally advantageous? if so, phenotypes closer to 1 will
                                      #have higher fitness at all locations on the land
                              }, # <END> trait 0
        #########
                          #trait 1#
                          #########
                          1: {
                          #trait name; each trait must get a unique numeric or
                          #string name (e.g. 0, 'trt0', 'bill_length'); trait
                          #names default to serial integers from 0
                              'scape_num':        2,
                                  #the landscape numbers to be used for selection on this trait
                              'phi':              0.1,
                                  #phenotypic selection coefficient for this trait; can either be a
                                      #numeric value, or can be an array of spatialized selection
                                      #values (with dimensions equal to land.dims)
                              'n_loci':           1,
                                  #number of loci to be assigned to this trait
                              'mu':      1e-9,
                                  #mutation rate for this trait (if set to 0, or if genome['mutation'] == False, no mutation will occur)
                                      #(set to 0 to disable mutation for this trait)
                              'alpha_distr_mu' : 0,
                              'alpha_distr_sigma' : 0.5,
                                  #the mean and standard deviation of the normal distribution used to choose effect size
                                      #(alpha) for this trait's loci
                                      #NOTE: for mean = 0, std = 0.5, one average locus is enough to generate both optimum
                                      #genotypes; for mean = 0, std = 0.025, 10 loci should generate both (on average, but depends of course on
                                      #the random sample of alphas drawn); and so on linearly
                              'gamma':            1,
                                  #gamma exponent for the trait's fitness function (determines the shape of the
                                  #curve of fitness as a function of absolute difference between an individual's
                                  #phenotype and its environment; <1 = concave up, 1 = linear, >1 = convex up)
                              'univ_adv':      False
                                  #is the trait universally advantageous? if so, phenotypes closer to 1 will
                                      #have higher fitness at all locations on the land
                              }, # <END> trait 1
  
  
      #### NOTE: Individual Traits' sections can be copy-and-pasted (and
      #### assigned distinct keys and names), to create additional Traits.
  
  
                          }, # <END> 'traits'
  
                      }, # <END> 'gen_arch'
  
  
              ############################
              #### pop num. 0: change ####
              ############################
  
                  'change': {
  
                      'dem': {
                          #(all population sizes are expressed relative to the carrying-capacity
                              #raster at the time that the demographic change event begins (i.e. as
                              #factors by which pop.K will be multiplied; thus they can also be thought
                              #of as multipliers of the expected total population size (i.e. pop.K.sum())
                              #and they will generally change the average population size by that multiple,
                              #but of course not precisely, because population size is stochastic. If you
                              #seek exact control of total population size, please seek a simpler simulation
                              #model, perhaps a coalescent one.
  
                          0: {
                              #can add an arbitrary number of demographic change events for
                                  #each population, each event identified by a distinct integer
                              'kind':             'custom',
                                  #what kind of change event? ('monotonic', 'stochastic', 'cyclical', 'custom')
                              'start':            200,
                                  #at which timestep should the event start?
                              'end':              1200,
                                  #at which timestep should the event end?
                              'rate':             .98,
                                  #at what rate should the population change each timestep
                                      #(positive for growth, negative for reduction)
                              'interval':         11,
                                  #at what interval should stochastic change take place (None defaults to every timestep)
                              'dist':             'uniform',
                                  #what distribution to draw stochastic population sizes from (valid values: 'uniform', 'normal')
                              'size_target':      None,
                                  #what is the target size of the demographic change event (defaults to None)
                              'n_cycles':         20,
                                  #how many cycles of cyclical change should occur during the event?
                              'size_range':       (0.5, 1.5), 
                                  #an iterable of the min and max population sizes to be used in stochastic or cyclical changes
                              'timesteps':        [6,8],
                                  #at which timesteps should custom changes take place?
                              'sizes':            [2,0.25],
                                  #what custom size-changes should occur at the above-stipulated timesteps?
                              } # <END> event 0
   
  
  
  
      #### NOTE: Individual demographic change events' sections can be
      #### copy-and-pasted (and assigned distinct keys and names), to create
      #### additional events.
  
  
                          }, # <END> 'dem'
  
                      'parameters': {
                          #other (i.e. non-demographic) population change events
                          'b': {
                              #the life-history parameters to be changed should be the keys in this dict,
                                  #and values are dictionaries containing a list of timesteps
                                  #at which to changed those parameters and a list of values
                                  #to which to change them
                              'timesteps':        None,
                              'vals':           None
                                  }
  
  
      #### NOTE: Individual life-history paramter change events' sections can be
      #### copy-and-pasted (and assigned distinct keys and names), to create
      #### additional events.
  
  
                              }, # <END> 'parameters'
  
                          } # <END> 'change'
  
                  }, # <END> pop num. 0
  
  
  
      #### NOTE: Individual Populations' sections can be copy-and-pasted (and
      #### assigned distinct keys and names), to create additional Populations.
  
  
              }, # <END> 'pops'
  
          }, # <END> 'comm'
  
  ###############
  #### MODEL ####
  ###############
      'model': {
          'time': {
              #parameters to control the number of burn-in and main timesteps to
              #run for each iterations
              'T':            100,
                  #total model runtime (in timesteps)
              'burn_T':       30
                  #minimum burn-in runtime (in timesteps; this is a mininimum because
                      #burn-in will run for at least this long but until
                      #stationarity detected, which will likely be longer)
              }, # <END> 'timesteps'
  
          'its': {
              #parameters to control how many iterations of the model to run,
              #and whether or not to randomize the land and/or community
              #objects in each model iteration
              'n_its': 1,
                  #how many iterations of the model should be run?
              'rand_land':    False,
                  #randomize the land for each new iteration?
              'rand_comm':    False,
                  #randomize the community for each new iteration?
              'rand_burn':  False,
                  #randomize the burn-in for each new iteration? (i.e. burn in
                  #each time, or burn in once at creation and then use the same
                  #burnt-in population for each iteration?)
              }, # <END> 'iterations'
  
  
          'data': {
              #dictionary defining the data to be collected, the sampling
              #strategy to use, the timesteps for collection, and other parameters
              'sampling': {
                  #args to be unpacked into sampling function (see docstring
                      #of sample_data function in data module for details)
                  'scheme':               'random',
                      #valid: 'all', 'random', 'point', or 'transect'
                  'n':                    50,
                      #size of samples to be collected (in number of individuals)
                  'points':               None,
                      #the x,y points at which data should be sampled (expressed
                          #as a list or tuple of length-2 lists or 2-tuples)
                  'transect_endpoints':   None,
                      #endpoints of the transect to be sampled (only needed if
                          #scheme is 'transect), expressed as a pair of
                          #ordered x,y pairs (in tuples or lists)
                  'n_transect_points':    None,
                      #the number of evenly spaced points along the transect
                      #at which to sample (only needed if scheme is 'transect')
                  'radius':               None,
                      #radius around sampling points within which to sample
                      #individuals (only needed is scheme is 'point' or
                      #'transect')
                  'when':                 None,
                      #can be an integer (in which case data will be collected every
                      #that many timesteps, plus at the end) or a list of specific
                      #timesteps; a value of 0 or None will default to a single
                      #data-collection step after the model has run
                  'include_land':         False,
                      #if True, will save the Landscape object each time other data is saved
                      #(probably only useful if land is changing in some way not manually coded by the user)
                  'include_fixed_sites':  False,
                      #if True, and if genetic data is to be formatted as VCFs,
                          #the VCFs will contain fixed sites, not just variants
                          #(defaults to False)
                  },
              'format': {
                  'gen_format':           ['vcf', 'fasta'],
                      #format to use for saving genetic data;
                          #currently valid values: 'vcf', 'fasta',
                          #or a list containing both, if both
                          #should be written
                  'geo_vect_format':      'csv',
                      #format to use for saving geographic points;
                          #currently valid values: 'csv', 'shapefile', 'geojson'
                  'geo_rast_format':      'geotiff',
                      #format to use for saving landscape rasters (which will
                          #only be saved if the 'include_land' parameter in the
                          #sampling subdict is True);
                          #currently valid values: 'geotiff', 'txt'
                  },
              }, #<END> 'data'
  
  
          'stats': {
              #dictionary defining which stats to be calculated, and parameters for
                  #their calculation (including frequency, in timesteps, of collection)
                  #valid stats include:
                      # 'Nt'  : population census size
                      # 'het' : heterozygosity
                      # 'maf' : minor allele frequency
                      # 'ld'  : linkage disequilibrium
                      # 'mean_fit' : mean fitness
              'Nt':       {'calc': True,
                           'freq': 2,
                          },
              'het':      {'calc': True,
                           'freq': 1,
                           'mean': False,
                          },
              'maf':      {'calc': True,
                           'freq': 5,
                          },
              'ld':       {'calc': True,
                           'freq': 10,
                          },
              'mean_fit': {'calc': True,
                           'freq': 3,
                          },
              }, # <END> 'stats'
  
  
          'seed': {
              #parameters to control whether and how to set the seed
              'set':          True,
                  #set the seed? (for reproducibility)
              'num':          94618
                  #value used to seed random number generators
              }, # <END> 'seed'
  
          } # <END> 'model'
  
      } # <END> params
  
 

-------------------------------------------------------------------------------
 
*****************************
Class and function docstrings
*****************************

==============================
:py:`gnx.make_parameters_file`
==============================

Create a new parameters file.

Write to disk a new, template parameters file. The file will contain the
numbers and types of sections indicated by the parameters fed to this
function. It can often be used 'out of the box' to make a new Model
object, but typically it will be edited by the user to stipulate
the scenario being simulated, then used to instantiate a Model.

----------
Parameters
----------
filepath : str, optional
    Where to write the resulting parameters file, in /path/to/filename.py
    format. Defaults to None. If None, a file named
    "GEONOMICS_params_<datetime>.py" will be written to the working
    directory.
scapes : {int, list of dicts}, optional
    Number (and optionally, types) of Layer-parameter sections to include
    in the parameters file that is generated. Defaults to 1. Valid values
    and their associated behaviors are:

    int:
        Add sections for the stipulated number of Layers, each with default
        settings:
        
          - parameters for creating Layers of type 'random' (i.e.
            Layers that will be generated by interpolation from
            randomly valued random points)
          - no LayerChanger parameters

    [dict, ..., dict]:
        Each dict in this list should be of the form:

        {'type':    'random', 'defined', 'file', or 'nlmpy',

        'change':   bool

        }

        This will add one section of Layer parameters, with the
        contents indicated, for each dict in this list.
populations : {int, list of dicts}, optional
    Number (and optionally, types) of Population-parameter sections to
    include in the parameters file that is generated. Defaults to 1. Valid
    values and their associated behaviors are:

    int:
        Add sections for the stipulated number of Populations, each with
        default settings:

          - parameters for movement without a MovementSurface
          - parameters for a GenomicArchitecture with 0 Traits (i.e. with
            only neutral loci)
          - no PopulationChanger parameters

    [dict, ..., dict]:
        Each dict should contain at least one argument from among the
        following:
        {'movement':                       bool,
        'movement_surface':                bool,
        'genomes':                         bool,
        'n_traits':                        int,
        'custom_genomic_architecture':     bool,
        'demographic_change':              int,
        'parameter_change':                bool
        }
        This will add one section of Population parameters, customized
        as indicated, for each dict in the list.

data : bool, optional
    Whether to include a Data-parameter section in the parameters file that
    is generated. Defaults to None. Valid values and their associated
    behaviors are:

    None, False:
        Will not add a section for parameterizing data to be collected.
        No DataCollector will be created for the Model object made from
        the resulting parameters file, and no data will be collected
        during the model runs.
    True:
        Will add a section that can be used to parameterize which
        data will be collected during the model runs, when, and what
        file formats will be used to write it to disk.
        (This which will be managed by the model's DataCollector
        object.)

stats : bool, optional
    Whether to include a Stats-parameter section in the parameters file that
    is generated. Defaults to None. Valid values and their associated
    behaviors are:

    None, False:
        Will not add a section for parameterizing the statistics to be
        calculated. No StatsCollector will be created for the Model
        object made from the resulting parameters file, and no
        statistics will be calculated during the model runs.
    True:
        Will add a section that can be used to parameterize which
        statistics will be calculated during the model runs, and when.
        (This will be managed by the model's StatsCollector object.)

-------
Returns
-------
out : None
    Returns no output. Resulting parameters file will be written to the
    location and filename indicated (or by default, will be written to a
    file named "GEONOMICS_params_<datetime>.py" in the working directory).

--------
See Also
--------
sim.params.make_parameters_file

-----
Notes
-----
All parameters of this function are optional. Calling the function without
providing any parameters will always produce the parameters file for the
default model scenario. This file can be instantiated as a Model object and
run without being edited. Those three steps (create default parameters file;
create model from that parameters file; run the model) serve as a base case
to test successful package installation, and are wrapped around by the
convenience function `gnx.run_default_model`.

--------
Examples
--------
In the simplest example, we can create a parameters file for the default
model. Then (assuming it is the only Geonomics parameters file in the
current working directory, so that it can be unambiguously identified) we
can call the gnx.make_model function to create a Model object from that
file, and then call the Model.run method to run the model (setting the
'verbose' parameter to True, so that we can observe model output).

>>> gnx.make_parameters_file()
>>> mod = gnx.make_model()
>>> mod.run(verbose = True)
TODO: PUT TYPICAL MODEL OUTPUT HERE, EVEN THOUGH IT'S ONLY PRINTED?

We can use some of the function's arguments, to create a parameters
file for a model with 3 Layers and 1 Population (all with the default
components for their sections of the parameters file) and with a section
for parameterizing data collection.

>>> gnx.make_parameters_file(scapes = 3, data = True)

As a more complex example that is likely to be similar to most use cases,
we can create a parameters file for a model scenario with:

    - 2 Layers (one being an nlmpy Layer that will not change over model
      time, the other being a raster read in from a GIS file and being
      subject to change over model time);
    - 2 Populations (the first having genomes, 2 Traits, and movement
      that is dictated by a MovementSurface; the second not having
      genomes but having a MovementSurface as well, and undergoing
      demographic change)
    - data-collection;
    - stats-collection;

We can save this to a file named "2-pop_2-trait_model.py" in our current
working directory.

>>> gnx.make_parameters_file(
>>>     #list of 2 dicts, each containing the values for each Layer's
>>>     #parameters section
>>>     scapes = [
>>>         {'type': 'nlmpy'},                              #scape 1 
>>>         {'type': 'gis',                                 #scape 2 
>>>          'change': True}
>>>         ],
>>>     #list of 2 dicts, each containing the values for each Population's
>>>     #parameters section
>>>     populations = [
>>>         {'genomes': True,                               #pop 1
>>>          'n_traits': 2,
>>>          'movement': True,
>>>          'movement_surface': True},
>>>         {'genomes': False,                              #pop 2
>>>          'movement': True,
>>>          'movement_surface': True,
>>>          'demographic_change': True}
>>>         ],
>>>     #arguments to the data and stats parameters
>>>     data = True, stats = True, 
>>>     #destination to which to write the resulting parameter file
>>>     filepath = '2-pop_2-trait_model.py')





==========================
:py:`read_parameters_file`
==========================

Create a new ParametersDict object.

Read the Geonomics parameters file saved at the location indicated by
'filepath', check its validity (i.e. that all the Layers and Populations
parameterized in that file have been given distinct names), then use the
file to instantiate a ParametersDict object.

----------
Parameters
----------
filepath : str
    String indicating the location of the Geonomics parameters file that
    should be made into a ParametersDict object.

-------
Returns
-------

An object of the ParametersDict class (a dict of nested dicts, all
of which have key-value pairs whose values can be accessed using typical
dict notation or using dot notation with the keys).

------
Raises
------
AssertionError
    If either the Layers or the Populations parameterized in the parameters
    file have not all been given distinct names

--------
See Also
--------
sim.params.read
sim.params.ParametersDict

--------
Examples
--------
Read a parameters file called "null_model.py" (located in the current
working directory).

>>> gnx.read_parameters_file('null_model.py')
<class 'sim.params.ParametersDict'>
Model name:                                     GEONOMICS_params_13-10-2018_15:54:03



================
:py:`make_model`
================

Create a new Model object.

Use either a ParametersDict object or the path to a valid Geonomics
parameters file (whichever is provided to the 'parameters' argument) to
create a new Model object.

----------
Parameters
----------
parameters : {ParametersDict, str}, optional
    The parameters to be used to make the Model object.
    If `parameters` is a ParametersDict object, the object will be used to
    make the Model.
    If `parameters` is a string, Geonomics will call
    `gnx.read_parameters_file` to make a ParametersDict object, then use
    that object to make the Model.
    If `parameters` is None, or is not provided, then Geonomics will
    attempt to find a single parameters file in the current working
    directory with the filename "GEONOMICS_params_<...>.py", will use that
    file to make a ParametersDict object, then will use that object to
    make the Model.

-------
Returns
-------
out : Model
    An object of the Model class

------
Raises
------
ValueError
    If the `parameters` argument was not provided and a single, valid
    Geonomics parameters file could not be identified in the current
    working directory
ValueError
    If the `parameters` arugment was given a string that does not point
    to a valid parameters file
ValueError
    If the ParametersDict provided to the `parameters` argument, or created
    from the parameters file being used, cannot be successfully made into a
    Model

--------
See Also
--------
gnx.read_parameters_file
sim.model.Model

--------
Examples
--------
Make a Model from a single, valid "GEONOMICS_params_<...>.py" file that can
be found in the current working directory (such as a file that would be
produced by calling gnx.make_parameters_file without any arguments).

>>> gnx.make_model()
<class 'sim.model.Model'>
Model name:                                     GEONOMICS_params_13-10-2018_15:54:03
Layers:                                         0: '0'
Populations:                                    0: '0'
Number of iterations:                           1
Number of burn-in timesteps (minimum):          30
Number of main timesteps:                       100
Geo-data collected:                             {}
Gen-data collected:                             {}
Stats collected:                                {}


Make a Model from a file called 'null_model.py', in the current working
directory.

>>> gnx.make_model('null_model.py')
<class 'sim.model.Model'>
Model name:                                     null_model
Layers:                                         0: 'tmp'
                                                1: 'ppt'
Populations:                                    0: 'C. fasciata'
Number of iterations:                           2500
Number of burn-in timesteps (mininum):          100
Number of main timesteps:                       1000
Geo-data collected:                             {csv, geotiff}
Gen-data collected:                             {vcf, fasta}
Stats collected:                                {maf, ld, mean_fit, het, Nt}

