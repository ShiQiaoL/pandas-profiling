========================
Profiling large datasets
========================

By default, ``pandas-profiling`` comprehensively summarizes the input dataset in a way that gives the most insights for data analysis. For small datasets, these computations can be performed in *quasi* real-time. For larger datasets, deciding upfront which calculations to make might be required.
Whether a computation scales to a large datasets not only depends on the exact size of the detaset, but also on its complexity and on whether fast computations are available. If the computation time of the profiling becomes a bottleneck, ``pandas-profiling`` offers several alternatives to overcome it.

Minimal mode
------------

``pandas-profiling`` includes a minimal configuration file where the most expensive computations are turned off by default.
This is the recommended starting point for larger datasets.

.. code-block:: python

  profile = ProfileReport(large_dataset, minimal=True)
  profile.to_file("output.html")


*(minimal mode was introduced in version v2.4.0)*

This configuration file can be found here: `config_minimal.yaml <https://github.com/ydataai/pandas-profiling/blob/master/src/pandas_profiling/config_minimal.yaml>`_. More details on settings and configuration are available in :doc:`../advanced_usage/available_settings`.

Sample the dataset
------------------

An alternative way to handle really large datasets is to use a portion of it to generate the profiling report. Several users report this is a good way to scale back the computation time while maintaining representativity.

.. image:: ../../_static/twitter_wisdom.png
  :target: https://twitter.com/ogrisel/status/951425284963733505

.. code-block:: python

  # Sample 10.000 rows
  sample = large_dataset.sample(10000)

  profile = ProfileReport(sample, minimal=True)
  profile.to_file("output.html")

The reader of the report might want to know that the profile is generated using a sample from the data. This can be done by adding a description to the report (see :doc:`metadata` for details).

.. code-block:: python

  description = "Disclaimer: this profiling report was generated using a sample of 5% of the original dataset."
  sample = large_dataset.sample(frac=0.05)

  profile = sample.profile_report(description=description, minimal=True)
  profile.to_file("output.html")

Concurrency
-----------

``pandas-profiling`` is a project under active development. One of the highly desired features is the addition of a scalable backend such as `Modin <https://github.com/modin-project/modin>`_, `Spark <https://spark.apache.org/>`_ or `Dask <https://dask.org/>`_.


Keep an eye on the `GitHub <https://github.com/ydataai/pandas-profiling/issues>`_ page to follow the updates on the implementation of a concurrent and highly scalable backend. Specifically, development of a Spark backend is `currently underway <https://github.com/ydataai/pandas-profiling/projects/3>`_.