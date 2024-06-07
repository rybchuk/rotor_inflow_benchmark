.. RAAW Inflow Benchmark documentation master file, created by
   sphinx-quickstart on Thu Jun  6 14:58:01 2024.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

RAAW Inflow Benchmark
=====================

.. figure:: ./images/illustration.png
  :align: center

*Figure: Illustration of the RAAW field experiment*

   
Data Provided
-------------

**Time series of lateral wind profiles**

Measurement identifier: ``NL`` (nacelle lidar)

Measurement source: nacelle-based scanning lidar 

We provide a time series of longitudinal wind velocity as a function of lateral coordinate, i.e. :math:`u(t,y)` where: 
   - :math:`u` is the longitudinal wind velocity
   - :math:`t \in [0,700]` s at :math:`dt=1` s :math:`\therefore n_t=701` 
   - :math:`y \in [-70,70]` m at :math:`dy=10` m :math:`\therefore n_y=15` 
   - :math:`x=-381` m 
   - :math:`z=120` m 

**Time series of vertical wind profiles**

Measurement identifier: ``GL`` (ground lidar)

Measurement source: ground-based vertically profiling lidar

We provide a time series of wind speed and direction as a function of height, i.e. :math:`U(t,z)` and :math:`\theta(t,z)` where:
   - :math:`U` is the wind speed considering longitudinal and lateral wind components
   - :math:`\theta` is the wind direction
   - :math:`t \in [0,700]` s at :math:`dt=1` s :math:`\therefore n_t=701` 
   - :math:`z \in [40, 290]` m irregularly spaced with :math:`n_z=9`
   - :math:`x` and :math:`y` are fixed but depend on the wind direction

**Time-averaged shear and veer**

Measurement identifier: ``SV`` (shear and veer)

Measurement source: ground-based vertically profiling lidar, scaled to match the lidar

We provide a single vertical profile of wind speed and direction, i.e. :math:`U(z)` and :math:`\theta(z)`. The shape of these profiles is obtained from the ``GL`` data. The magnitude is obtained from the ``GL`` data and then scaled by the same offset value at all heights to match the time-averaged values of ``NL`` at hub height. This scaling is necessary because the measurements ``NL`` and ``GL`` might refer to different areas of the site depending on wind direction: ``NL`` measurements will always be upwind because the scanning lidar moves with the nacelle; while measurements in ``GL`` are stationary. We scale ``NL`` to ``GL`` and not the other way around for two reasons: ``NL`` is the main constraint data for the inflow generation; and ``NL`` is most representative of the flow directly upwind of the wind turbine.

**Exponential coherence parameters**

Measurement identifier: ``COH``

Measurement source: anemometers on meteorological tower

We provide a recommendation for the two parameters of the exponential coherence model implemented in the IEC 61400-1, i.e. :math:`a_k` and :math:`b_k` from:

   :math:`\gamma = \exp \left[ -a_k \sqrt{\left( \frac{fr}{u_m} \right)^2 + \left( b_K r\right)^2} \right]` where

   - :math:`\gamma` is the magnitude-squared coherence
   - :math:`a_k` is the decrement parameter (assumed to be :math:`a_k=12` in IEC 61400-1; here it is fit to the measurements instead)
   - :math:`f` is the frequency
   - :math:`r` is the separation distance
   - :math:`u_m` is the wind speed averaged over time and over the two points separated by :math:`r`
   - :math:`b_k` is a scaling parameter (assumed to be :math:`b_k=0.12/L` in IEC 61400-1, where :math:`L` is the flow length scale; here it is fit to measurements instead)

The :math:`a_K` and :math:`b_K` provided here are are obtained by computing the magnitude-squared coherence between every possible combination of pairs of anemometers on the meteorological tower. There are 21 possible combinations ranging in vertical separation between 4 m and 127 m. The calculations are based on 1-Hz time series of wind speeds over a four-hour period centered on the shorter period of interest, which is on the order of 10 minutes. The Davenport coherence model is then fit to each of the magnitude-squared coherence functions calculated from the measurements, leading to 21 values for the coherence parameters. The :math:`a_K` and :math:`B_K` pair that yields the lowest error for the largest amount of separation distances is then selected and provided as a recommendation.

**Pointwise turbulence**

Measurement identifier: ``UVW``

Measurement source: ultrasonic anemometers on meteorological tower 

We provide a 24-hour time series centered on the shorter period of interest, which is on the order of 10 minutes. The data refer to the full wind velocity vector at three heights spanning the rotor from bottom to top, i.e. :math:`\vec{U}(t,z)` where:

   - :math:`\vec{U}` is the three-dimensional wind velocity vector :math:`(u,v,w)`
   - :math:`t \in [0,700]` s at :math:`dt=0.05` s :math:`\therefore n_t=14001` 
   - :math:`z` is 52.6 m, 110.5 m and 179.5 m :math:`\therefore n_z=3` 

The ``UVW`` measurement is intended to provide modelers with sufficient turbulence data to derive any parameters specific to their model which are not covered by the information provided in ``NL``, ``GL``, ``SV``, and ``COH``.

Data Requested
--------------

Modelers are requested to:

* Generate a time series of rotor inflow on a lateral-vertical plane, i.e. :math:`\vec{U}(t,y,z)` where:

   - :math:`\vec{U}` is the three-dimensional wind velocity vector :math:`(u,v,w)`
   - :math:`t \in [0,700]` s at :math:`dt=1` s :math:`\therefore n_t=701` 
   - :math:`y \in [-70, 70]` m at :math:`dy=5` m :math:`\therefore n_y=29`   
   - :math:`z \in [0, 200]` m at :math:`dz=5` m :math:`\therefore n_z=41`   

* Exactly match the ``NL`` data in time and space

* Exactly match the time-averaged ``SV`` data 

* Generate 6 different inflows for each benchmark study to address uncertainties related to model stochasticity

**Guidelines on deviating from data request:**

* The lateral and vertical extent of the generated inflow may be larger than the above-specified values, which represent minimum requirements, but it is important that there be a value at each of the grid points above. This ensures consistency across the different simulation strategies, between the simulations and the measurements, and ensures a hub point for the subsequent wind turbine simulation.

* If the modeling capability allows, the lateral and vertical resolution of the generated inflow should be :math:`dy=5` m = :math:`dz=5` m. We expect this to be the minimum required spatial resolution to produce robust predictions of structural load response for this wind turbine.

* For higher fidelity codes that cannot exactly match a desired shear and veer profile, use your judgment and provide the best that your simulation tool and you can achieve together.

**Guidelines on data formatting:**

Please submit your data in netcdf format with appropriately named and dimensioned coordinates, or in a numpy (``*npy`` or ``*npz``) format with accompanying information on the coordinates and units.

Coordinate System
-----------------

A right-hand coordinate system is defined for each benchmark study. The origin of the coordinate system is always the wind turbine tower base. The longitudinal (:math:`x`) coordinate is aligned along the time-averaged wind direction, pointing downwind. The lateral (:math:`y`) coordinate is parallel to the ground and points from right to left when looking downwind. The vertical (:math:`z`) coordinate is normal to the ground pointing upwards. 

.. toctree::
   :maxdepth: 2
   :caption: Contents:



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
