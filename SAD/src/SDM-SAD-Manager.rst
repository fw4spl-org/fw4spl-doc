Manager and updater services
==================================

Concepts
--------

In the FW4SPL architecture, there is an object container which is often used:
``::fwData::Composite``. This container is also an Object and represents a map
which associates a string with an Object. The architecture provides two main
services to manage a Composite: a composite updater and a service manager.

Updater
~~~~~~~

The updater service extends service type ``::ctrlSelection::IUpdaterSrv`` and
the work on a selection composite. This kind of service listens specific events
from objects identified by their UID. When it receives an event, it performs an
operation on an object in the selection composite and notifies composite
listeners.

Available operations on composite are:

- Adding an object
- Swapping an object
- Removing an object
- Removing an object if present
- Adding or swapping an object
- Doing nothing

There are few generic updater services which listen all events sent by Objects,
and few other which work with particular Object events.

Implementation
--------------

Updater
~~~~~~~

An updater implementation must inherit from the ``::ctrlSelection::IUpdaterSrv`` service.

In the example below, an updater is used to manage a ``::fwData::Reconstruction`` object identified with 
the ``reconstruction`` key in a selection composite. This ``::fwData::Reconstruction`` is stored in a
``::fwMedData::ModelSeries`` and we used a specific updater to listen signals and manage the structure.

The updater provides slots to react on object/service signals.


Example
********

For example, the updater ``::ctrlSelection::SObjFromSlots`` provides the following slots :

- ``add(object)``: add the given object in the composite with the configured key
- ``swapObj(object)``: swap the given object in the composite with the configured key
- ``addOrSwap(object)``: if the configured key exists in the composote, the object is swapped, else it is added
- ``remove()``: remove the object with the configured key from the composite
- ``removeIfPresent()``: remove the object if the configured key exists in the composite

Updater configuration example:

.. code-block:: xml

    <object id="model" type="::fwMedData::ModelSeries">
        <service uid="listOrganEditor" type="::uiMedData::editor::SModelSeriesList" autoConnect="yes" />
    </object>

    <object type="::fwData::Composite">
        <service uid="myUpdater" type="::ctrlSelection::updater::SObjFromSlot" >
            <out key="object" uid="reconstruction" /> <!-- key of the updated object -->
        </service>
    </object>

    <!-- connect updater to listen the reconstruction selection -->
    <connect>
        <signal>listOrganEditor/reconstructionSelected</signal>
        <slot>myUpdater/addOrSwap</slot>
    </connect>

