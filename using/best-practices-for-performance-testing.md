---
title: Best Practices for Performance Testing
seo-title: Best Practices for Performance Testing
description: null
seo-description: This article outlines the overall strategies and methodologies used for performance testing as well as some of the tools that are available to assist in the process.
uuid: 6a8c5d52-ee93-4d94-9b42-2ad94e45f877
content-encoding: UTF-8
aemsrcnodepath: /content/help/en/experience-manager/6-4/sites/deploying/using/best-practices-for-performance-testing
contentOwner: User
cq-designpath: /etc/designs/help
cq-lastmodified: 2018-04-30T03 26 39.981-0400
cq-lastmodifiedby: msm-service
cq-lastreplicated: 2018-04-03T08 39 42.113-0400
cq-lastreplicatedby: carlino
cq-lastreplicationaction: Activate
products: SG_EXPERIENCEMANAGER/6.4/SITES
content-type: reference
topic-tags: best-practices
cq-template: /apps/help/templates/article-3
discoiquuid: 446a7149-f74d-4f22-bdc8-1ca321e77d88
firstPublishExternalDate: 2017-10-31T16:18:14.801-0400
isreadyforlocalization: false
jcr-created: 2018-01-19T19 04 23.864-0500
jcr-createdby: admin
jcr-ischeckedout: true
jcr-language: en_us
lastPublishExternalDate: 2018-04-03T08:39:41.188-0400
lochandoffdate: 2018-04-30T03 26 39.981-0400
lr-lastreplicatedby: carlino@adobe.com
moreHelpPaths: /content/help/en/experience-manager/6-4/sites/deploying/morehelp/best-practices;/content/help/en/experience-manager/6-4/sites/deploying/morehelp/best-practices
mwpw-migration-script-version: 2017-10-12T21 46 58.665-0400
navTitle: Best Practices for Performance Testing
publishexternaldate: 2018-04-03T08 39 41.188-0400
publishExternalURL: https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/best-practices-for-performance-testing.html
qaDate: 2017-10-12T21:46:00.000-0400
qaNotes: /content/docs/en/aem/6-3/deploy/best-practices/best-practices-for-performance-testing
sha1: 1153cff8fe8a3a49782b4b395edb294c7f973066
topicBrowsingSortDate: 2018-04-03T08:39:41.188-0400
index: y
internal: n
snippet: y
---

# Best Practices for Performance Testing



## Introduction
Performance testing is an important part of any AEM deployment. Depending on customer requirements, performance testing may be performed on the publish instances, author instances, or both.

This documentation will outline overall strategies and methodologies of performing performance tests as well as some of the tools that are made available by Adobe to assist in the process. Finally, we will analyze some of the tools that are available in AEM 6 to assist with performance tuning, both from a code analysis and system configuration perspective.

### Simulating Reality
What is most important when performing performance tests is making sure that you mimic a production environment as closely as possible. While this can often be difficult, it is imperative to ensure the accuracy of these tests. When working on designing performance tests, it is important to take the following points into consideration:

* Production-like content

Many performance measurements in AEM, such as query response time, can be impacted by the size of the content on the system. It is important to make sure that the test environment has as close of a copy of the production data as possible.

* Production code

The AEM version and hotfixes deployed in production should be the same in the test environment. It is also important to test on the version of the code that is deployed to production.

* Production-like hardware and network configuration

The tests will be meaningless without an environment that resembles the production one as closely as possible. Ideally, the hardware specifications, network interfaces, load balancers and CDN should be identical to production in the test environment.

* Production load

Many performance issues are not seen until the system is under heavy load. Good performance tests should simulate the load that the production systems will be under at their peak.

### Setting Goals
Before starting on performance testing, it is necessary to set non functional requirements to specify load and response times. If you are migrating from an existing system, make sure that response time are similar to your current production values. For load, it is best to take the current peak load and double it. This will ensure that the website can continue to perform well as it grows.

### Tools
There are many commercially available performance testing tools on the market. When running a load generating tool, it is important to ensure that the computers which are performing the tests have sufficient network bandwidth. Otherwise, once the test machine reaches the limits of its connection, no additional load will be generated on the environment under test.

#### Testing Tools

* Adobe’s **Tough Day** tool can be used to generate load on AEM instances and collect performance data. Adobe’s AEM engineering team actually uses the tool to do load testing of the AEM product itself. The scripts executed in Tough Day are configured via property files and JMX XML files. For more information, see the [Tough Day documentation](/content/help/en/experience-manager/6-4/sites/developing/using/tough-day).

* AEM provides out of the box tools to quickly see problematic queries, requests and error messages. For more information, see the [Diagnosis Tools](/content/help/en/experience-manager/6-4/sites/administering/using/operations-dashboard#main-pars_title_8) section of the Operations Dashboard documentation.
* Apache provides a product called **JMeter** that can be used for performance and load testing as well as functional behavior. It is open source software and free to use, but has a smaller feature set than enterprise products and a steeper learning curve. JMeter can be found on Apache’s website at [http://jmeter.apache.org/](http://jmeter.apache.org/)

* **Load Runner** is an enterprise grade load testing product produced by HP. A free evaluation version is provided. More information can be found on HP’s website at [http://www8.hp.com/us/en/software-solutions/loadrunner-load-testing/](http://www8.hp.com/us/en/software-solutions/loadrunner-load-testing/)

* Cloud based load testing tools like [Neustar](https://www.neustar.biz/services/web-performance/load-testing) can also be used.
* When it comes to testing mobile or responsive websites, a separate set of tools need to be used. They work by throttling network bandwidth, simulating slower mobile connections like 3G or EDGE. Among the more widely used tools are:

    * ** [Network Link Conditioner](http://nshipster.com/network-link-conditioner/)** - it provides an easy to use UI and works at a fairly low level on the networking stack. It includes versions for OS X and iOS; [](http://nshipster.com/network-link-conditioner/)
    
    * [**Charles**](http://www.charlesproxy.com/) - a web debugging proxy application that in addition to several other uses, provides network throttling. Versions are provided for Windows, OS X and Linux. [](http://www.charlesproxy.com/)

#### Optimization Tools
**Monitoring**

The [Monitoring Performance](monitoring-and-maintaining.md#main-pars_title_3) documentation is a good resource for tools and methods that can be used to diagnose issue and pinpoint areas for tuning.

**Developer Mode in Touch UI**

One of the new features in AEM 6’s touch UI is the Developer Mode. Just as authors can switch between edit and preview modes, developers can switch to developer mode in the author UI to see the render time for each of the components on the page and to see stack traces of any errors. For more information on developer mode, see this [CQ Gems presentation](http://docs.adobe.com/content/ddc/en/gems/aem-6-0-developer-mode.html).

**Using the rlog.jar to read the request logs**

For a more comprehensive analysis of the request logs on an AEM system, `rlog.jar` can be used to search through and sort the `request.log` files that AEM generates. This jar file is included with an AEM installation in the `/crx-quickstart/opt/helpers` folder. For more information on rlog tool and the request log in general, see the [Monitoring and Maintaining](monitoring-and-maintaining.md) documentation.

**The Explain Query Tool**

The [Explain Query tool](/content/help/en/experience-manager/6-4/sites/administering/using/operations-dashboard#main-pars_title_1097830066) in ACS AEM Tools can be used to view the indexes that are used when running a query. This can be very useful when optimizing slow running queries.

**PageSpeed Tools  
**

Google’s PageSpeed tools offer site analysis for adherence to best practices for page performance as well as a plugin that can be installed alongside the dispatcher on an Apache instance for additional optimizations. For more information, see the [PageSpeed Tools Website](https://developers.google.com/speed/pagespeed/).

## Author Environment

### Performing Tests
In order to conduct performance testing on the author environment it is necessary that you simulate the experience of porduction authors. This means that the author installations must contain all the components, OSGi bundles, UI customization, custom indexes and any other additions you have in place for the production author instances.

There are many automation frameworks available that are designed for performance and load testing. Custom scripts can be recorded in these tools and then played back to simulate a peak number of authors performing similar content creation and activation activities simultaneously. It is recommended you use the Tough Day tool to simulate activities like uploading thousands of assets or activating large numbers of pages.

For the types of environments that have requirements of heavy asset loading or page authoring it is imperative to use tools like Tough Day in order to ensure that the environment will operate efficiently under peak load. [WebDAV](/content/help/en/experience-manager/6-4/sites/administering/using/webdav-access) is a tool that does not require scripting and can also be used to load large amounts of assets.

#### MongoDB Specific Steps
On systems with MongoDB backends, AEM provides several [JMX](/content/help/en/experience-manager/6-4/sites/administering/using/jmx-console) MBeans that need to be monitored when performing load or performance tests:

* The **Consolidated Cache Statistics** MBean. It can be accessed directly by going to *http://server:port/system/console/jmx/org.apache.jackrabbit.oak%3Aid%3D6%2Cname%3D%22Consolidated+Cache+statistics%22%2Ctype%3D%22ConsolidatedCacheStats%22*

For the cache named **Document-Diff**, the hit rate should be over `.90`. If the hit rate falls below 90%, it is likely that you will need to modify your `DocumentNodeStoreService` configuration. Adobe product support can recommend optimal settings for your environment.

* The **Oak Repository Statistics** Mbean. It can be accessed directly by going to *http://server:port/system/console/jmx/org.apache.jackrabbit.oak%3Aid%3D16%2Cname%3D%22Oak+Repository+Statistics%22%2Ctype%3D%22RepositoryStats%22*

The **ObservationQueueMaxLength** section will show the number of events in Oak’s observation queue over the last hours, minutes, seconds and weeks. Find the largest number of events in the "per hour" section. This number needs to be compared to the `oak.observation.queue-length` setting which can be found in the **SlingRepositoryManager** component in the [OSGi console](configuring-web-console.md). If the highest number shown for the observation queue exceeds the `queue-length` setting, contact Adobe Support for assistance with raising the setting. The default setting is 1,000, but most deployments usually need to raise it to 20,000 or 50,000.

## Publish Environment

### Performing Tests
The most important part of a deployment that needs to be subjected to load tests is the end user facing publish or dispatcher environment.

Third party automated testing tools can be used to test the performance of the website. These tools will allow you to record steps that users will go through on the site and run many of these sessions at the same time to simulate the load that is typical of a production website.

Most production websites have optimizations in place like dispatcher caching and a content delivery network in place. When testing, you need to make sure that these optimizations are available for the test environment as well. In addition to monitoring response times for the end users, you also need to monitor system metrics on the publish servers and dispatchers to ensure that the system is not constrained by hardware resources.

On a system that does not require a high level of personalization, the dispatcher should cache most requests. As a result, the load on the publish instance should remain relatively flat. If a high level of personalization is required, it is recommended to use technologies such as iFrames or AJAX requests for the personalized content so as to allow as much dispatcher caching as possible.

For basic tests, Apache Bench can be used to measure web server response times and help to create load for measuring things like memory leaks. For more see the example in the [Monitoring documentation](monitoring-and-maintaining.md#main-pars_title_7-ztphmx-refd).

## Troubleshooting Performance Issues
After running performance tests on the author instance, any issues will need to be investigated, diagnosed and addressed. You can use several tools and techniques when performing analysis and addressing issues:

* You can inspect the [Request Performance log](/content/help/en/experience-manager/6-4/sites/administering/using/operations-dashboard#main-pars_title_10) in the Operations Dashboard. This tool can be used to identify slow page requests
* Analyze slow running queries with the [Query Performance tool](/content/help/en/experience-manager/6-4/sites/administering/using/operations-dashboard#main-pars_title_11)  

* Watch the error lof for errors or warnings. For more information, see [Logging](configure-logging.md)
* Monitor system hardware resources such as memory and CPU utilization, disk I/O or network I/O. These resources are often the causes of performance bottlenecks
* Optimize the architecture of the pages and how they are addressed to minimize the usage of URL parameters to allow for as much caching as possible
* Follow the [Performance Optimization](configuring-performance.md) and [Performance tuning tips](/content/help/en/experience-manager/kb/performance-tuning-tips) documentation

* If issues are present with editing certain pages or components on author instances, use the TouchUI Developer Mode to inspect the page in question. This will provide a breakdown of each content area on the page as well as its load time
* Minify all JS and CSS on the site. For more information on how to do this, see this [blog post](https://blogs.adobe.com/foxes/enable-js-and-css-minification/).
* Eliminate embedded CSS and JS from the components. They should be included and minified with the client-side libraries to minimize the number of requests required to render the page
* Use browser tools like Chrome’s Network tab to inspect the server requests and see which are taking the longest.

Once problem areas are identified, application code can be inspected for performance optimizations. Any out of the box AEM features that are not performing properly can be addressed with Adobe Support.