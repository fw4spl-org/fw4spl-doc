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

.. include:: CommonDeps.rst

Compilation
++++++++++++++

Now you can compile the FW4SPL dependencies with make in the console, it will automaticaly download, build and install each dependency.

.. code:: bash

    $ make all
    $ make install_tool
    
.. include:: CommonSrc.rst

Recommended software
-------------------------

The following programs may be helpful for your developments:

- IDE:
    - `Qt creator <http://www.qt.io/download-open-source/#section-6>`_
    - `Eclipse CDT <https://eclipse.org/cdt/>`_.

- Versioning tools:
    - `SourceTree <http://www.sourcetreeapp.com/>`_

