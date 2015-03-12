
=======================================
Software Architecture Description (SAD)
=======================================


.. section-numbering::

.. contents::
    :depth: 1

General
========

Introduction
------------

All our software are developted with the open source framework :
FrameWork for Software Production Line as known as FW4SPL or F4S. This
framework, created by Ircad, is a modular workspace for rapid
application development. It provides some software modules to make
easier the creation of software application. It is mainly dedicated to
the medical software. Therefore, many functionalities like digital image
processing, simulation of medical interactions are allowed.

FW4SPL is a component-oriented architecture with the notion of
role-based programming. This architecture consists of a set of
cross-platform C++ libraries and it is multi-plateforms (runs under
Windows, Linux and MacOS). It has been built around three ideas:

-  The concept of data and services
-  The component approach
-  The communication links

This document will introduce the general architecture of FW4SPL.

Annexes
-------

-  Bundle and Srclib description: this document describes briefly all
   projects (libraries and components) used in the architecture.
-  Objects list: this document describes briefly all data used in the
   architecture.
-  Services list: this document describes briefly all services used in
   the architecture.
-  SOUP: this document contains a description of libraries used to
   support this architecture and its functionalities.
-  OSR diagram: this document introduces how an application
   configuration is represented on a diagram.
-  Medical data version: this document represents the life cycle of the
   Visible Patient medical data and its modifications between different
   software version


Object-Service concept
======================

.. include:: SDM-SAD-ObjService.rst


Signal-slot communication
=========================

.. .. include:: SDM-SAD-SigSlot.rst

App-config
=======================

.. include:: SDM-SAD-App-config.rst

.. [TODO]Activities
.. =======================
.. .. include:: SDM-SAD-Activities.rst

Multi-threading
=======================

.. .. include:: SDM-SAD-Thread.rst

Serialization
=======================

.. .. include:: SDM-SAD-Serialization.rst

.. [TODO]Environment management
.. ============================
.. .. include:: SDM-SAD-Env.rst

.. [TODO]Binary packages
.. =======================
.. .. include:: SDM-SAD-bp.rst

.. [TODO]Memory management
.. =======================
.. .. include:: SDM-SAD-memory.rst

Medical patient folder
============================

.. .. include:: SDM-SAD-PatientFolder.rst

.. [TODO]Generic scene
.. =======================
.. .. include:: SDM-SAD-GenericScene.rst

Manager and updater services
==================================

.. .. include:: SDM-SAD-Manager.rst

.. [TODO]Progress bar
.. =======================
.. .. include:: SDM-SAD-ProgressBar.rst

.. [TODO]Software license
.. ======================
.. .. include:: SDM-SAD-License.rst

.. [TODO]Log
.. =========
.. .. include:: SDM-SAD-Log.rst

.. [TODO]Application and launcher
.. ===============================
.. .. include:: SDM-SAD-Application.rst

Component-based software
==============================

.. .. include:: SDM-SAD-Component.rst

Graphical User Interface
========================

.. .. include:: SDM-SAD-GUI.rst

Appendix : Medical folder
=========================

.. .. include:: Appendix/SDM-SAD-APX-MedFolder.rst

