*************************
How to add a new Deps ?
*************************

fw4spl dependencies are based on the `ExternalProject <http://www.cmake.org/cmake/help/v3.0/module/ExternalProject.html>`_ concept from lastest versions of cmake.

The concept is to create custom targets to build projects in external trees.
Each project has custom steps for download, update/patch, configure, build and install.

You may want to add a new dependency into fw4spl-deps or you may want to add your own folder of dependencies.

.. tip::
    You need to know that the main CMakeLists.txt is in fw4spl-deps, and you can add as many additional folders as you want.
    Use the ``ADDITONNAL_DEPS`` option in cmake to set the path of your custom deps.

Add a new deps in fw4spl-deps
------------------------------

Adding a new deps is quite easy, the only things to do is to add a new folder *myNewDeps* and put a CMakeLists.txt file into it.
The CMakeLists.txt should contain at least:

- cmake_minimum_required(...)
- project(...)
- include(ExternalProject)
- ExternalProject_Add(...)

Here is a simple example from camp :

.. code-block:: cmake

    cmake_minimum_required(VERSION 2.8)

    project(campBuilder)

    include(ExternalProject)

    set(CAMP_CMAKE_ARGS ${COMMON_CMAKE_ARGS}
                        -DBUILD_DOXYGEN:BOOL=OFF
                        -DBOOST_INCLUDEDIR:PATH=${CMAKE_INSTALL_PREFIX}/include/boost-1_57
    )

    getCachedUrl(https://github.com/greenjava/camp/archive/0.7.1.1.tar.gz CACHED_URL)

    ExternalProject_Add(
        camp
        URL ${CACHED_URL}
        DOWNLOAD_DIR ${ARCHIVE_DIR}
        DEPENDS boost
        INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
        CMAKE_ARGS ${CAMP_CMAKE_ARGS}
    )

The important parts are in the *ExternalProject_Add* fonction:

- URL: is the download link of the sources
- DOWNLOAD_DIR: The folder where the sources will be stored (set globaly for all deps)
- DEPENDS: The dependencies of the current library (will be compiled before)
- INSTALL_DIR: The folder in which the library will be installed (set globaly for all deps)
- CMAKE_ARGS: CMake options for library which have a cmake build system

.. note::

    Note that in other script you can have much more options like:

    - PATCH_COMMAND
    - CONFIGURE_COMMAND
    - BUILD_COMMAND
    - INSTALL_COMMAND

    Refer you to the documentation of `ExternalProject <http://www.cmake.org/cmake/help/v3.0/module/ExternalProject.html>`_ for more informations.
 

Add a custom deps repository
-----------------------------

You may want to add your own folder of dependencies (as fw4spl-ext-deps).
In this case your main need to create a CMakeLists.txt in the root of your folder (myDepsFolder/CMakeLists.txt) in order to list the subdirectories of your deps.

.. code-block:: cmake

    cmake_minimum_required(VERSION 3.1)

    project(CustomDeps)

    list(APPEND SUBDIRECTORIES myDeps1)
    list(APPEND SUBDIRECTORIES myDeps2)
    ...

Then when you do a *ccmake* or *cmake-gui* in the build of your deps, you need to add the path to your custom repository in the ``ADDITONNAL_DEPS`` option.
Then cmake will automaticaly parsed your folder.


