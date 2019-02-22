---
title: MSM Rollout Conflicts
seo-title: MSM Rollout Conflicts
description: Learn how to deal with Multi Site Manager rollout conflicts.
seo-description: Learn how to deal with Multi Site Manager rollout conflicts.
uuid: 6ae0c838-6c23-4657-ba7d-35c385fafa0f
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.4/SITES
topic-tags: site-features
content-type: reference
discoiquuid: d9511c2f-5696-4c09-b83b-9364d0c973fb
index: y
internal: n
snippet: y
---

# MSM Rollout Conflicts{#msm-rollout-conflicts}

Conflicts can occur if new pages with the same page name are created in both the blueprint branch and a dependent live copy branch.

Such conflicts need to be handled and resolved upon rollout.

## Conflict Handling {#conflict-handling}

When conflicting pages do exist (in the blueprint and live copy branches), MSM allows you to define how (or even if) they should be handled.

To ensure that the rollout is not blocked, possible definitions can include:

* which page (blueprint or live copy) will have priority during rollout,
* which pages will be renamed (and how),  
* how this will affect any published content.  
  The default behavior of AEM (out-of-the-box) is that published content will not be impacted. So if a page that was manually created in the live copy branch has been published, that content will still be published after the conflict handling and rollout.

In addition to the standard functionality, customized conflict handlers can be added to implement different rules. These can also allow publishing actions as an individual process.

### Example Scenario {#example-scenario}

In the following sections we use the example of a new page `b`, created in both the blueprint and the live copy branch (created manually), to illustrate the various methods of conflict resolution:

* blueprint: `/b`  
  A master page; with 1 child page, bp-level-1. 

* live copy: `/b`  
  A page manually created in the live copy branch; with 1 child page, `lc-level-1`.

    * Activated on publish as `/b`, together with the child page.

<table border="1" cellpadding="1" cellspacing="0" height="159" width="533"> 
 <caption>
   Before Rollout 
 </caption> 
 <tbody> 
  <tr> 
   <td width="30%"><strong>blueprint before rollout</strong></td> 
   <td width="30%"><strong>live copy before rollout</strong></td> 
   <td width="40%"><strong>publish before rollout</strong></td> 
  </tr> 
  <tr> 
   <td><span class="code">b</span><br /> <br /> (created in blueprint branch, ready for rollout)<br /> </td> 
   <td><span class="code">b</span><br /> <br /> (manually created in live copy branch)<br /> </td> 
   <td><span class="code">b</span><br /> <br /> (contains the content of the page b that was manually created in the live copy branch)</td> 
  </tr> 
  <tr> 
   <td><code class="code"> /bp-level-1

      </code></td> 
   <td><span class="code"> /lc-level-1</span><br /> <br /> (manually created in live copy branch)<br /> </td> 
   <td><span class="code"> /lc-level-1</span><br /> <br /> (contains the content of the page<br /> child-level-1 that was manually created in the live copy branch)</td> 
  </tr> 
 </tbody> 
</table>

## Rollout Manager and Conflict Handling {#rollout-manager-and-conflict-handling}

The rollout manager allows you to activate or deactivate conflict management.

This is done using [OSGi configuration](../../../sites/deploying/using/configuring-osgi.md) of **Day CQ WCM Rollout Manager**:

* **Handle conflict with manually created Pages**:  
  ( `rolloutmgr.conflicthandling.enabled`)  
  Set to true if the rollout manager should handle conflicts from a page created in the live copy with a name that exists in the blueprint.

AEM has [predefined behavior when conflict management has been deactivated](#behaviorwhenconflicthandlingdeactivated).

## Conflict Handlers {#conflict-handlers}

AEM uses conflict handlers to resolve any page conflicts that exist when rolling out content from a blueprint to a live copy. Renaming pages is one (the usual) method of resolving such conflicts. More than one conflict handler can be operational to allow for a selection of different behaviors.

AEM provides:

* The [default conflict handler](#defaultconflicthandler):

    * `ResourceNameRolloutConflictHandler`

* The possibility to implement a [customized handler](#customizedhandlers).
* The service ranking mechanism that allows you to set the priority of each individual handler. The service with the highest ranking is used.

### Default Conflict Handler {#default-conflict-handler}

The default conflict handler:

* Is called `ResourceNameRolloutConflictHandler`  

* With this handler the blueprint page is given precedence.
* The service ranking for this handler is set low ( ``i.e. below the default value for the `service.ranking` property) as the assumption is that customized handlers will need a higher ranking. However, the ranking is not the absolute minimum to ensure flexibility when required.

This conflict handler gives precedence to the blueprint. The live copy page `/b` is moved (within the live copy branch) to `/b_msm_moved`.

* live copy: `/b`  
  Is moved (within the live copy) to `/b_msm_moved`. This acts as a backup and ensures that no content is lost.

    * `lc-level-1` is not moved.

* blueprint: `/b`  
  Is rolled out to the live copy page `/b`.

    * `bp-level-1` is rolled out to the livecopy.

<table border="1" cellpadding="1" cellspacing="0" width="700"> 
 <caption>
   After Rollout 
 </caption> 
 <tbody> 
  <tr> 
   <td style="text-align: left;" valign="top"><strong>blueprint after rollout</strong></td> 
   <td colspan="2" style="text-align: left;" valign="top" width="30%"><strong>live copy after rollout</strong><br /> </td> 
   <td style="text-align: left;" valign="top" width="30%"><strong>live copy after rollout</strong><br /> <br /> <br /> </td> 
   <td style="text-align: left;" valign="top" width="30%"><strong>publish after rollout</strong><br /> <br /> </td> 
  </tr> 
  <tr> 
   <td style="text-align: left;" valign="top"><span class="code">b</span></td> 
   <td colspan="2" style="text-align: left;" valign="top"><span class="code">b</span><br /> <br /> (has the content of the blueprint page b that was rolled out)<br /> </td> 
   <td style="text-align: left;" valign="top"><span class="code">b_msm_moved</span><br /> <br /> (has the content of the page b that was manually created in the live copy branch)</td> 
   <td style="text-align: left;" valign="top"><span class="code">b</span><br /> <br /> (no change; contains the content of the original page b that was manually created in the live copy branch and is now called b_msm_moved)<br /> </td> 
  </tr> 
  <tr> 
   <td style="text-align: left;" valign="top"><span class="code"> /bp-level-1</span></td> 
   <td style="text-align: left;" valign="top"><code class="code"> /bp-level-1
      
      </code></td> 
   <td style="text-align: left;" valign="top"><span class="code"> /lc-level-1</span><br /> <br /> (no change)</td> 
   <td style="text-align: left;" valign="top"><span class="code"> </span></td> 
   <td style="text-align: left;" valign="top"><span class="code"> /lc-level-1</span><br /> <br /> (no change)</td> 
  </tr> 
 </tbody> 
</table>

### Customized Handlers {#customized-handlers}

Customized conflict handlers allow you to implement your own rules. Using the service ranking mechanism you can also define how they interact with other handlers.

Customized conflict handlers can:

* Be named according to your requirements. ``
* Be developed/configured according to your requirements; for example, you can develop a handler so that the live copy page is given precedence.
* Can be designed to be configured using the [OSGi configuration](../../../sites/deploying/using/configuring-osgi.md); in particular the:

    * **Service Ranking**:  
      Defines the order related to other conflict handlers ( `service.ranking`).  
      The default value is 0.

### Behavior When Conflict Handling Deactivated {#behavior-when-conflict-handling-deactivated}

If you manually [deactivate conflict handling](#rolloutmanagerandconflicthandling) then AEM takes no action on any conflicting pages (non-conflicting pages are rolled out as expected).

>[!CAUTION]
>
>AEM does not give any indication that conflicts are being ignored as this behavior must be explicitly configured, so it is assumed that it is the required behavior.

In this case the live copy effectively takes precedence. The blueprint page `/b` is not copied and the live copy page `/b` is left untouched.

* blueprint: `/b`  
  Is not copied at all, but is ignored.

* live copy: `/b`  
  Stays the same.

<table border="1" cellpadding="1" cellspacing="0" width="100%"> 
 <caption>
   After Rollout 
 </caption> 
 <tbody> 
  <tr> 
   <td style="text-align: left;" valign="top"><strong>blueprint after rollout</strong></td> 
   <td style="text-align: left;" valign="top" width="40%"><strong>live copy after rollout</strong><br /> <br /> <br /> </td> 
   <td style="text-align: left;" valign="top" width="40%"><strong>publish after rollout</strong><br /> <br /> </td> 
  </tr> 
  <tr> 
   <td style="text-align: left;" valign="top"><span class="code">b</span></td> 
   <td style="text-align: left;" valign="top"><span class="code">b</span><br /> <br /> (no change; has the content of the page b that was manually created in the live copy branch)</td> 
   <td style="text-align: left;" valign="top"><span class="code">b</span><br /> <br /> (no change; contains the content of the page b that was manually created in the live copy branch)<br /> </td> 
  </tr> 
  <tr> 
   <td style="text-align: left;" valign="top"><span class="code"> /bp-level-1</span><br /> </td> 
   <td style="text-align: left;" valign="top"><span class="code"> /lc-level-1</span><br /> <br /> (no change)</td> 
   <td style="text-align: left;" valign="top"><span class="code"> /lc-level-1</span><br /> <br /> (no change)</td> 
  </tr> 
 </tbody> 
</table>

### Service Rankings {#service-rankings}

The [OSGi](https://www.osgi.org/) service ranking can be used to define the priority of individual conflict handlers.