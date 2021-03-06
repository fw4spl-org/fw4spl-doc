*************************
How to fix my bug?
*************************

.. _dataNotFound:

My data is not found
-----------------------
#. Is the data properly written in your XML configuration? Don't forget the ``::``.
#. Is the xxDataReg bundle in your application/activity requirement? Where xxDataReg is the bundle that register the library containing your data (dataReg, arDataReg, ...).

.. _serviceNotFound:

My service is not found
-------------------------
#. Is the service properly written in your XML configuration? Dont't forget the ``::``?
#. Did you call ``cmake`` after adding the new files? You need to call ``cmake .`` in your build repository to ensure that the files are built.
#. Is the bundle of your service added in your application/activity Properties.cmake?
#. Is the bundle properly registered? See :ref:`bundleCreation`
#. Is the plugin.xml properly generated?

For example with a bundle named ``myBundle`` with version ``0.2`` containing the service ``SMyService``:
It must be written in ``<build_dir>/share/myBundle_0.2/plugin.xml``

.. code-block:: xml

    <!-- WARNING, this file is GENERATED by FW4SPL CMake-based build system from CMake/build/plugin.xml.in -->
    <!-- DO NOT EDIT MANUALLY !!! -->

    <plugin id="myBundle" class="::myBundle::Plugin" version="0.2" >
        <library name="myBundle" />
        <requirement id="dataReg"/>
        <requirement id="servicesReg"/>

        <extension implements="::fwServices::registry::ServiceFactory">
             <type>::fwServices::IOperator</type>
             <service>::myBundle::SMyService</service>
             <object key="myInput">::fwData::Image</object>
             <desc>My service newly added.</desc>
        </extension>
    </plugin>

The requirement may be different according to your bundle requirement (in the Properties.cmake).
The ``<library>`` tag is important, it defines that the bundle is associated to a shared library and must be loaded when the bundle is used.

There should be an ``extension`` tag for each service in your bundle. All the objects of a service should be declared in the ``object`` tag (if they have any).

The *Plugin.cpp* contains the following line with the proper bundle name (Don't forget the ``::``)

.. code-block:: cpp

    static ::fwRuntime::utils::GenericExecutableFactoryRegistrar<Plugin> registrar("::myBundle::Plugin");

The register service macro should be present. If you don't have the macro ``fwServicesRegisterMacro(...)`` in
SMyService.cpp, it should be generated in ``<build_dir>/myBundle/registerServices.cpp``

.. code-block:: cpp

    /* WARNING, this file is GENERATED by FW4SPL CMake-based build system from CMake/build/registerServices.cpp.in
     * DO NOT EDIT MANUALLY !!!
     */

    #include <fwData/Image.hpp>
    #include "myBundle/myService.hpp"
    #include <fwServices/macros.hpp>

    fwServicesRegisterMacro( ::fwServices::IOperator, ::myBundle::SMyService )
    fwServicesRegisterObjectMacro( ::myBundle::SMyService, ::fwData::Image )

All the services of the bundle and their objects should be present in this file.

.. note::

    To generate the ``plugin.xml`` and the ``registerServices.cpp`` a ``CMake`` script will parse the services macro and doxygen.
    Make sure that they are properly written (see :ref:`serviceCreation`).

.. _activityNotFound:

My activity is not found
-------------------------

#. Is the bundle containing you activity in your application *Properties.cmake*?
#. Is the ``activities`` bundle in the requirement of your application *plugin.xml*?
