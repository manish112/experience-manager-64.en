---
title: Extending Asset Editor
seo-title: Extending Asset Editor
description: Learn how to extend the capabilities of Asset Editor using custom components.
seo-description: Learn how to extend the capabilities of Asset Editor using custom components.
uuid: 27402f84-f34d-4066-a906-95abf162c997
contentOwner: Guillaume Carlino
products: SG_EXPERIENCEMANAGER/6.4/ASSETS
topic-tags: extending-assets
content-type: reference
discoiquuid: ed0bbfc5-2400-4388-94e1-6dd02252033d
index: y
internal: n
snippet: y
---

# Extending Asset Editor{#extending-asset-editor}

The Asset Editor is the page that opens when an asset found through the Asset Share is clicked allowing the user to edit such aspects of the asset as metadata, thumbnail, title and tags.

Configuration of the editor using the predefined editing components is covered in [Creating and Configuring an Asset Editor Page](../../assets/using/assets-finder-editor.md#main-pars-title-1).

In addition to using pre-existing editor components, Adobe Experience Manager (AEM) developers can also create their own components.

### Creating an Asset Editor Template {#creating-an-asset-editor-template}

The following sample pages are included in geometrixx:

* Geometrixx Sample Page: **/content/geometrixx/en/press/asseteditor.html**
* Sample Template: **/apps/geometrixx/templates/asseteditor**
* Sample Page Component:** /apps/geometrixx/components/asseteditor**

#### Configuring Clientlib {#configuring-clientlib}

AEM Assets components use an extension of the WCM edit clientlib. The clientlibs are usually loaded in **init.jsp**.

Compared to the default clientlib loading (in core's **init.jsp**), an AEM Assets template must have the following:

* The template must include the **cq.dam.edit** clientlib (instead of **cq.wcm.edit**).

* The clientlib must also be included in disabled WCM mode (for example, loaded on **publish**) to be able to render the predicates, actions, and lenses.

In most cases, copying the existing sample **init.jsp** (**/apps/geometrixx/components/asseteditor/init.jsp**) should meet these needs.

#### Configuring JS actions {#configuring-js-actions}

Some of the AEM Assets components require JS functions defined in **component.js**. Copy this file to your component directory and link it.

```xml
<script type="text/javascript" src="<%= component.getPath() %>/component.js"></script>

```

The sample loads this javascript source in **head.jsp **(**/apps/geometrixx/components/asseteditor/head.jsp**).

#### Additional Style Sheets {#additional-style-sheets}

Some of the AEM Assets components use the AEM widgets library. To be rendered properly in the content context, an additional style sheet has to be loaded. The tag action component requires one more.

```xml
<link href="/etc/designs/geometrixx/ui.widgets.css" rel="stylesheet" type="text/css">
```

#### Geometrixx Style Sheet {#geometrixx-style-sheet}

The sample page components require that all selectors start with **.asseteditor** of **static.css** (**/etc/designs/geometrixx/static.css**). Best practice: Copy all **.asseteditor** selectors to your style sheet and adjust the rules as desired.

#### FormChooser: Adjustments for eventually loaded Resources {#formchooser-adjustments-for-eventually-loaded-resources}

The Asset Editor uses the Form Chooser, which allows you to edit resources - in this case assets - on the same form page by simply adding a form selector and the path of the form to the URL of the asset.

For example:

* Plain form page: [http://localhost:4502/content/geometrixx/en/press/asseteditor.html](http://localhost:4502/content/geometrixx/en/press/asseteditor.html)
* Asset loaded into the form page: [http://localhost:4502/content/dam/geometrixx/icons/diamond.png.form.html/content/geometrixx/en/press/asseteditor.html](http://localhost:4502/content/dam/geometrixx/icons/diamond.png.form.html/content/geometrixx/en/press/asseteditor.html)

The sample handles in **head.jsp** (**/apps/geometrixx/components/asseteditor/head.jsp**) do the following:

* They detect if an asset is loaded or if the plain form has to be displayed.
* If an asset is loaded, they disable WCM mode as parsys can only be edited on a plain form page.   
* If an asset is loaded, they use its title instead of the one on the form page.

```java
 List<Resource> resources = FormsHelper.getFormEditResources(slingRequest);
    if (resources != null) {
        if (resources.size() == 1) {
            // single resource
            FormsHelper.setFormLoadResource(slingRequest, resources.get(0));
        } else if (resources.size() > 1) {
            // multiple resources
            // not supported by CQ 5.3
        }
    }
    Resource loadResource = (Resource) request.getAttribute("cq.form.loadresource");
    String title;
    if (loadResource != null) {
        // an asset is loaded: disable WCM
        WCMMode.DISABLED.toRequest(request);

        String path = loadResource.getPath();
        Asset asset = loadResource.adaptTo(Asset.class);
        try {
            // it might happen that the adobe xmp lib creates an array
            Object titleObj = asset.getMetadata("dc:title");
            if (titleObj instanceof Object[]) {
                Object[] titleArray = (Object[]) titleObj;
                title = (titleArray.length > 0) ? titleArray[0].toString() : "";
            } else {
                title = titleObj.toString();
            }
        }
        catch (NullPointerException e) {
            title = path.substring(path.lastIndexOf("/") + 1);
        }
    }
    else {
        title = currentPage.getTitle() == null ? currentPage.getName() : currentPage.getTitle();
    }

```

In the HTML part, use the preceding title set (either asset or page title):

```xml
<title><%= title %></title>

```

### Creating a simple form field component {#creating-a-simple-form-field-component}

This example describes how to build a component that shows and displays the metadata of a loaded asset.

1. Create a component folder in your projects directory, for example, **/apps/geometrixx/components/samplemeta**.
1. Add **content.xml**:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image Dimension"
       sling:resourceSuperType="foundation/components/parbase"
       allowedParents="[*/parsys]"
       componentGroup="Asset Editor"/>
   
   ```

1. Add **samplemeta.jsp**:

   ```xml
   <%--
   
     Sample metadata field comopnent
   
   --%><%@ page import="com.day.cq.dam.api.Asset,
                    java.security.AccessControlException" %><%
   %><%@include file="/libs/foundation/global.jsp"%><%
   
       String value = "";
       String name = "dam:sampleMetadata";
       boolean readOnly = false;
   
       // If the form page is requested for an asset loadResource will be the asset.
       Resource loadResource = (Resource) request.getAttribute("cq.form.loadresource");
   
       if (loadResource != null) {
   
           // Determine if the loaded asset is read only.
           Session session = slingRequest.getResourceResolver().adaptTo(Session.class);
           try {
               session.checkPermission(loadResource.getPath(), "set_property");
               readOnly = false;
           }
           catch (AccessControlException ace) {
               // checkPermission throws exception if asset is read only
               readOnly = true;
           }
           catch (RepositoryException re) {}
   
           // Get the value of the metadata.
           Asset asset = loadResource.adaptTo(Asset.class);
           try {
               value = asset.getMetadata(name).toString();
           }
           catch (NullPointerException npe) {
               // no metadata dc:description available
           }
       }
   %>
   <div class="form_row">
       <div class="form_leftcol">
           <div class="form_leftcollabel">Sample Metadata</div>
       </div>
       <div class="form_rightcol">
           <%
           if (readOnly) {
               %><c:out value="<%= value %>"/><%
           }
           else {
               %><input class="text" type="text" name="./jcr:content/metadata/<%= name %>" value="<c:out value="<%= value %>" />"><%
           }%>
       </div>
   </div>
   
   ```

1. To make the component available, you need to be able to edit it. To make a component editable, in CRXDE Lite, add a node **cq:editConfig** of primary type **cq:EditConfig**. So that you can remove paragraphs, add a multi-value property **cq:actions** with a single value of **DELETE**.  

1. Navigate to your browser, and on your sample page (for example, **asseteditor.html**) switch to design mode and enable your new component for the paragraph system.  

1. In **Edit** mode, the new component (for example, **Sample Metadata**) is now available in the sidekick (found in the **Asset Editor **group). Insert the component. To be able to store the metadata, it must be added to the metadata form.

### Modifying Metadata Options {#modifying-metadata-options}

You can modify the namespaces available in the [metadata form](../../assets/using/assets-finder-editor.md#main-pars-title-14).

Currently available metadata are defined in **/libs/dam/options/metadata**:

* The first level inside this directory contains the namespaces.
* The items inside each namespace represents a metadata, such as results in a local part item.
* The metadata content contains the information for the type and the multi-value options.

The options can be overwritten in **/apps/dam/options/metadata**:

1. Copy the directory from **/libs **to **/apps**.

1. Remove, modify, or add items.

>[!NOTE]
>
>If you add new namespaces, they must be registered in your repository/CRX. Otherwise submitting the metadata form will result in an error.
