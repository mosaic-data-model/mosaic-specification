.. Written by Konrad Hinsen
.. License: CC-BY 3.0

.. index::
   single: H5MD

The H5MD Mosaic module
######################

`H5MD <http://nongnu.org/h5md/index.html>`_ is a modular file format
for trajectory files in molecular simulation, based on `HDF5
<http://www.hdfgroup.org/HDF5/>`_. It defines how the typically large
time-dependent datasets in trajectories are stored, but it does not
define a description for the simulation universe itself because such
descriptions are very domain specific. The combination of H5MD and
Mosaic provides a complete specification for self-contained trajectory
files, i.e. trajectory files that do not require additional information
for their interpretation.

The H5MD specification consists of a basic format specification plus
so-called *modules* for additional domain-specific specifications.
The combination of H5MD and Mosaic is thus defined in the following
by a specification of the "Mosaic module" for H5MD.


Objective
---------

The Mosaic module provides a way to store the definition of a
simulation universe and of subsets of the particles using Mosaic data
items. These data items assign a number to each site of each
atom. These numbers are the indices of the particle data in the
trajectory.

Module name and version
-----------------------
The name of this module is ``mosaic``. The module version is 0.1.0.

Mosaic root group
-----------------

H5MD files applying the Mosaic module contain an additional group
at the root level, i.e. next to the ``h5md`` group. This additional group
is called ``mosaic`` and contains all Mosaic data items related to
the trajectory.

The ``mosaic`` group contains one obligatory item, the simulation
universe, in a group called ``universe``. It may not contain more than
one universe data item. It can contain any number of other Mosaic data
items, whose names are subject to the same rules as the names of
particle groups in H5MD.

Each subgroup of the H5MD particles group must have a corresponding
Mosaic data item in the ``mosaic`` group that has the same name. This
data item can either be the simulation universe, or a selection data
item. Each H5MD particle corresponds to a Mosaic site. The site order
defined by the Mosaic universe or selection thus assigns a site
to each position in the particles group.

The ``box`` groups in the H5MD trajectory must be compatible with the
simulation universe: the dimension must be 3, the boundary must be
[none, none, none] for a universe with "infinite" cell shape and
[periodic, periodic, periodic] for a universe of any other 
currently defined cell shape, and the edges must also be compatible
with the universe's cell shape.

