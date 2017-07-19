************
Testing
************

.. toctree::
    :maxdepth: 2

   

.. _CTest: http://cmake.org/
.. _CMake: http://cmake.org/
.. _CppUnit: https://sourceforge.net/projects/cppunit
.. _fw4spl: https://github.com/fw4spl-org/fw4spl

fw4spl_ uses CTest_ and CppUnit_ for unit testing.

Building
--------

When building fw4spl_ with CMake_, you will need to enable the ``BUILD_TESTS`` option, e.g. with the ``-DBUILD_TESTS=ON`` command line option.

Launching unit tests
--------------------

In you build directory, you can launch the unit tests with the ``ctest`` command in the following way:

.. code-block:: shell

    # Launch the tests sequentially
    ctest .

    # Launch the tests using 4 jobs, similar to the -j option of make
    ctest -j 4 .

    # You can also use the make or ninja commands to do so
    make test

    ninja test

Additional data
---------------

Additional data need to be download to run all the unit tests. They are available at the following `link <https://owncloud.ircad.fr/index.php/s/zT2FzoTCJiMZdGo/download>`_.
You can then specify the directory, where the data are located, with the ``FWTEST_DATA_DIR`` environment variable.

