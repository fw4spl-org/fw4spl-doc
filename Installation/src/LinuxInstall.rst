Installation for Linux
======================

Prerequisites 
---------------

If not already installed:

#. Install `git <https://git-scm.com/>`_
#. Install `gcc <https://gcc.gnu.org/>`_ The minimal version required is 4.8 or `clang <http://clang.llvm.org/>`_ The minimal version required is 3.5
#. Install `Python 2.7 <https://www.python.org/downloads/>`_
#. Install `CMake <http://www.cmake.org/download/>`_. The minimal version required is **3.7** if you want to compile with precompiled headers (build twice faster, enabled by default). Otherwise you can use a 3.1 version.
#. Install `Ninja <https://ninja-build.org/>`_

Depending on which linux distribution you use, for example on **Debian/Ubuntu/Mint** you can run the following command to download and install these tools:

.. code:: bash

    $ apt-get install build-essential ninja-build python2.7 git cmake

.. warning::
    If the **CMake** version of your distribution is not sufficient (Mint 17 for instance ships only the 2.18 version), you can easily grab it on the `Kitware website <https://cmake.org/download/>`_. Download the **binary** version (much easier than compiling yourself), extract it to a folder (i.e. /home/login/software/cmake/) and add the ``bin/`` folder inside it to your ``PATH`` environment variable:

    .. code:: bash

        # ~/.bashrc
        export PATH=/home/login/software/cmake/bin:$PATH

Few basic development libraries need to be installed first: ``zlib``, ``iconv``, ``jpeg``, ``png``, ``tiff``, ``freetype``, ``libxml``, ``expat``, and ``icu``. On **Mint 18.x** for instance, you can install them using the following command : 

.. code:: bash

    $ sudo apt-get install libz3-dev libiconv-hook-dev libpng12-dev \
      libjpeg-turbo8-dev libtiff5-dev libfreetype6-dev libxml2-dev \
      libexpat1-dev libicu-dev

Next, we also need to install specific development libraries for **Qt**. These requirements are detailed here:

- http://wiki.qt.io/Building_Qt_5_from_Git

Follow the instructions there to install the necessary packages on your system for **Build essentials**, **libxcb** and **QtMultimedia**. For the latter, please note that we use **gstreamer-1.0** by default, so please replace ``libgstreamer0.10-dev`` and ``libgstreamer-plugins-base0.10-dev`` by ``libgstreamer1.0-dev`` and ``libgstreamer-plugins-base1.0-dev``. You can safely ignore instructions for QtWebKit and QtWebEngine, we don't build them.
Since we build Qt with openssl support you also need to install ``libssl-dev`` (be sure that the version is equal or upper to 1.0.0).
``libudev-dev`` and ``libusb-1.0.0-dev`` are required by the OpenNI library. Last for VTK we also need the X Toolkit Intrinsics library headers, that you can easily install for instance on a Debian-based distribution with the packages ``libxt-dev``, ``libxrandr-dev`` and ``libxaw7-dev``.

If you plan to build extra dependencies, the VLC libraries are also needed, regarding to streaming capabilities, and thus the packages: ``libvlc-dev``, ``libvlccore-dev`` and ``vlc-nox``, are required.

Finally, please note that we provide Dockerfile at this `link <https://github.com/fw4spl-org/docker-fw4spl-deps>`_. You can have a look at the Dockerfile to get the precise list of commands needed to install dependencies.

.. include:: CommonDeps.rst

Build
~~~~~~~~~

Now you can compile the FW4SPL dependencies with make in the console, it will automatically download, build and install each dependency.

.. code:: bash

    $ cd Dev\BinPkgs\Build\Debug
    # Adjust the number of cores depending of the CPU cores and the RAM available on your computer
    $ make -j4 install
    
.. include:: CommonSrc.rst

Recommended software
--------------------------------

The following programs may be helpful for your developments:

- `Eclipse CDT <https://eclipse.org/cdt/>`_

- `QtCreator <https://www.qt.io/download-open-source/#section-2>`_

