*******************************************************************
How to create a bundle, a lib, an executable or an application ?
******************************************************************

In fw4spl, the bundles, libraries, applications and executables are folder containing:
- [required] two files to generate the *CMake* target: CMakelists.txt and Propertires.cmake (see :ref:`HowCMake`).
- [optional] *include* and *src* folder to contain the headers and source files.
- [optional] *rc* folder to contain resources and XML configutation files
- [optional] *test* folder to contain the unit test



How to create a bundle ?
==========================

In fw4spl, there is two type of bundle:
- the bundle containing only XML configurations
- the bundle containing services or other cpp code

It is possible to contain at the same time configuration and services (or C++ code), but it is better to separate the two.


Configuration bundles
----------------------

These bundles does not contain C++ code, they only contain XML files and the required *CMake* files.

In the bundle folder, there is only the *CMake* files and the *rc* folder.

CMake Files
~~~~~~~~~~~~

The CMakeLists.txt contain only ``fwLoadProperties()`` to load the Properties.cmake

The Properties.cmake defines the bundles needed to launch the configuration (ie. the bundle of all the services present 
in the configurations).

Example:

set( NAME dataManagerConfig )
set( VERSION 0.1 )
set( TYPE BUNDLE )
set( DEPENDENCIES  ) # no dependency

set( REQUIREMENTS # required bundle
    gui
    guiQt
    uiMedDataQt
    uiReconstructionQt
    ctrlSelection
    media
) 

Configurations
~~~~~~~~~~~~~~~

A bundle could contain several configurations, they are in the ``plugin.xml`` file in the *rc* folder.

.. note::

    To separate the configuration in several files, you can use ``<xi:include href="..." />``

.. code-block:: xml

    <plugin id="dataManagerConfig" version="@PROJECT_VERSION@" >

        <requirement id="dataReg" />
        <requirement id="servicesReg" />

        <!-- ... -->

    </plugin>

Service bundles
----------------