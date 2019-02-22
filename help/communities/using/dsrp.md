---
title: DSRP - Relational Database Storage Resource Provider
seo-title: DSRP - Relational Database Storage Resource Provider
description: Set up AEM Communities to use a relational database as its common store
seo-description: Set up AEM Communities to use a relational database as its common store
uuid: 6704b67e-9e34-4d6c-99a7-e1f65d4f59d5
contentOwner: Janice Kendall
products: SG_EXPERIENCEMANAGER/6.4/COMMUNITIES
topic-tags: administering
content-type: reference
discoiquuid: 5b6edbad-de5c-4128-aa4d-c741e6cfcad5
index: y
internal: n
snippet: y
---

# DSRP - Relational Database Storage Resource Provider{#dsrp-relational-database-storage-resource-provider}

## About DSRP {#about-dsrp}

When AEM Communities is configured to use a relational database as its common store, user generated content (UGC) is accessible from all author and publish instances without the need for synchronization nor replication.

See also [Characteristics of SRP Options](../../communities/using/working-with-srp.md#characteristicsofsrpoptions) and [Recommended Topologies](../../communities/using/topologies.md).

## Requirements {#requirements}

* [MySQL](#mysqlconfiguration), a relational database
* [Apache Solr](#solrconfiguration), a search platform

## Relational Database Configuration {#relational-database-configuration}

### MySQL Configuration {#mysql-configuration}

A MySQL installation may be shared between enablement features and common store (DSRP) within the same connections pool by using different database (schema) names and also different connections (server:port).

For installation and configuration details, see [MySQL Configuration for DSRP](../../communities/using/dsrp-mysql.md).

### Solr Configuration {#solr-configuration}

A Solr installation may be shared between the node store (Oak) and common store (SRP) by using different collections.

If both the Oak and SRP collections are used intensively, a second Solr may be installed for performance reasons.

For production environments, SolrCloud mode provides improved performance over standalone mode (a single, local Solr setup).

For installation and configuration details, see [Solr Configuration for SRP](../../communities/using/solr.md).

### Select DSRP {#select-dsrp}

The [Storage Configuration console](../../communities/using/srp-config.md) allows for the selection of the default storage configuration, which identifies which implementation of SRP to use.

On author, to access the Storage Configuration console

* sign in with administrator privileges
* from the **main menu**

    * select **Tools** (from the left hand pane)
    * select **Communities**
    * select **Storage Configuration**

        * as an example, the resulting location is: [http://localhost:4502/communities/admin/defaultsrp](http://localhost:4502/communities/admin/defaultsrp)

![](assets/chlimage_1-128.png)

* select **Database Storage Resource Provider (DSRP)**
* **Database Configuration**

    * **JDBC datasource name** 
      name given to MySQL connection  
      must be the same as entered in [JDBC OSGi configuration](../../communities/using/dsrp-mysql.md#configurejdbcconnections)  
      *default* : communities
    
    * **Database name** 
      name given to schema in [init_schema.sql](../../communities/using/dsrp-mysql.md#obtainthesqlscript) script  
      *default* : communities

* **SolrConfiguration**

    * ** [Zookeeper](https://cwiki.apache.org/confluence/display/solr/Using+ZooKeeper+to+Manage+Configuration+Files) Host** 
      Leave this value blank if running Solr using the internal ZooKeeper. Else, when running in [SolrCloud mode](../../communities/using/solr.md#solrcloudmode) with an external ZooKeeper, set this value to the URI for the ZooKeeper, such as *my.server.com:80* 
      *default* : * &lt;blank&gt;*
    
    * **Solr URL** 
      *default* : http://127.0.0.1:8983/solr/
    
    * **Solr Collection** 
      *default* : collection1

* select **Submit**

## Publishing the Configuration {#publishing-the-configuration}

DSRP must be identified as the common store on all author and publish instances.

To make the identical configuration available in the publish environment :

* on author :

    * navigate from main menu to `Tools > Operations > Replication`
    * double-click **Activate Tree**
    * **Start Path :**

        * browse to `/etc/socialconfig/srpc/`

    * ensure *Only Modified* is not selected.
    * select **Activate**

## Managing User Data {#managing-user-data}

For information regarding *users*, *user profiles* and *user groups*, often entered in the publish environment, visit

* [User Synchronization](../../communities/using/sync.md)
* [Managing Users and User Groups](../../communities/using/users.md)

## Reindexing Solr for DSRP {#reindexing-solr-for-dsrp}

To reindex DSRP Solr, follow the documentation for [reindexing MSRP](../../communities/using/msrp.md#msrpreindextool), however when reindexing for DSRP, use this URL instead: **/services/social/datastore/rdb/reindex**

For example, a curl command to re-index DSRP would look like this:

```shell
curl -u admin:password -X POST -F path=/ http://host:port/services/social/datastore/rdb/reindex
```
