---
title: Activity stream in timeline
seo-title: Activity stream in timeline
description: This article describes how to display activity logs for assets on the timeline. 
seo-description: Display activity logs for assets on the timeline.
uuid: bf6dc1cc-afcb-4ec6-825d-f37a23fcfcdf
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.4/ASSETS
content-type: reference
topic-tags: authoring
discoiquuid: df8458a9-cd85-4170-b292-b4176cbe617f
---

# Activity stream in timeline{#activity-stream-in-timeline}

This article describes how to display activity logs for assets on the timeline. 

This feature displays activity logs for assets on the timeline. If you perform any of the following asset-related operations in Adobe Experience Manager (AEM) Assets, the Activity stream feature updates the timeline to reflect the activity.

The following operations are logged in the activity stream:

* Create
* Delete
* Download (including renditions)
* Publish
* Unpublish
* Approve
* Reject
* Move

The activity logs to be displayed in the timeline are fetched from the location */var/audit/com.day.cq.dam/content/dam* in CRX, where log files are stored.  

>[!NOTE]
>
>Transient workflows are not displayed in the timeline, because no history information is saved for these workflows.

To view the activity stream, perform one or more of the operations on the asset, select the asset, and then choose **Timeline** from the GlobalNav list.

![](assets/timeline-3.png)

The timeline displays the activity stream for the operations you perform on the assets. 

![](assets/activity_stream.png)

>[!NOTE]
>
>The default log storage location for **Publish** and **Unpublish** tasks is */var/audit/com.day.cq.replication/content*. For **Move** tasks, the default location is */var/audit/com.day.cq.wcm.core.page*.
