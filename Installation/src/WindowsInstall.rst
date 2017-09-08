Installation for Windows
=========================

Prerequisites for Windows users
--------------------------------

If not already installed:

1. Install `git <https://git-scm.com/>`_

2. Optionally you can install `SourceTree <https://www.sourcetreeapp.com/>`_ to manage your repositories

3. Install `Visual Studio 2015 Community <https://www.microsoft.com/en-us/download/details.aspx?id=48146>`_

4. Install `Python 2.7 <https://www.python.org/downloads/>`_

5. Install `CMake <http://www.cmake.org/download/>`_

6. Install `jom <http://wiki.qt.io/Jom>`_

7. Install `ninja <https://github.com/ninja-build/ninja/releases>`_

Qt is an external library used in FW4SPL. For the successful compilation of Qt for FW4SPL, please see the following requirements:

- http://wiki.qt.io/Building_Qt_5_from_Git



FW4SPL installation
-------------------------

Good practice in FW4SPL recommend to separate source files, build and install folders. 
So to prepare the development environment:

* Create a development folder (Dev)

* Create a build folder (Dev\\Build)

    * Add a sub folder for Debug and Release.
    
* Create a source folder (Dev\\Src)

* Create a install folder (Dev\\Install)

    * Add a sub folder for Debug and Release.

To prepare the third party environment:

* Create a third party folder (BinPkgs)

* Create a build folder (BinPkgs\\Build)

    * Add a sub folder for Debug and Release.
    
* Create a source folder (BinPkgs\\Src)

* Create an install folder (BinPkgs\\Install) 

    * Add a sub folder for Debug and Release.                    

|directories|

Set the environment for a x64 version.
To compile BinPkgs and sources, you must use the 'VS2015 x64 Native Tools Command Prompt' 

.. |directories| image:: ../media/Directories.png

Dependencies
~~~~~~~~~~~~~~~~~


* `Clone <http://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository#Cloning-an-Existing-Repository>`_ the following repository in the (BinPkgs/Src) source folder:

    * `fw4spl-deps <https://github.com/fw4spl-org/fw4spl-deps.git>`_

.. code:: bash

    > cd Deps/BinPkgs/Src
    > git clone https://github.com/fw4spl-org/fw4spl-deps.git

.. note:: *Optionnal*: 
    You can also clone this extension repository: `fw4spl-ext-deps <https://github.com/fw4spl-org/fw4spl-ext-deps.git>`_

    You'll need it if you want to add extension to fw4spl (like fw4spw-ar).

* Check if all the cloned repositories are on the same `branch <https://git-scm.com/docs/git-branch>`_.

* Update the cloned repositories to the *11.0.4* `tag <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

.. warning:: Be sure to be in the 'VS2015 x64 Native Tools Command Prompt'

.. note:: 
    Make sure that CMake executable (cmake.exe and cmake-gui.exe)location is present in your PATH environment variable. 
    
    - SET PATH=%PATH%;D:\Tools\CMake\bin

.. note:: 
    Make sure that JOM executable (jom.exe) location is present in your PATH environment variable.
    
    - SET PATH=%PATH%;D:\Tools\jom

* Call the cmake-gui by executing command : cmake-gui

.. code:: bash

    > cmake-gui

* Set the wanted Build directory (e.g. Dev\\BinPkgs\\Build\\Debug or Release)

* Set the wanted Source directory (e.g. Dev\\BinPkgs\\Src\\fw4spl-deps)

* During Configure, choose the generator 'NMake Makefiles JOM'. 

* Set the following arguments:

    * ``CMAKE_INSTALL_PREFIX``: set the install location (e.g. Dev\\BinPkgs\\Install\\Debug).
    * ``CMAKE_BUILD_TYPE``: set to Debug or Release.
    * ``ADDITIONAL_DEPS``: you can leave it empty, it is only needed if you have an extra source location like fw4spl-ext-deps or a custom repository.

.. warning::

    ``ENABLED_PCL_DEPS``, ``ENABLE_LIBSGM`` and ``ENABLE_OPENCV_CUDA`` need `Cuda <https://developer.nvidia.com/cuda-downloads>`_ library before. If you install Cuda during the process, you must re-open the ‘VS2015 x64 Native Tools Command Prompt’ to update your PATH.

* Set the following options (some of the options will be needed for the optional source):

    * ``ENABLED_INFINITAM``: set to ON to build infinitam.
    * ``ENABLED_BUILD_ORB_SLAM2``: set to ON to build ORB Slam 2
    * ``ENABLED_OGRE_DEPS``: set to ON to build ogrecode.
    * ``ENABLED_PCL_DEPS``: set to ON to build PCLcode.
    * ``ENABLED_REALSENSE``: set to ON to build librealsensecode.
    * ``ENABLED_SOFA_DEPS``: set to ON to build sofacode.
    * ``ENABLE_EXPERIMENTAL_DEPS``: set to ON to build experimentals libraries.
    * ``ENABLE_LIBSGM``: set to ON to build libSGM dependenciescode.
    * ``ENABLE_ODIL``: set to ON to build Odil dependenciescode.
    * ``ENABLE_OPENCV_CONTRIB``: set to ON to build OpenCV contrib extra modules.
    * ``ENABLE_OPENCV_CUDA``: set to ON to build OpenCV with CUDA supportcode.
    * ``ENABLE_OPEN_MP``: set to ON use allow the use of OpenMP” OFF).
    * ``ENABLE_PCL_CUDA``: set to ON to build PCL with CUDA support.

* Generate the code. 

* Compile the FW4SPL dependencies using jom in the console: 

    * go to the build directory (e.g. Dev\\BinPkgs\\Build\\Debug or Release)
    * Use "jom all" to compile all the dependencies
    * Use "jom name_of_target" to compile only the wanted target

.. code:: bash

    > cd Dev\BinPkgs\Build\Debug
    > jom all

* All the generate library are in the install directory (e.g. Dev\BinPkgs\Install\Debug or Release)

.. note:: To prevent any futur problemes with source generation, check if all of the library has been compiled

Source
~~~~~~
    
* `Clone <http://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository#Cloning-an-Existing-Repository>`_ the following repositories in the (Dev\Src) source folder:
    * `fw4spl <https://github.com/fw4spl-org/fw4spl.git>`_


.. code:: bash

    > cd Deps/Src
    > git clone https://github.com/fw4spl-org/fw4spl.git

.. note:: 
    - *Optionnal*: You can also clone these extension repositories:
        - `fw4spl-ar <https://github.com/fw4spl-org/fw4spl-ar.git>`_ contains functionalities for augmented reality (video tracking for instance).
        - `fw4spl-ext <https://github.com/fw4spl-org/fw4spl-ext.git>`_ contains experimental code.
        - `fw4spl-ogre <https://github.com/fw4spl-org/fw4spl-ogre.git>`_ contains a 3D backend using `Ogre3D <http://www.ogre3d.org/>`_.

* Check if all the cloned repositories are on the same `branch <https://git-scm.com/docs/git-branch>`_.

* Update the cloned repositories to the *11.0.4* `tag <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

.. warning:: Be sure to be in the 'VS2015 x64 Native Tools Command Prompt'

.. note:: 
    Make sure that CMake executable (cmake.exe and cmake-gui.exe)location is present in your PATH environment variable. 
    
    - SET PATH=%PATH%;D:\Tools\CMake\bin

.. note:: 
    Make sure that Ninja executable (ninja.exe) location is present in your PATH environment variable.
    
    - SET PATH=%PATH%;D:\Tools\ninja

* Call the cmake-gui.

.. code:: bash

    > cmake-gui

* Set the wanted Build directory (e.g. Dev\\Build\\Debug or Release)

* Set the wanted Source directory (e.g. Dev\\Src)

* During configure step, choose the generator 'Ninja' to compile FW4SPL sources.

* Set the following arguments:

    * ``ADDITIONAL_PROJECTS``: set the source location of fw4spl-ar, fw4spl-ext and fw4spl-ogre, separated by ";".
    * ``CMAKE_INSTALL_PREFIX``: set the install location (e.g. Dev\\Install\\Debug).
    * ``CMAKE_BUILD_TYPE``: set to Debug or Release.
    * ``EXTERNAL_LIBRARIES``: set the install path of the dependencies install directory (e.g. Dev\\BinPkgs\\Install\\Debug or Release).
    * ``PROJECT_TO_BUILD``: set the names of the applications to build (see Dev\Src\Apps or Dev\Src\Samples, ex: VRRender, Tuto01Basic ...), each project should be separated by ";".
    * ``ECLIPSE_PROJECT``: check this box if you want to generate an Eclipse project.

* If you want to generate installers:
    * ``PROJECT_TO_INSTALL``: set the names of the applications you want to install (i.e. VRRender).

.. note::
    - If ``PROJECT_TO_BUILD`` is empty, all application will be compiled
    - If ``PROJECT_TO_INSTALL`` is empty, no application will be installed
    
.. warning:: Make sure the arguments concerning the compiler (advanced arguments) point to Visual Studio.

* Generate the code. 

* Compile the FW4SPL source using ninja in the console: 

    * go to the build directory (e.g. Dev\\Build\\Debug or Release)
    * Use "ninja" if you want to compile all the applications set in CMake.
    * Use "ninja name_of_application" to compile only one of the applications set in CMake.

.. code:: bash

    > cd Dev\Build\Debug
    > ninja

Launch an application
-------------------------

After a successful compilation the application can be launched with the fwlauncher.exe from FW4SPL. 
Therefore the profile.xml of the application in the build folder has to be passed as argument. 

.. note:: Make sure that the external libraries directory is set to the path (set PATH=<FW4SPL Binpkgs path>\\Debug\\bin;<FW4SPL Binpkgs path>\\Debug\\x64\\vc12\\bin;%PATH%).

.. image:: ../media/launchApp.png

Generate an installer
-------------------------

After setting the applications for which you want to generate installers in the *PROJECT_TO_INSTALL* variable of CMake and generating the code, follow these two steps:
    * Run *ninja install application_to_install* in the Build directory
    * Run *ninja package* in the Build directory
The installer will be generated in the Build directory.
    
Recommended software
-------------------------

The following programs may be helpful for your developments:

* `Eclipse CDT <https://eclipse.org/cdt/>`_: Eclipse is a multi-OS Integrated Development Environment (IDE) for computer programming. 
* `Notepad++ <http://notepad-plus-plus.org/>`_: Notepad++ is a free source code editor, which is designed with syntax highlighting functionality. 
* `ConsoleZ <https://github.com/cbucher/console/wiki/Downloads>`_: ConsoleZ is an alternative command prompt for Windows, adding more capabilities to the default Windows command prompt. To compile FW4SPL with the console the windows command prompt has to be set in the tab settings. 

   
