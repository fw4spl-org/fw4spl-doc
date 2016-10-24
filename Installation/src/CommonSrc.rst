
.. warning::
    Do NOT use ninja to compile the dependencies, it causes conflict with qt compilation.

If you get compilation errors at this step, please ensure you installed all the requirements, especially those for `Qt <http://wiki.qt.io/Building_Qt_5_from_Git>`_.

Source
~~~~~~~~

- Clone fw4spl repository into your source directory:

.. code:: bash

    $ cd Dev/Src
    $ git clone https://github.com/fw4spl-org/fw4spl.git fw4spl

- Go into fw4spl folder and update to the latest stable version:

.. code:: bash

    $ cd fw4spl
    $ git checkout fw4spl_0.11.0

- Go into your Build directory (Debug or Release) : here an example if you want to compile in debug:

.. code:: bash

    $ cd Dev/Build/Debug

- Now you have to configure the project. You can use one of the three tools explained above. 

Also, for FW4SPL, we recommend to use the `Ninja <https://martine.github.io/ninja/>`_ generator. It builds faster, and it is much better for everyday use because it is fast as hell to check the files you need to compile. In other words, with Ninja the compilation starts instantly whereas Make spends a dozen of seconds to check what should be compiled before actually compiling something. So if you plan to develop with FW4SPL, go with Ninja. If you only want to give a single try, you can live with the standard "Unix Makefiles" generator.

To use make, here with ``ccmake`` :

.. code:: bash

    $ ccmake ../../Src/fw4spl

To use ninja :

.. code:: bash

    $ ccmake -G Ninja ../../Src/fw4spl

- Change the following cmake arguments
    - ``CMAKE_INSTALL_PREFIX``: set the install location (~/Dev/Install/Debug or Release)
    - ``CMAKE_BUILD_TYPE``: set to Debug or Release
    - ``EXTERNAL_LIBRARIES``: set the install path of the third party libraries you compiled before.(ex : ~/Dev/Deps/Install/Debug)
    - ``PROJECT_TO_BUILD``: set the list of the projects you want to build (ex: VRRender, Tuto01Basic ...), each project should be separated by ";"
    - ``PROJECT_TO_INSTALL``: set the name of the application to install

.. note::
    - If ``PROJECT_TO_BUILD`` is empty, all application will be compiled
    - If ``PROJECT_TO_INSTALL`` is empty, no application will be installed

.. image:: ../media/osx_cmake_fw4spl.png

Press *"c"* to configure and then *"g"* to generate the makefiles.

.. note::

    To generate the projects in release mode, change CMake argument ``CMAKE_BUILD_TYPE`` to ``Release`` **both** for fw4spl and fw4spl-deps
    
Then, according to the generator you chose, build FW4SPL with make :

.. code:: bash

    # Adjust the number of cores depending of the CPU cores and the RAM available on your computer
    $ make -j4 
    
Or with ninja:

.. code:: bash

    $ ninja
    
If you didn't specify anything in ``PROJECT_TO_BUILD`` you may also build specific targets, for instance:

.. code:: bash

    $ ninja Tuto01Basic VRRender

Launch an application
-------------------------

After a successful compilation the application can be launched with the *fwlauncher* program from FW4SPL.
The profile.xml of the application in the build folder has to be passed as argument to the *fwlauncher* call in the console.

.. code:: bash

    $ bin/fwlauncher Bundles/MyApplication_Version/profile.xml

Example:

.. code:: bash

    $ cd /Dev/Build/Debug
    $ bin/fwlauncher Bundles/VRRender_0-9/profile.xml

Extensions
----------

**FW4SPL** has two main extension repositories:

- `fw4spl-ar <https://github.com/fw4spl-org/fw4spl-ar.git>`_: extension of fw4spl repository, contains functionalities for augmented reality (video tracking for instance).

.. code:: bash

    $ cd Dev/Src
    $ git clone https://github.com/fw4spl-org/fw4spl-ar.git fw4spl-ar
    $ cd fw4spl-ar
    $ git checkout fw4spl_0.11.0

- `fw4spl-ogre <https://github.com/fw4spl-org/fw4spl-ogre.git>`_: another extension of fw4spl, contains a 3D backend using `Ogre3D <http://www.ogre3d.org/>`_.

    $ cd Dev/Src
    $ git clone https://github.com/fw4spl-org/fw4spl-ogre.git fw4spl-ogre
    $ cd fw4spl-ogre
    $ git checkout fw4spl_0.11.0
    

Then you have to reconfigure your CMake project:

.. code:: bash

    $ cd ../../Build/Debug
    $ ccmake .

Modify `̀ ADDITIONAL_PROJECTS`̀ : set the source location of fw4spl-ar and fw4spl-ogre separated by a semi-colon.

.. code:: bash

    ~/Dev/Src/fw4spl-ar/;~/Dev/Src/fw4spl-ogre/
