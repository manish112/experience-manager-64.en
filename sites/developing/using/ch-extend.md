---
title: Extending ContextHub
seo-title: Extending ContextHub
description: null
seo-description: Define new types of ContextHub stores and modules when the ones provided do not meet your solution requirements
uuid: c13f87c5-b40d-4190-a8c9-648d7c98935c
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.4/SITES
topic-tags: personalization
content-type: reference
discoiquuid: c38ac688-8bf9-4de9-a5e6-40ef43892b7a
isreadyforlocalization: false
index: y
internal: n
snippet: y
---

# Extending ContextHub{#extending-contexthub}

Define new types of ContextHub stores and modules when the ones provided do not meet your solution requirements.

## Creating Custom Store Candidates {#creating-custom-store-candidates}

ContextHub stores are created from registered store candidates. To create a custom store, you need to create and register a store candidate.

The javascript file that includes the code that creates and registers the store candidate must be included in a [client library folder](../../developing/using/clientlibs.md#main-pars-title-0). The category of the folder must match the following pattern:

```xml
contexthub.store.[storeType]
```

The `[storeType]` part of the category is the storeType with which the store candidate is registered. (See [Registering a ContextHub Store Candidate](../../developing/using/ch-extend.md#main-pars-title-2052793894)). For example, for the storeType of `contexthub.mystore`, the category of the client library folder must be `contexthub.store.contexthub.mystore`.

### Creating a ContextHub Store Candidate {#creating-a-contexthub-store-candidate}

To create a store candidate, you use the [ `ContextHub.Utils.inheritance.inherit`](../../developing/using/contexthub-api.md#main-pars-title-33) function to extend one of the base stores:

* ` [ContextHub.Store.PersistedStore](../../developing/using/contexthub-api.md#main-pars-title-2)`
* ` [ContextHub.Store.SessionStore](../../developing/using/contexthub-api.md#main-pars-title-3)`
* ` [ContextHub.Store.JSONPStore](../../developing/using/contexthub-api.md#main-pars-title-0)`
* ` [ContextHub.Store.PersistedJSONPStore](../../developing/using/contexthub-api.md#main-pars-title-1)`

Note that each base store extends the ` [ContextHub.Store.Core](../../developing/using/contexthub-api.md#main-pars-title)` store.

The following example creates the simplest extension of the `ContextHub.Store.PersistedStore` store candidate:

```
myStoreCandidate = function(){};
ContextHub.Utils.inheritance.inherit(myStoreCandidate,ContextHub.Store.PersistedStore);

```

Realistically, your custom store candidates will define additional functions or override the store's initial configuration. Several [sample store candidates](../../developing/using/ch-samplestores.md) are installed in the repository below /libs/granite/contexthub/components/stores. To learn from these samples, use CRXDE Lite to open the javascript files.

### Registering a ContextHub Store Candidate {#registering-a-contexthub-store-candidate}

Register a store candidate to integrate it with the ContextHub framework and enable stores to be created from it. To register a store candidate, use the [ `registerStoreCandidate`](../../developing/using/contexthub-api.md#main-pars-title-36) function of the `ContextHub.Utils.storeCandidates` class.

When you register a store candiate, you provide a name for the store type. When creating a store from the candidate, you use the store type to identify the candidate upon which it is based.

When you register a store candidate, you indicate its priority. When a store candidate is registered using the same store type as an already-registered store candidate, the candidate with the higher priority is used. Therefore, you can override existing store candidates with new implementations.

```
ContextHub.Utils.storeCandidates.registerStoreCandidate(myStoreCandidate, 
                                'contexthub.mystorecandiate', 0);
```

## Creating ContextHub UI Module Types {#creating-contexthub-ui-module-types}

Create custom UI module types when the ones that are [installed with ContextHub](../../developing/using/ch-samplemodules.md) do not meet your requirements. To create a UI module type, create a new UI module renderer by extending the `ContextHub.UI.BaseModuleRenderer` class and then registering it with `ContextHub.UI`.

To create a UI moduler renderer, create a `Class` object that contains the logic that renders the UI module. At a minimum, your class must perform the following actions:

* Extend the `ContextHub.UI.BaseModuleRenderer` class. This class is the base implementation for all UI module renderers. The `Class` object defines a property named `extend` that you use to name this class as that which is being extended.

* Provide a default configuration. Create a `defaultConfig` property. This property is an object that includes the properties that are defined for the [ `contexthub.base`](../../developing/using/ch-samplemodules.md#main-pars-title) UI module, and any other properties that you require.

The source for `ContextHub.UI.BaseModuleRenderer` is located at /libs/granite/contexthub/code/ui/container/js/ContextHub.UI.BaseModuleRenderer.js.  To register the renderer, use the ` [registerRenderer](../../developing/using/contexthub-api.md#main-pars-title-1001018641)` method of the `ContextHub.UI` class. You need to provide a name for the module type. When administrators create a UI module based on this renderer, they specify this name.

Create and register the renderer class in a self-executing anonymous function. The following example is based on the source code for the contexthub.browserinfo UI module. This UI module is a simple extension of the `ContextHub.UI.BaseModuleRenderer` class.

```xml
;(function() {

    var SurferinfoRenderer = new Class({
        extend: ContextHub.UI.BaseModuleRenderer,

        defaultConfig: {
            icon: 'coral-Icon--globe',
            title: 'Browser/OS Information',

            storeMapping: {
                surferinfo: 'surferinfo'
            },

            template:
                '<p>{{surferinfo.browser.family}} {{surferinfo.browser.version}}</p>' +
                '<p>{{surferinfo.os.name}} {{surferinfo.os.version}}</p>'
        }
    });

    ContextHub.UI.registerRenderer('contexthub.browserinfo', new SurferinfoRenderer());

}());
```

The javascript file that includes the code that creates and registers the renderer must be included in a [client library folder](../../developing/using/clientlibs.md#main-pars-title-0). The category of the folder must match the following pattern:

```xml
contexthub.module.[moduleType]
```

The `[moduleType]` part of the category is the `moduleType` with which the module renderer is registered. For example, for the `moduleType` of `contexthub.browserinfo`, the category of the client library folder must be `contexthub.module.contexthub.browserinfo`.