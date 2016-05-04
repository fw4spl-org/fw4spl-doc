AppXml2
****************************************

Ce document résume les modifications proposées dans le cadre des travaux qui ont été regroupés sous la bannière "AppXml2". 
En réalité il s'agit davantage d'une refonte de l'approche objets/services qui a ensuite été appliquée
à un nouveau type d'application Xml.

1. Description de l'existant
===========================================

1.1. Un service travaille sur une donnée
-------------------------------------------

Notre approche objets/services repose sur le postulat qu'un service travaille sur une unique donnée. Si cela fonctionne parfaitement sur des cas simples, comme les readers ou writers, il nous arrive très rapidement de devoir dépasser cette contrainte dès qu'une application entière est construite. Bien souvent il n'est pas possible, ou pas souhaitable de découper un algorithme travaillant sur N données en N services.

Ce constat historique a mené à l'introduction d'une nouvelle donnée, le **composite**, contenant une map d'objets. Les données nécessaires au service sont introduites dans cette collection. Le service décrit ensuite quelles clés il utilise dans cette donnée. Nous nous retrouvons rapidement avec un composite partagé par plusieurs services avec de nombreuses clés. Pour isoler certains services (même si ce n'est pas l'unique raison), des composites peuvent même être ajoutés dans les composites. La lecture des XML devient pénible avec de nombreuses imbrications et conduit également à des jonglages de données entre les composite pour faire fonctionner le tout correctement.

Nous l'aurons compris toute application hors des cas triviaux comme les tutoriels utilise donc inévitablement un composite comme donnée principale et de nombreux services travaillent sur des composites, donc des données multiples.

1.2. Récupération des données et connections
----------------------------------------------

L'implication en terme de développement d'un service est loin d'être anodine. Pour chaque service, il faut déclarer quelles clés utiliser dans la configuration du service. Il faut donc parser une string dans l'implémentation de *IService::configuring()*, la stocker puis récupérer la donnée associée dans le composite dans *IService::starting()*. 

Ensuite pour être notifié des changements, il faut créer les connections à la main, indépendamment de l'attribut autoConnect du service, car la fonction virtuelle *IService::getObjSrvConnections()* ne travaille que sur le composite lui-même. Entre un service qui travaille directement sur une image ou un service qui travaille sur une image dans un composite, il y aura donc des différences notables en terme de setup avant de pouvoir travailler sur la donnée. Du point de vue l'XML, il est facile de déterminer si le service se connecte à l'image avec l'attribut *autoConnect* alors que dans le 2e cas il faut bien souvent regarder dans le code du service.

À cause de la complexité de l'organisation des données dans les composite, une dérive a conduit à utiliser directement les UID des objets au lieu des clés de composite. Le design devient alors définitivement caduque : le service a une donnée "principale", parfois difficile à déterminer et pioche ensuite des objets à sa guise dans un *pool* global, rendant ainsi le périmètre d'action d'un service encore plus difficile à lire. Sans compter que certains services mixent les clés et les UID...

1.3. Swapper
--------------

La plupart des services travaillent sur des données qui doivent être valides au lancement, ce qui est tout à fait normal, voire souhaitable. Toutefois, dans le cycle de vie d'une application, ces données ne sont pas toujours disponibles dès le lancement de l'application. À cause de l'écriture de ces applications en XML, il n'est pas possible de conditionner le lancement de services à la mise en disponibilité d'une donnée. Avec un langage de programmation fournissant des structures de contrôles, le problème ne se serait pas posé, du moins pas de façon aussi évidente.

Pour répondre à cette problématique, un service de services a été ajouté : le **SSwapper**. Il écoute l'ajout ou la suppression d'objets sur un composite et démarre ou stoppe une liste de services en fonction. En pratique, ce service est complexe à utiliser. Il dispose de plusieurs "modes" peu appréhendables sans connaissance approfondie des services; la lecture du XML est alourdie, les clés de composite ajoutées ou supprimées ne sont pas directement lisibles.

L'expérience sur plusieurs années a montré que la mise en place de ce service est toujours problématique, et pourtant elle est indispensable dans de nombreux cas.


2. Propositions
====================

2.1. Nouveau bundle
----------------------
Pour intégrer les propositions suivantes tout en gardant la compatibilité avec l'ancien système, nous proposons d'introduire un nouveau bundle appXml2. Conjointement, une AppConfig2, un AppConfigManager2 sont créés pour supporter toutes les modifications liées au Xml.

2.2. Un service travaille sur N données
-------------------------------------------

Nous proposons de supprimer l'indirection du composite et d'assumer le fait qu'un service travaille sur plusieurs données. 

Une AppConfig2 xml ne travaille plus sur une seule donnée également. Les objets sont spécifiés en tête de configuration :

.. code-block :: xml

    <config>

        <object uid="modelSeries" type="::fwMedData::ModelSeries" />
        <object uid="image" type="::fwData::Image" />
        ...

Un service propose une liste d'entrées (*in*), de sorties (*out*), et d'entrées/sorties(*inout*). Cette différence dans l'accès permet de mieux identifier comment les données sont employées et de sécuriser leur utilisation (les *in* ne sont accessibles qu'en *const*). Le parsing de ces entrées/sorties est réalisé par l'AppConfig, supprimant ainsi une partie du travail répétitif de parsing et de récupération des données. Les données sont accessibles par le service à l'aide de méthodes simples comme *getInput<>(const KeyType key)*, *getOutput(const KeyType key)*, etc...

Au niveau du xml de l'AppConfig2, cela change également l'écriture d'un service : il n'est plus inclus dans une balise <object> et il doit donc préciser chaque donnée utilisée :

.. code-block :: xml

        <service uid="imageReader" impl="::uiIO::editor::SIOSelector">
            <out key="target" uid="image" />
            <type mode="reader" />
        </service>
            
        <service uid="mesher50ServiceUID" impl="::opVTKMesh::SVTKMesher">
            <in key="image" uid="image" autoConnect="yes" />
            <out key="modelSeries" uid="modelSeries" />
            <config>
                <percentReduction>50</percentReduction>
            </config>
        </service>

L'écriture du xml est potentiellement plus verbeuse si de nombreux services utilisent la même donnée mais localement il est plus aisé d'identifier quelles sont les données utilisées par un service. À terme, il va de soit qu'aucun service ne devra accéder à des données autrement que par ce mécanisme et l'accès par UID sera à proscrire.

Dans la continuité, une nouvelle méthode *IService::getAutoConnections()* est implémentable pour définir, pour chaque clé, les connections à effectuer avec le service. L'attribut *autoConnect* est spécifiable globalement à toutes les données ou individuellement.

.. code-block :: cpp

    IService::KeyConnectionsMap SService::getAutoConnections() const
    {
        KeyConnectionsMap connections;
        connections.push("image", s_MODIFIED_SIG, s_UPDATE_IMAGE_SLOT);
        connections.push("image", s_BUFFER_MODIFIED_SIG, s_UPDATE_IMAGE_SLOT);
        connections.push("modelSeries", s_MODIFIED_SIG, s_UPDATE_MESH_SLOT);

        return connections;
    }
    
2.3. Données différées
------------------------

Nous proposons de supprimer le service SSwapper et autres services similaires comme le service SField. 

Pour répondre au besoin de lancement différé de services en fonction de la disponibilité d'une donnée, nous introduisons la notion de **donnée différée**. Il s'agit d'un attribut de build de la donnée, qui indique en premier lieu à l'AppConfig que la donnée ne doit pas être créée au lancement de celle-ci. Deuxièmement, cela indique implicitement que tous les services utilisant cette donnée **en entrée** ne seront pas créés eux aussi; toutefois ils seront instanciés puis démarrés automatiquement lorsque toutes leurs données différées seront disponibles.

Dans l'exemple suivant, le service *updaterReconst* travaille sur la donnée différée *reconst* **en sortie** et il est donc capable de démarrer. En revanche *organMaterial* ne peut démarrer car il utilise cette même donnée **en entrée**. Le service *updaterReconst* va produire la donnée à un instant donné, rendant la donnée *reconst* disponible pour le reste de l'AppConfig2. Le service *organMaterial* sera alors automatiquement lancé par l'AppConfig2. Si la donnée vient à disparaître il sera automatiquement stoppé. 

.. code-block :: xml
    
    <object uid="reconst" type="::fwData::Reconstruction" src="deferred" />

    <service uid="updaterReconst" impl="::ctrlSelection::updater::SObjFromSlot">
        <out key="target" uid="reconst" />
    </service>
            
    <service uid="organMaterial" impl="::uiReconstruction::OrganMaterialEditor">
        <inout key="reconstruction" uid="reconst" />
    </service>
    
Une donnée différée est créée, supprimée ou modifiée par un service travaillant sur cette donnée **en sortie**. Pour rendre cette donnée disponible aux autres services, la méthode *::fwServices::OSR::register* est utilisée. Celle-ci envoie un signal qui est intercepté par l'AppConfig2, qui peut démarrer ensuite les services concernés si toutes les conditions sont remplies.
    
La fonctionnalité proposée par le SSwapper est donc toujours présent, mais intégrée à l'AppConfig2, d'une manière proche des scènes génériques VTK, Ogre ou 2D. Dans le futur, il serait souhaitable d'homogénéiser ce comportement avec du code commun. Celui-ci pourrait également être utilisé si nécessaire dans le cadre de l'écriture d'une application sans le XML, en C++ ou un autre langage pour ne pas avoir à gérer cette problématique manuellement.
          
2.4. Données optionnelles
----------------------------

Dans un certain nombre de cas, il est souhaitable qu'un service travaillant sur une ou plusieurs données différées **en entrée** ne bloque pas sa création et son lancement sur leur disponibilité. Une scène générique par exemple peut travailler sur un mesh qui n'est pas présent au lancement; toutefois elle sait gérer son absence et son apparition/disparition. Il faut donc permettre ce cas, ce qu'il est possible de faire en précisant l'attribut "optional" :

.. code-block :: xml

    <service uid="organMaterial" impl="::uiReconstruction::OrganMaterialEditor">
        <inout key="reconstruction" uid="reconst" optional="yes"/>
    </service>
    
Dans ce contexte, un service pourra être notifié de l'apparition, de la modification ou de la disparition d'un objet grâce à la nouvelle méthode *IService::swapping(const KeyType&, ::fwData::Object::sptr)*.
    
2.5. Connections
-------------------

Pour simplifier l'écriture du xml, nous avons choisi de fusionner les balises *<connect>* et *<proxy>* dans le cadre de l'AppConfig2. La balise *<proxy>* est supprimée tandis qu'il a été ajouté la possibilité d'ajouter plusieurs signaux et un nom de canal sur la balise *<connect>*. En terme d'implémentation nous n'avons donc gardé, en réalité, que les proxys qui sont exposés dans le xml via la balise *<connect>*.

Ainsi le code suivant:

.. code-block :: xml

    <connect>
        <signal>listOrganEditor/reconstructionSelected</signal>
        <slot>updaterReconstUID/addOrSwap</slot>
    </connect>

    <proxy channel="modelSeriesNormalChannel">
        <signal>representationEditor/normalsModeModified</signal>
        <signal>representationEditor2/normalsModeModified</signal>
    </proxy>
            
devient :

.. code-block :: xml

    <connect>
        <signal>listOrganEditor/reconstructionSelected</signal>
        <slot>updaterReconstUID/addOrSwap</slot>
    </connect>

    <connect channel="modelSeriesNormalChannel">
        <signal>representationEditor/normalsModeModified</signal>
        <signal>representationEditor2/normalsModeModified</signal>
    </connect>
            

Dans le cadre d'un service utilisant une donnée différée, il faut noter que ces connections ne sont créés/détruites que lorsque ce service est démarré/stoppé par l'AppConfig2.

2.6. Debug
------------

Pour aider au débogage du démarrage des services, des logs ont été ajouté au niveau INFO, indiquant par exemple qu'un service n'a pas été démarré car une ou plusieurs ne sont pas disponibles (en précisant lesquelles), ou encore qu'un service a été démarré/stoppé à cause d'une création/destruction de donnée.

De façon générale, les erreurs sont remontées de façon plus explicite en essayant de préciser un contexte, notamment l'identifiant de la configuration en particulier, pour aider à comprendre les erreurs sans avoir à lancer un débogueur.
