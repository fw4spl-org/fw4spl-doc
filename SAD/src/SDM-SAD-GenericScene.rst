.. _generic_scene:

Generic Scene
==============

Overview
------------------------

A generic scene in FW4SPL is a visualization feature to visualize several elements like meshes or images in a scene.
The scene is based on VTK. The main task of the generic scene is to manage all visualization services of the different
elements contained in the scene. The generic scene is universal and therefore applicable for diverse visualization tasks.

As used in FW4SPL, the generic scene configures a VTK scene with a simple xml configuration. Hence, FW4SPL is mainly
used for medical assignments, the generic scene can be seen as fusion of a negatoscope and the 3D model visualization.

Components
------------------------

Manager
~~~~~~~~

The SRender is the manager service of the VTK scene. This service works on an object of type
`fwData::Composite` that contains all objects to display.

The manager retrieves its specified container. The VTK context (vtkRender and vtkRenderWindow) is installed in the
container of the manager.

The manager listens to the object signals of the associated `fwData::Composite` object. The transferred signals inform
the manager if objects in the `fwData::Composite` have been added, removed or changed. In response to the modifications
within the `fwData::Composite` object the manager supervises the starting and stopping of the visualization services
(adaptors explained below), which are specified in its configuration. Thus, an object is added or removed to the
`fwData::Composite` object, the corresponding adaptor which works on this object is started or stopped.

Adaptor
~~~~~~~~

An adaptor (inherited from ``::fwRenderVTK::IVtkAdaptorService``) is a service to manipulate or display a FW4SPL data.
Services representing an adaptor are managed by a generic scene (SRender).
The adaptors are the gateway between FW4SPL objects and VTK objects.
To respect the principles of the framework, adaptors are kept as generic as possible.
Therefore they are reusable in further applications or even adaptors.

An adaptor is a specific service that needs to implement the methods ``doStart``, ``doStop``, ``doUpdate``, ``doConfigure`` and
``doSwap`` instead of the usual ``starting``, ``updating``, ...


.. code-block:: cpp

    class MyAdaptor : public ::fwRenderVTK::IVtkAdaptorService
    {

    public:

        fwCoreServiceClassDefinitionsMacro ( (MyAdaptor)(::fwRenderVTK::IVtkAdaptorService) );

    protected:

        /// Parse the adaptor "config" tag
        void configuring() throw(fwTools::Failed);

        /// Initialize the vtk pipeline (actor, mapper, ...)
        void doStart();

        /// Clear the vtk pipeline
        void doStop();

        /// Update the pipeline from the current object
        void doUpdate();

        /// Update the pipeline with the new object (eventually call doStop();doStart();)
        void doSwap();
    };


Configuration
--------------

.. code-block:: xml

    <service uid="generiSceneUID" impl="::fwRenderVTK::SRender" type="::fwRender::IRender">
        <scene renderMode="auto|timer|none" offScreen="imageKey" width="1920" height="1080">
            <renderer id="myRenderer" layer="0" background="0.0" />
            <vtkObject id="transform" class="vtkTransform" />
            <picker id="negatodefault" vtkclass="fwVtkCellPicker" />
            
            <adaptor uid="meshAdaptor" />
            <adaptor uid="imageAdaptor" />

        </scene>
        <fps>30</fps><!-- used if renderMode=="timer" -->
    </service>


renderMode (optional, "auto" by default)
    This attribute is forwarded to all adaptors. For each adaptor, if renderMode="auto",  the scene is automatically
    rendered after doStart, doUpdate, doSwap, doStop and m_vtkPipelineModified=true. If renderMode="timer" the scene is
    rendered at N frame per seconds (N is defined by **fps** tag). If renderMode="none" you should call 'render' slot to
    call reder the scene.

offScreen (optional):
    Key of the image used for off screen render

width (optional, "1280" by default):
    Width for off screen render

height (optional, "720" by default):
    Height for off screen render

renderer
    Defines a renderer. At least one renderer is mandatory, but there can be multiple renderer on different layers.

    - **id** (mandatory): the identifier of the renderer
    - **layer** (optional): defines the layer of the vtkRenderer. This is only used if there are layered renderers.
    - **background** (optional): the background color of the rendering screen.

    The color value can be defined as a grey level value (ex . 1.0 for white) or as a hexadecimal value (ex : \#ffffff for white).

vtkObject
    Represents a vtk object. It is usually used for vtkTransform or vtkImageBlend.

   - **id** (mandatory): the identifier of the vtkObject
   - **class** (mandatory): the classname of the vtkObject to create. For example vtkTransform, vtkImageBlend, ...

picker
    Represents a picker.

    - **id** (mandatory): the identifier of the picker
    - **vtkclass** (optional, by default vtkCellPicker): the classname of the picker to create.

adaptor
    Defines the adaptors to display in the scene.
    
    - **uid** (mandatory): the uid of the adaptor service


**Adaptors are written as other services in the xml**
   
.. code-block:: xml

    <service uid="meshAdaptor" type="::visuVTKAdaptor::SMesh" autoConnect="yes">
        <in key="mesh" uid="meshUID" />
        <config renderer="default" picker="" uvgen="sphere" />
    </service>

