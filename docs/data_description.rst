.. _data_description:

Detailed Data Description
-------------------------

Data provided
^^^^^^^^^^^^^

**Constraint Inflow Turbulence**

- Identifier: ``NL`` (nacelle lidar)

- Instrument source: nacelle-based Halo Streamline XR scanning lidar 

- Upstream processing:
   - Line-of-sight velocities are converted to longitudinal velocities 
   - Nearest neighbor interpolation is performed to fill in data gaps in space and time
   - Plan-position-indicator scans are upsampled to 1 Hz frequency using Beck and Kühn (2019) algorithm
   - Nearest neighbor interpolation is performed again to go from a polar to a rectilinear grid
   - Flow field is subsampled at upstream distance of three rotor diameters, i.e. :math:`x=-3D` 
   - The flow field is denoised and expanded laterally using a machine learning algorithm trained on LES flow fields

- Description: Time series of longitudinal wind velocity as a function of lateral coordinate, i.e. :math:`u(t,y)` where: 
   - :math:`u` is the longitudinal wind velocity [m s :math:`^{-1}`]
   - :math:`t \in [0,703]` is the time coordinate [s]
      - Discretization: :math:`dt=1` s 
      - Size: :math:`n_t=704` 
   - :math:`x=-381` is the longitudinal (along-wind) coordinate [m] 
   - :math:`y \in [-150,150]` is the lateral (cross-wind) coordinate [m]
      - Discretization: :math:`dy=10` m
      - Size: :math:`n_y=31` 
   - :math:`z=120` is the vertical (ground-normal) coordinate [m]


**Optional Supporting Data: Shear & Veer**

- Identifier: ``GL`` (ground lidar)

- Instrument source: ground-based WindCube v3 vertically profiling lidar

- Upstream processing:
   - Time-averaged wind speed and direction. Please note that the ``GL`` is not co-located with the ``NL`` and the time-averaged wind speed values at hub height differ slightly between both instruments. The simulated inflow should match the wind speeds in ``NL`` and not in ``GL``.

- Description: Time-averaged wind speed and direction as a function of height, i.e. :math:`\overline{u}(z)` and :math:`\overline{\theta}(z)` where:
   - :math:`\overline{u}` is the time-averaged longitudinal wind velocity [m s :math:`^{-1}`]
   - :math:`\overline{\theta}` is the time-averaged wind direction
   - :math:`x=-381` is the longitudinal (along-wind) coordinate [m] 
   - :math:`y=0` is the lateral (cross-wind) coordinate [m]
   - :math:`z \in [40, 290]` is the vertical (ground-normal) coordinate [m]
      - Discretization: irregularly spaced
      - Size: :math:`n_z=9`

**Optional Supporting Data: Exponential Coherence Parameters**

- Identifier: ``COH``

- Instrument source: anemometers on meteorological tower

- Upstream processing: 
   - The magnitude-squared coherence is computed between every possible combination of pairs of anemometers on the meteorological tower. There are 21 possible combinations ranging in vertical separation between 4 m and 127 m. The calculations are based on 1-Hz time series of wind speeds over a four-hour period centered on the first period of interest. 
   - The Davenport coherence model is fit to each of the magnitude-squared coherence functions calculated from the measurements, leading to 21 values for the coherence parameters :math:`a_K` and :math:`b_K`.
   - Each :math:`a_K` and :math:`b_K` pair is then fit back to each distance for which we have the measured magnitude-squared coherence. Therefore, each of the 21 values is applied to 21 distances leading to 441 total fits. The sum of absolute deviations between the fit and the measured values is calculated for each of the 144 combinations.
   - The :math:`a_K` and :math:`b_K` pair that yields the lowest error for the largest amount of separation distances is then selected and provided as a recommendation. 

- Description: We provide a recommendation for the two parameters of the exponential coherence model implemented in the IEC 61400-1, i.e. :math:`a_k` and :math:`b_k` from:

   :math:`\gamma = \exp \left[ -a_k \sqrt{\left( \frac{fr}{u_m} \right)^2 + \left( b_K r\right)^2} \right]` where

   - :math:`\gamma` is the magnitude-squared coherence
   - :math:`a_k` is the decrement parameter (assumed to be :math:`a_k=12` in IEC 61400-1; here it is fit to the measurements instead)
   - :math:`f` is the frequency [Hz]
   - :math:`r` is the separation distance [m]
   - :math:`u_m` is the wind speed averaged over time and over the two points separated by :math:`r` [m s :math:`^{-1}`]
   - :math:`b_k` is a scaling parameter (assumed to be :math:`b_k=0.12/L` in IEC 61400-1, where :math:`L` is the flow length scale; here it is fit to measurements instead) [m :math:`^{-1}`]

**Optional Supporting Data: Extended Pointwise Turbulence**

- Identifier: ``UVW``

- Instrument source: ultrasonic anemometers on meteorological tower 

- Description: We provide a 24-hour time series centered on the shorter period of interest, which is on the order of 10 minutes. The data refer to the full wind velocity vector at three heights spanning the rotor from bottom to top, i.e. :math:`\vec{U}(t,z)` where:

   - :math:`\vec{U}` is the three-dimensional wind velocity vector :math:`(u,v,w)` [m s :math:`^{-1}`]
   - :math:`t \in [0,703]` is the time coordinate [s]
      - Discretization: :math:`dt=0.05` s 
      - Size: :math:`n_t=14080` 
   - :math:`z` is the vertical (ground-normal) coordinate [m] 
      - Provided at 52.6 m (:math:`\sim z_{hub}-0.5D`), 110.5 m (:math:`\sim z_{hub}`), 179.5 m (:math:`\sim z_{hub}+0.5D`)
      - Size: :math:`n_z=3` 

The ``UVW`` measurement is intended to provide modelers with sufficient turbulence data to derive any parameters specific to their model which are not covered by the information provided in ``NL``, ``GL``, and ``COH``.

Data Requested
^^^^^^^^^^^^^^

Modelers are requested to:

- Generate a time series of turbulent rotor inflow on a lateral-vertical plane, i.e. :math:`\vec{U}(t,y,z)` where:

   - :math:`\vec{U}` is the three-dimensional wind velocity vector :math:`(u,v,w)` [m s :math:`^{-1}`]
   - :math:`t` is the time coordinate [s]
      - Minimum acceptable simulation length: :math:`L_t=700` s 
      - Minimum acceptable time discretization: :math:`dt=1` s 
   - :math:`y` is the lateral (cross-wind) coordinate [m]
      - Minimum acceptable lateral extent: :math:`L_y=140` m centered at :math:`y=0`
      - Minimum acceptable lateral discretization:: :math:`dy=10` m
      - Number of points :math:`N_y` must be odd, ensuring a grid point at :math:`y=0`
   - :math:`z`  is the vertical (ground-normal) coordinate [m]
      - Minimum acceptable vertical extent: :math:`z \in \left[0, 190\right]` m
      - Minimum acceptable vertical discretization: :math:`dz=10` m
      - There must be a grid point at :math:`z=z_{hub}=120` m

- Exactly match the time-lateral longitudinal wind constraints (``NL`` data)

- Exactly match the time-averaged wind direction and speed profile constraints (``GL`` data)

- Generate 6 different inflows for each time period (to address uncertainties related to model stochasticity)

**Guidelines on deviating from data request:**

* The lateral and vertical extent of the generated inflow may be larger than the above-specified values, which represent minimum requirements, but it is important that there be a value at :math:`y=0` and :math:`z=120` m. This ensures consistency across the different simulation strategies, between the simulations and the measurements, and ensures a hub point for the subsequent wind turbine simulations.

* If the modeling capability allows, the lateral and vertical resolution of the generated inflow should be :math:`dy=dz=5` m. We expect this to be the minimum required spatial resolution to produce robust predictions of structural load response for this wind turbine.

* For higher fidelity codes that cannot exactly match a desired shear and veer profile, use your judgment and provide the best that your simulation tool and you can achieve together.

**Guidelines on data formatting:**

Please submit your data in netcdf format with appropriately named and dimensioned coordinates, or in a numpy (``*npy`` or ``*npz``) format with accompanying information on the coordinates and units.

Coordinate System
^^^^^^^^^^^^^^^^^

A right-hand coordinate system is defined: 

- Origin: wind turbine tower base
- Longitudinal (:math:`x`) coordinate: parallel to the ground and to the time-averaged nacelle heading for each 704-second period
- Lateral (:math:`y`) coordinate: parallel to the ground and points from right to left when looking downwind at the rotor
- Vertical (:math:`z`) coordinate is normal to the ground pointing upwards

.. figure:: ./images/coordinate_system.png
  :align: center

*Figure: Schematic of coordinate system, which is defined according to the time-averaged yaw heading for each 704-second period.*

Data Access
^^^^^^^^^^^

Link to data download forthcoming.