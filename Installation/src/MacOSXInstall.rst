Installation for MacOSX
=======================

Prerequisites for MacOSX users
--------------------------------

If not already installed:

#. Install `Xcode <https://itunes.apple.com/fr/app/xcode/id497799835?mt=12>`_
#. Install `git <https://git-scm.com/downloads>`_
#. Install `Python 2.7 <https://www.python.org/downloads/>`_
#. Install `CMake <http://www.cmake.org/download/>`_
#. Install `Ninja <https://github.com/martine/ninja/releases>`_ : to use instead of **make**.

For an easy install, you can use the `Hombrew project <http://brew.sh/>`_  to install missing packages.
        
.. code:: bash

    $ brew install git
    $ brew install python
    $ brew install cmake
    $ brew install ninja


FW4SPL installation
-------------------------

FW4SPL works with data separation for source, build and install data.
To prepare the development environment:

- Create a development folder (Dev)

.. code:: bash

    $ mkdir Dev

- Create the build, source and install folder 
    - Dev/Build
    - Dev/Src
    - Dev/Install

.. code:: bash

    $ mkdir Dev/Build Dev/Src Dev/Install

To prepare the third party environment:

- Create a third party folder (Deps)

.. code:: bash

    $ mkdir Dev/Deps

- Create the build, source and install folder
    - Dev/Deps/Build
    - Dev/Deps/Src
    - Dev/Deps/Install

.. code:: bash

    $ mkdir Dev/Deps/Build Dev/Deps/Src Dev/Deps/Install
    

.. _build_tools:

Build tools
~~~~~~~~~~~~

FW4SPL is a CMake project. That means, for each build target there is a CMakeLists that provides build parameters.
To configure you project you can use the ``cmake`` command from the build folder with the sources as arguments:

.. code:: bash

    $ ccmake /PATH/TO/fw4spl

if you want to use **Ninja** as build to tools, use the option ``-G Ninja``, as following:

.. code:: bash

    $ ccmake -G Ninja /PATH/TO/fw4spl

It is the same process for BinPkgs and FW4SPL sources. It is recommended to use make to compile the deps.

Dependencies
~~~~~~~~~~~~~~~~~

For the third party libraries the following repository have to be `cloned <http://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository#Cloning-an-Existing-Repository>`_ in the (Deps) source folder:

- `fw4spl-deps <https://github.com/fw4spl-org/fw4spl-deps.git>`_: contains the scripts to compile the external libraries used by fw4spl (Boost, VTK, ITK, Qt, …​)

.. code:: bash

    $ cd ~/Dev/Deps/Src
    $ git clone https://github.com/fw4spl-org/fw4spl-deps.git fw4spl-deps
    $ cd fw4spl-deps
    $ git checkout fw4spl_0.11.0

To build the dependencies, you must configure the project with cmake into the Build folder

.. code:: bash

    $ cd ~/Dev/Deps/Build
    $ cmake ../Src/fw4spl-deps -DCMAKE_INSTALL_PREFIX=../Install -DCMAKE_BUILD_TYPE=Debug

Or open cmake gui editor, see :ref:`build_tools` instructions.
 
.. code:: bash

    $ ccmake ../Src/fw4spl-deps

Some CMake variables have to be change:

- *CMAKE_INSTALL_PREFIX*: set the install location.
- *CMAKE_BUILD_TYPE*: set the build type 'Debug' or 'Release'

.. image:: ../media/osx_cmake_binpkgs.png

Press configure (*[c]*) and generate (*[g]*) makefiles. 

Then, compile the FW4SPL dependencies with make

.. code:: bash

    $ make all
    $ make install_tool
    
.. warning::
    Do NOT use ninja to compile the dependencies, it cause conflict with qt compilation.


Source
~~~~~~~~~~~~~~~~~

For the FW4SPL source code the following repository have to be `cloned <http://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository#Cloning-an-Existing-Repository>`_ in the (Dev) source folder:

- `fw4spl <https://github.com/fw4spl-org/fw4spl.git>`_: main repository, contains the core libraries and bundles.

.. code:: bash

    $ cd ~/Dev/Src
    $ git clone https://github.com/fw4spl-org/fw4spl.git fw4spl
    $ cd fw4spl
    $ git checkout fw4spl_0.11.0

.. note::
    For the source compilation we use ``ninja`` instead of ``make``. But if you prefer to use make, replace all the ``ninja`` command with ``make`` and remove ``-G Ninja`` in the cmake command.

To build fw4spl, you must configure the project with cmake into the Build folder

.. code:: bash

    $ cd ~/Dev/Build
    $ cmake ../Src/fw4spl -DCMAKE_INSTALL_PREFIX=../Install -DCMAKE_BUILD_TYPE=Debug -DEXTERNAL_LIBRARIES=../Deps/Install -G Ninja

Or open cmake gui editor, see :ref:`build_tools` instructions.

.. code:: bash

    $ ccmake ../Src/fw4spl -G Ninja

Some CMake variables have to be change:

- *CMAKE_INSTALL_PREFIX*: set the install location.

- *EXTERNAL_LIBRARIES*: set the install path of the third part libraries.
- *CMAKE_BUILD_TYPE*: set to Debug or Release

You can re-edit cmake configuration :

.. code:: bash

    $ ccmake .

- *PROJECT_TO_BUILD* set the name of the application to build (See Dev\Src\Apps)
- *PROJECT_TO_INSTALL* set the name of the application to install

.. note:: 
    - If PROJECT_TO_BUILD is empty, all application will be compiled
    - If PROJECT_TO_INSTALL is empty, no aplication will be installed

.. image:: ../media/osx_cmake_fw4spl.png

Press configure (**[c]**) and generate (**[g]**) makefiles. 

Then, build dependencies with ninja.

.. code:: bash

    $ ninja all
    

Launch an application
-------------------------

To build a specific or several applications the CMake argument ``PROJECTS_TO_BUILD`` can be set.
Use ``;`` so separate each application name.

After an successful compilation the application can be launched with the *fwlauncher* program from a terminal.
Therefore the profile.xml of the application in the build folder has to be passed as argument to the *fwlauncher*:

.. code:: bash

    $ bin/fwlauncher Bundles/MyApplication_Version/profile.xml
    
Example: 

.. code:: bash

    $ cd ~/Dev/Build
    $ bin/fwlauncher Bundles/VRRender_0-9/profile.xml

.. note::

    To generate the projects in release, change CMake argument ``CMAKE_BUILD_TYPE`` to ``Release`` for fw4spl and fw4spl-deps
    
.. warning::
    Do NOT compile debug and release in the same Build and Install folder
    
    
Extensions
----------

**fw4spl** has two main extension repositories:

- `fw4spl-ar <https://github.com/fw4spl-org/fw4spl-ar.git>`_: extension of fw4spl repository, contains functionalities for augmented reality (video tracking for instance).

.. code:: bash

    $ cd Dev/Src
    $ git clone https://github.com/fw4spl-org/fw4spl-ar.git fw4spl-ar
    $ cd fw4spl-ar
    $ git checkout fw4spl_0.11.0

- `fw4spl-ogre <https://github.com/fw4spl-org/fw4spl-ogre.git>`_: another extension of fw4spl, contains a 3D backend using `Ogre3D <http://www.ogre3d.org/>`_.

.. code:: bash

    $ cd ../../Build
    $ ccmake .

Modify *ADDITIONAL_PROJECTS*: set the source location of fw4spl-ar and fw4spl-ogre separated by ';'

.. code:: bash

    ~/Dev/Src/fw4spl-ar/;~/Dev/Src/fw4spl-ogre/

Recommended softwares
-------------------------

The following programs may be helpful for your developments:

- IDE:
    - `Qt creator <http://www.qt.io/download-open-source/#section-6>`_
    - `Eclipse CDT <https://eclipse.org/cdt/>`_.

- Versioning tools:
    - `SourceTree <http://www.sourcetreeapp.com/>`_
