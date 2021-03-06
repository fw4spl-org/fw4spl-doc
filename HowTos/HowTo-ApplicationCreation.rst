.. _xmlApplication:

****************************************
How to create a XML based application ?
****************************************

In the Tutorials, we explain how to create simple applications.
A XML based application, is defined by an "application bundle" (like a bundle but with some differences that we will 
describe further).

Application bundle
-------------------

This "application bundle" contains a base configuration to run when the application is launched and lists the required 
bundles for this configuration. 

Like a bundle, the application folder needs the CMake files and a plugin.xml file. The first difference, is that the 
*Properties.cmake* ``TYPE`` is ``APP`` instead of ``BUNDLE``.
The second difference is the line:

.. code-block:: cmake

    bundleParam(appXml PARAM_LIST config PARAM_VALUES tutoDataServiceBasicConfig)
    
It defines the main configuration to be launched by the application (see :ref:`Properties.cmake`).

The main configuration should be written in the ``plugin.xml`` file in a ``<extension implements="::fwServices::registry::AppConfig">``
tag (see :ref:`tuto01`).

.. _profile.xml: 

profile.xml
------------

To launch an application, we use:

.. code::

    bin/fwlauncher share/<myApplication>_<version>/profile.xml

This ``profile.xml`` file, used as an input of the fwlauncher command, holds a list of all bundles
necessary to run an application. We describe the content of this file here for reference, but hopefully you do **not** have to write it yourself. 
The Properties.cmake of an application generates automatically a ``profile.xml``.
    

Here is for example the ``profile.xml`` generated for :ref:`tuto01`.

.. code-block:: xml

    <!-- WARNING, this file is GENERATED by FW4SPL CMake-based build system from CMake/build/profile.xml.in -->
    <!-- DO NOT EDIT MANUALLY !!! -->

    <profile name="Tuto01Basic" version="0.1" check-single-instance="false">

        <activate id="Tuto01Basic" version="0.1" />
        <activate id="appXml" version="0.2" >
            <param id="config" value="tutoBasicConfig" />
        </activate>
        <activate id="dataReg" version="0.1" />
        <activate id="gui" version="0.1" />
        <activate id="guiQt" version="0.1" />
        <activate id="servicesReg" version="0.1" />

        <start id="appXml" />
        <start id="guiQt" />

    </profile>

activate:
    List of bundles used in this application. We see the parameter given to *appXML* bundle that we wrote in the *Properties.cmake*.
    
start:
    List of bundles to start when the application is launched. Basically, there are a few bundles to start at the beginning:
    
    - *appXML*: to launch the configuration
    - *guiQt*: to launch the qt event loop for applications with a GUI
    - *memory*: to manage image and mesh buffers

    The other bundles will be started according to the XML <requirement> tags of the bundles, or when a service is used in 
    an XML configuration and its bundle is not started. That way we only have the minimum number of shared libraries loaded.
