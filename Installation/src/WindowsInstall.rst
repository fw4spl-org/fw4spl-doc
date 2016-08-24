Installation for Windows
=========================

Prerequisites for Windows users
--------------------------------

If not already installed:

1. Install `git <https://git-scm.com/>`_

2. Optionally you can install `TortoiseHg <http://tortoisehg.bitbucket.org/>`_

3. Install `Visual Studio 2013 Community <https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx>`_

4. Install `Python 2.7 <https://www.python.org/downloads/>`_

5. Install `CMake <http://www.cmake.org/download/>`_

6. Install `jom <http://wiki.qt.io/Jom>`_

7. Install `ninja <https://github.com/martine/ninja/releases>`_

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

.. .. image:: media/Directories.png

* Set the environment for a x64 version.
  To compile BinPkgs and sources, you must use the 'VS2013 x64 Native Tools Command Prompt' 

Dependencies
~~~~~~~~~~~~~~~~~

.. warning:: Be sure to be in the 'VS2013 x64 Native Tools Command Prompt'

* `Clone <http://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository#Cloning-an-Existing-Repository>`_ the following repository in the (BinPkgs) source folder:

    * `fw4spl-deps <https://github.com/fw4spl-org/fw4spl-deps.git>`_

* Update the cloned repositories to the *fw4spl_0.11.0* tag.

.. note:: Make sure that CMake executable location is present in your PATH environment variable. 

* Call the cmake-gui

* During Configure, choose the generator 'NMake Makefiles JOM'. 

.. note:: Make sure that JOM executable location is present in your PATH environment variable.

* Set the following arguments:

    * *CMAKE_INSTALL_PREFIX*: set the install location.
    * *CMAKE_BUILD_TYPE*: set to Debug or Release.
    * *ADDITIONAL_PROJECTS*: you can leave it empty, it is only needed if you have an extra source location like fw4spl-ext-deps or a custom repository.

* Generate the code. 

* Compile the FW4SPL dependencies with jom in the console (e.g. jom all, jom qt, etc).

Source
~~~~~~

.. warning:: Be sure to be in the 'VS2013 x64 Native Tools Command Prompt'
    
* `Clone <http://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository#Cloning-an-Existing-Repository>`_ the following repositories in the (Dev) source folder:
    * `fw4spl <https://github.com/fw4spl-org/fw4spl.git>`_

* Update the cloned repositories to the *fw4spl_0.11.0* tag.

.. note:: Make sure that CMake executable location is present in your PATH environment variable.

* Call the cmake-gui.

* During configure step, choose the generator ('NMake Makefiles JOM' to compile BinPkgs or 'Ninja' to compile FW4SPL sources).

.. note:: Make sure that Ninja and JOM executables location are present in your PATH environment variable.

* Set the following arguments:

    * *ADDITIONAL_PROJECTS*: set the source location of fw4spl-ar and fw4spl-ext
    * *CMAKE_INSTALL_PREFIX*: set the install location.
    * *CMAKE_BUILD_TYPE*: set to Debug or Release
    * *EXTERNAL_LIBRARIES*: set the install path of the third part libraries.
    * *PROJECT_TO_BUILD* set the name of the application to build (see Dev\Src\Apps, ex: VRRender, Tuto01Basic ...), each project should be separated by ";".
    * *PROJECT_TO_INSTALL* set the name of the application to install (i.e. VRRender)

.. note::
    - If PROJECT_TO_BUILD is empty, all application will be compiled
    - If PROJECT_TO_INSTALL is empty, no application will be installed
    
.. warning:: Make sure the arguments concerning the compiler (advanced arguments) point to Visual Studio.

* Generate the code. 

* Compile the FW4SPL source code with ninja in the console. 

.. note:: it is possible to generate eclipse project with CMake. You just have to check ECLIPSE_PROJECT.

Launch an application
-------------------------

After an successful compilation the application can be launched with the fwlauncher.exe from FW4SPL. 
Therefore the profile.xml of the application in the build folder has to be passed as argument. 

.. note:: Make sure that the external libraries directory is set to the path (set PATH=<FW4SPL Binpkgs path>\\Debug\\bin;<FW4SPL Binpkgs path>\\Debug\\x64\\vc12\\bin;%PATH%).

.. image:: ../media/launchApp.png

Extensions
----------

**fw4spl** has two main extension repositories:

- `fw4spl-ar <https://github.com/fw4spl-org/fw4spl-ar.git>`_: extension of fw4spl repository, contains functionalities for augmented reality (video tracking for instance).
- `fw4spl-ogre <https://github.com/fw4spl-org/fw4spl-ogre.git>`_: another extension of fw4spl, contains a 3D backend using `Ogre3D <http://www.ogre3d.org/>`_.

If you want to use them, clone them first in the (Dev) source folder. Then modify *ADDITIONAL_PROJECTS* in cmake-gui to set the source location of fw4spl-ar and fw4spl-ogre separated by ';'. Last, re-generate the code and compile with ninja.

Recommended software
-------------------------

The following programs may be helpful for your developments:

* `Eclipse CDT <https://eclipse.org/cdt/>`_: Eclipse is a multi-OS Integrated Development Environment (IDE) for computer programming. 
* `Notepad++ <http://notepad-plus-plus.org/>`_: Notepad++ is a free source code editor, which is designed with syntax highlighting functionality. 
* `ConsoleZ <https://github.com/cbucher/console/wiki/Downloads>`_: ConsoleZ is an alternative command prompt for Windows, adding more capabilities to the default Windows command prompt. To compile FW4SPL with the console the windows command prompt has to be set in the tab settings. 

   
