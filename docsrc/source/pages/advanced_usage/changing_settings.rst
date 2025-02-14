=================
Changing settings
=================

There are three ways to change the settings listed in :doc:`available_settings`:

* through code 
* through a custom configuration file
* through environment variables


Through code
------------

.. code-block:: python
   :caption: Configuration example

    # Change the config when creating the report
    profile = df.profile_report(title="Pandas Profiling Report", pool_size=1)

    # Change the config after
    profile.config.html.minify_html = False

    profile.to_file("output.html")


Some related settings are grouped together in configuration shorthands, making it easy to selectively enable or disable certain report sections or functionality: 

- ``samples``: control whether the dataset preview is shown. 
- ``correlation``: control whether correlation computations are executed.
- ``missing_diagrams``: control whether missing value analysis is executed. 
- ``duplicates``: control whether duplicate rows are previewed.
- ``interactions``: control whether interactions are computed. 

.. code-block:: python

    # Disable samples, correlations, missing diagrams and duplicates at once
    r = ProfileReport(
        samples=None,
        correlations=None,
        missing_diagrams=None,
        duplicates=None,
        interactions=None,
    )


Through a custom configuration file
-----------------------------------

To control ``pandas-profiling`` through a custom file, you can start with one of the sample configuration files below:

- `default configuration file <https://github.com/pandas-profiling/pandas-profiling/blob/master/src/pandas_profiling/config_default.yaml>`_ (default)
- `minimal configuration file <https://github.com/pandas-profiling/pandas-profiling/blob/master/src/pandas_profiling/config_minimal.yaml>`_ (minimal computation, optimized for performance)

Change the configuration to your liking and point towards that configuration file when computing the report:  

.. code-block:: python

  from pandas_profiling import ProfileReport

  profile = ProfileReport(df, config_file="your_config.yml")
  profile.to_file("report.html")


Through environment variables
-----------------------------

Any configuration setting can also be read from environment variables. For example:

.. code-block:: python

    from pandas_profiling import ProfileReport

    profile = ProfileReport(df, title="My Custom Pandas Profiling Report")

is equivalent to setting the title as an environment variable

.. code-block:: console

    export PROFILE_TITLE="My Custom Pandas Profiling Report"

and then running

.. code-block:: python

    from pandas_profiling import ProfileReport

    profile = ProfileReport(df)
