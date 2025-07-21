.. _stable:

Stable Benchmarks
-------------------

This page gives information specifc to the Stable Benchmarks. Information that applies to the rotor inflow benchmarks in general is found in :ref:`Data Description <data_description>`.

Modeling Instructions
^^^^^^^^^^^^^^^^^^^^^

Perform steps 1--3 twice: one for the validation against field measurements, and one for the validation against reference simulation data.

1. Download the constraint data for the three cases in :ref:`Data Access <data_access>` 

2. Generate :math:`\geq 6` inflows for each case

  - Details on the requested data (what to submit) apply to all benchmarks and are found in :ref:`Data Requested <data_requested>`

  - If applicable, use the coherence parameters provided in the table below. Note that they are the same for the two real-world cases.

3. Upload the :math:`\geq 18` inflows 

  - Email alex.rybchuk@nrel.gov to recieve a private Box link

.. figure:: ./images/benchmarks_requested.png
  :align: center

Shear and veer instructions
***************************

  - For the validation against measurements, use the power-law exponent and veer provided in *Table 1* below. The same values are also provided in each ``NL`` netCDF constraint file as an attribute.

  - For the validation against LES, use the power-law exponent and veer provided in *Table 2* below. The same values are also provided in each ``LES`` netCDF constraint file as an attribute.

  - Positive veer values convey that the flow rotates in a clockwise fashion when increasing in height. In other words, in the turbine coordinate system, positive veer means that the flow has a +y component at the bottom of the rotor and a -y component at the top of the rotor. 

The Primary Two Measurement Periods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Each benchmark case refers to an individual 704-second period measured in the field. For the stable benchmark, we will primarily consider two such periods observed within a window of less than two hours. The two periods are similar in terms of atmospheric conditions, but different enough in terms of dynamics. We consider two periods to add variation and build confidence in the results.

.. table:: *Table 1: Stable benchmark cases. With the exception of the rows in* **bold**, *the information below is* **not** *meant to be used in the simulations.*

    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Case identifier                       | 0530                                       | 0600                                       |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Date of period                        | May 9, 2023                                | May 9, 2023                                |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Start of period                       | 05:31:20 UTC                               | 06:03:11 UTC                               |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | End of period                         | 05:43:04 UTC                               | 06:14:55 UTC                               | 
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Hub height                            | 120 m                                      | 120 m                                      |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Freestream wind speed                 | 13.48 m s :math:`^{-1}`                    | 12.99 m s :math:`^{-1}`                    |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Advection time to turbine             | 29 s                                       | 31 s                                       |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Freestream wind direction             | 123.2 deg                                  | 144.4 deg                                  |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Freestream turbulence intensity       | 6.0%                                       | 4.9%                                       |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Rotor-layer shear                     | 4.79 m s :math:`^{-1}`                     | 6.94 m s :math:`^{-1}`                     |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | **Power law exponent**                | 0.313                                      | 0.486                                      |      
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | **Rotor-layer veer**                  | 8.5 deg                                    | 13.5 deg                                   |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Near-surface kinematic heat flux      | -0.054 K m s :math:`^{-1}`                 | -0.048 K m s :math:`^{-1}`                 |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Obukhov length                        | 61.0 m                                     | 42.3 m                                     |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | Hub-height density                    | 1.037 kg m :math:`^{-3}`                   | 1.039 kg m :math:`^{-3}`                   |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | **Coherence decrement parameter**     | 37.47                                      | 37.47                                      |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+
    | **Coherence scaling parameter**       | 1.4 :math:`\times 10^{-3}` m :math:`^{-1}` | 1.4 :math:`\times 10^{-3}` m :math:`^{-1}` |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+


.. raw:: html

Notes on the values provided above:

- The freestream wind speed is the hub-height, time-averaged, laterally-averaged longitudinal velocity as derived from the nacelle-mounted scanning lidar and corresponds to the single-value average of the ``NL`` dataset provided as a constraint. We use the ``NL`` here to be consistent with the data that is provided as a constraint. Other estimates of the mean freestream wind speed are available from the meteorological tower and ground-based profiling lidar, but those measure a parcel of air that is off to the side for these prevailing wind directions and therefore differ slightly from the lidar-measured values.
- The freestream wind direction and turbulence intensity come from hub-height, time-averaged measurements from the meteorological mast. The turbulence intensity calculation uses detrended wind speed values.
- The rotor layer shear is the difference between the time-averaged wind speeds at the rotor top (184 m) and rotor bottom (57 m) as measured by the meteorological mast. The power law exponent is fit to this data.
- The rotor layer veer is the difference between the time-averaged wind directions at the rotor top (184 m) and rotor bottom (57 m) as measured by the meteorological mast.
- The heat flux is obtained from the 2.5-meter temperature and vertical velocity ultrasonic measurements, considering a 10-minute window for the Reynolds averaging.
- The Obukhov length utilizes the value of heat flux provided, friction velocities estimated from the same instrument and using the same methodology as was employed for the heat flux calculation, and the time-averaged,  2-meter air temperature for the reference temperature.
- The hub-height density considers dry air and water vapor. It is obtained from vapor pressure and saturation vapor pressure estimates derived from the hub-height air pressure, temperature and relative humidity measurements at the meteorological tower.
- The coherence parameters (:math:`a_K` and :math:`b_K`) were selected according to the procedures described in :ref:`data_description`. The values that provided the lowest errors for most separation distances were those fit to the measured coherence at a separation of 41 m between the wind speed measurements at 110.5 m and 151.8 m.

The Optional Measurement Periods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The above measurement periods leverage heavily pre-processed nacelle-mounted lidar data in order to provide a uniform input that is compatible with many different inflow reconstruction methodologies. In the Stable Benchmark, we additionally provide the lidar PPI scans that underlie this pre-processed data, as well as upsampled scans following Beck and Kuhn (2019). Participants may submit additional reconstructions for 0530, 0600, and a new 0630 period using this information. These reconstructions will gage the impact of preprocessing the nacelle-lidar data and may pave the way for a 3rd Benchmark Phase using a largely expanded set of measurement windows.


The Three Simulated Periods
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The large-eddy simulations performed for this study approximately match the atmospheric conditions measured in the field. In these simulations, we have less control over the time-averaged vertical profiles. Therefore, they do not match exactly the field measurements. Instead, the shear and veer for each of the three simulated cases is provided below. As for the measurement data, the same values are also provided as attributes in the netCDF constraint files.

.. table:: *Table 2: Shear and veer in the large-eddy simulations of the stable benchmark cases.*

    +---------------------------------------+--------------------------------------------+--------------------------------------------+---------------------------------------------+
    | Case identifier                       | 1                                          | 2                                          | 3                                           |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+---------------------------------------------+
    | Hub height                            | 110 m                                      | 110 m                                      | 110 m                                       |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+---------------------------------------------+
    | Hub-height wind speed                 | 12.88 m s :math:`^{-1}`                    | 12.73 m s :math:`^{-1}`                    | 12.62 m s :math:`^{-1}`                     |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+---------------------------------------------+
    | **Power law exponent**                | 0.33                                       | 0.33                                       | 0.34                                        |      
    +---------------------------------------+--------------------------------------------+--------------------------------------------+---------------------------------------------+
    | **Rotor-layer veer**                  | 5.9 deg                                    | 5.7 deg                                    | 5.4 deg                                     |
    +---------------------------------------+--------------------------------------------+--------------------------------------------+---------------------------------------------+

.. _data_access:

Data Access
^^^^^^^^^^^

Find the constraints for each of the periods on `Zenodo <https://zenodo.org/records/15747684>`_.

- One set of files per case (0530 and 0600 for the measured flows; 1, 2 and 3 for the simulated flows)

