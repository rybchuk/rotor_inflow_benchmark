.. _rotor_inflow_benchmark:

Rotor Inflow Benchmarks
=======================

.. toctree::
   :hidden:

   data_description
   unstable
   stable

Current status of this benchmark study: **CONCLUDED!** 

The results are presented in `"The IEA Wind Task 57 inflow reconstruction benchmark for a single turbine in simple terrain: real-world and synthetic case studies" (Rybchuk et al. 2026) <https://wes.copernicus.org/preprints/wes-2026-77/>`_.

Overview
^^^^^^^^

The benchmark studies the ability of modern turbine inflow models to reconstruct time-varying inflow data over the rotor disk when given hub-height constraint information. The primary motivation is to understand reconstruction skill in the real world, but additional analysis was conducted using high-resolution LES fields as the target data.

Validation against Field Measurements
*************************************

Models extrapolate above and below the constraint. The flow fields are validated against spinner lidar measurements and used to drive loads simulations. Simulated loads are validated against field measurements.

.. figure:: ./images/benchmark_illustration_field.png
  :align: center

Validation against Reference Simulations
****************************************

If you would like to see how your model compares to a reference large-eddy simulation dataset where the ground truth flow fields are known in full detail, we recommend performing this optional parallel validation study. To make this optional study easy to conduct, the simulation constraint data (``LES``) are formatted identically to the measurement constraint data (``NL``).

In this study, models extrapolate above and below the constraint. The flow fields are validated against reference simulation flow data and used to drive loads simulations. Simulated loads are validated against the loads produced when simulations are driven with the reference flow data. This parallel validation study **complements the validation against field measurements** by circumventing measurement uncertainties and instead validating against ground truth from a reference large-eddy simulation (LES).

.. figure:: ./images/benchmark_illustration_les.png
  :align: center

Why?
^^^^

There exist many different methods for simulating turbulent inflows to aeroelastic wind turbine simulations.
The various methods have never been compared side by side and validated against experimental data.
Understanding their strengths and weaknesses will inform us on potential model adoption or model development needs.

How?
^^^^
- Use multiple methods to generate constrained rotor inflows
- Compare them to one another
- **Validate simulated flow fields against measurements** (and reference simulation flow fields)
- Perform aeroelastic simulations that are identical except for the inflow used
- Quantify the effect of inflow accuracy on simulated structural loads
- **Validate simulated structural loads against measurements**

When?
^^^^^

The :ref:`Unstable Benchmarks <unstable>` follow the schedule below:

☑ October 2024: benchmark is released

☑ October - December 2024: modelers simulate inflow planes

☑ December 2024: modelers submit inflow planes to benchmark lead

☑ January 2025: benchmark lead performs inflow analyses

☑ January 2025: modelers perform any requested iterations

☑ February 2025: benchmark lead performs aeroelastic simulations

☑ March 2025: benchmark lead shares final results with modelers

The anticipated final product is a **journal publication** containing model development and adoption recommendations.