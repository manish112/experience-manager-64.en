---
title: Blog Feature
seo-title: Blog Feature
description: Community information in a journaling format
seo-description: Community information in a journaling format
uuid: b68a69e8-e461-4547-9c6e-524314871c70
products: SG_EXPERIENCEMANAGER/6.4/COMMUNITIES
topic-tags: authoring
content-type: reference
discoiquuid: eb18ac06-1056-438d-93d3-61288b514e05
index: y
internal: n
snippet: y
---

# Blog Feature{#blog-feature}

## Introduction {#introduction}

The blog feature for AEM Communities has transformed from an authoring activity to a true community activity that takes place in the publish environment.

The blog feature supports providing community information in a journaling format. Blog entries are made in the publish environment by authorized members (registered, signed-in users).

The blog feature provides :

* Publish-side creation of blog articles and comments
* Rich text editing
* Inline images (with support for drag and drop)
* Embedded social networking content ([oEmbed support](../../communities/using/blog-developer-basics.md#allowingrichmedia))
* Draft mode
* Scheduled publishing
* Compose on-behalf (a [privileged member](../../communities/using/users.md#privilegedmembersgroup) can create content on behalf of a different communty member)
* [In-context and bulk moderation](../../communities/using/moderate-ugc.md) of blog articles and comments

This section of the documentation describes

* adding the blog feature to an AEM site
* configuration settings for blog components

>[!NOTE]
>
>The components `Journal`and `Journal Sidebar` are titled `Blog` and `Blog Sidebar`. 
>
>The blog feature found in AEM 6.0 and earlier releases is now removed. It was based on a template and only allowed authors to create content in the author environment.

## Adding Blog Components to a Page {#adding-blog-components-to-a-page}

If it is desired to add a blog to a page in author mode, use the component browser to locate

* `Communities / Blog`
* `Communities / Blog Sidebar`

and drag them into place on a page where the blog should appear.

For necessary information, visit [Communities Components Basics](../../communities/using/basics.md).

When the [required client-side libraries](../../communities/using/blog-developer-basics.md#essentialsforclientside) are included, this is how the `Blog`component will appear :

![](assets/chlimage_1-147.png)

And how the `Blog Sidebar` will appear :

![](assets/chlimage_1-148.png)

### Configuring Blog {#configuring-blog}

Select the placed `Blog` component to access and select the `Configure` icon which opens the edit dialog.

![](assets/chlimage_1-149.png) ![](assets/blog-settings.png)

#### Settings tab {#settings-tab}

Under the **Settings** tab, specify the basic features of the blog :

* **Allow Attachment Thumbnail  
  **If checked, a thumbnail of the attached image is created.
* **Max Attach Thumbnail Size  
  **Maximum size (in pixels) of the attachment thumbnail image. The default value is 800 x 800.** **

* **Max Thumbnail Size  
  **Maximum size (in pixels) of the thumbnail image for inline image. The default value is 800 x 800.
* **Allow Privileged Members  
  **If checked, only Privileged members are allowed to create content.
* **Allowed Privileged Members** 
  Add the privileged members allowed to create content.

* **Block UGC in Author Edit Mode  
  **Block User Generated Content while editing in Author Mode.
* **Journal Title** 
  The blog title to display on the page.

>[!NOTE]
>
>The Journal Title is used to automatically create URL for the blog.  
>Maximum 50 characters (with 5 characters additional for uniqueness) are used from the journal title you specify here to create URL for the blog.

* **Journal Description** 
  The blog description.

* **Topics Per Page** 
  Defines the number of blog entries/comments shown per page. The default is 10.

* **Moderated** 
  If checked, posting of blog entries and comments must be approved before they will appear on a published site. The default is unchecked.

* **Closed** 
  If checked, the blog is closed to new blog entries and comments. Default is unchecked.

* **Rich Text Editor** 
  If checked, blog entries and comments may be entered with markup. Default is checked.

* **Allow Tagging** 
  If checked, allow members to add tag labels to their post (see **Tag field** tab). Default is unchecked.

* **Allow File Uploads** 
  If checked, allow file attachments to be added to a blog entry or comment. Default is unchecked.

* **Max File Size** 
  Relevant only if `Allow File Uploads` is checked. This field will limit the size (in bytes) of an uploaded file. Default is 104857600 (10 Mb).

* **Allowed File Types** 
  Relevant only if `Allow File Uploads` is checked. A comma separated list of file extensions with the "dot" separater. For example : .jpg, .jpeg, .png, .doc, .docx, .pdf. If any file types are specifed, then those not specified will not be allowed to be uploaded. Default is none specified such that** **all file types are allowed.

* **Max Attach Image File Size** 
  Relevant only if Allow File Uploads is checked. Maximum number of bytes an uploaded image file may have. Default is 2097152** **(2 Mb).

* **Allow Replies** 
  If checked, allow replies to comments posted to the blog entry. Default is unchecked.

* **Allow Users to Delete Comments and Topics** 
  If checked, allow members to delete the comments and blog entries they posted. Default is** **unchecked.

* **Allow Following** 
  If checked, include the following feature for blog articles, which allows members to be [notified](../../communities/using/notifications.md) of new posts. Default is unchecked.

* **Allow Email Subscriptions** 
  If checked, allow members to be notified of new posts by email ([subscription](../../communities/using/subscriptions.md)). Requires `Allow Following` to be checked and [email configured](../../communities/using/email.md). Default is unchecked.

* **Allow Voting** 
  If checked, include the Voting feature with a blog entry. Default is unchecked.

* **Display Badges** 
  If checked, display earned and assigned [badges](../../communities/using/implementing-scoring.md) with a member's blog entry. Default is unchecked.

* **Allow Featured Content** 
  if checked, the idea is able to be identified as [featured content](../../communities/using/featured.md). Default is unchecked.

#### User Moderation tab {#user-moderation-tab}

Under the **User Moderation** tab, specify the moderation settings :

* **Deny Posts** 
  If checked, trusted member moderators will be allowed to deny posts and prevent the post from appearing on the public forum. Default is unchecked.  

* **Close / Reopen Topics** 
  If checked, trusted member moderators may close a topic to further edits and comments, and may also reopen a topic. Default is unchecked.  

* **Flag Posts** 
  If checked, allow members to flag others' topics or comments as inappropriate. Default is unchecked**.** 

* **Flag Reason List** 
  If checked, allow members to choose, from a drop-down list, their reason for flagging a topic or comment as inappropriate. Default is unchecked.  

* **Custom Flag Reason** 
  If checked, allow members to enter their own reason for flagging a topic or comment as inappropriate. Default is unchecked**.** 

* **Moderation Threshold** 
  Enter the number of times a topic or comment has to be flagged by members before moderators are notified. Default is 1 ( one time).

* **Flagging Limit** 
  Enter the number of times a topic or comment has to be flagged before it is hidden from public view. If set to -1, the flagged topic or comment is never hidden from public view. Else, this number must be greater than or equal to the Moderation Threshold. Default is 5.

#### Tag field tab {#tag-field-tab}

Under the **Tag field** tab, specify the which tags may be applied if **Allow Tagging** is check on the **Settings** tab :

* **Allowed Namespaces** 
  Relevant if `Allow Tagging` is checked under the **Settings **tab. The tags which may be applied are limited to those within the namespace categories checked. The list of namespaces includes "Standard Tags" (the default namespace) as well as "Include All Tags". Default is none checked, which means all namespaces are allowed.

* **Suggestion Limit** 
  Enter the number of tags to be displayed as a suggestion to the member posting to the forum. A value of -1 means no limits. Default is 0.

### Configuring Blog Sidebar {#configuring-blog-sidebar}

When you double-click the `Blog Sidebar` component, an edit dialog opens up.

Under the **Journal Sidebar Settings** tab, specify the date format for archives and what type of entries to display in the sidebar :

![](assets/chlimage_1-151.png)

* **Date format** 
  The format used to display for archives of blog entries. The format uses placeholders following the Java convention.

    * yyyy : full year, like '2015'
    * yy : short year, like '15'
    * MMMMM : full month, like June
    * MMM : short month, like Jun
    * MM : month number, like 06

  Default is "yyyy MMMMM" which would display, for example, "2015 June"

* **View Type** 
  The Title and type of blog entries to display in the sidebar. The choice is between

    * Authors
    * Categories
    * Archives

* **Journal Component Path** 
  *(Optional)* The location of the blog resource from which blog articles are to be listed. If left blank, will use the component of resourceType `social/journal/components/hbs/journal` that appears on the same page.

    * for example, `/content/sites/engage/en/blog/jcr:content/content/primary/blog`

* **Suggestion Limit** 
  The number of blog articles to be displayed. A value of -1 means no limit. Default is -1.

## Site Visitor Experience {#site-visitor-experience}

In the publish environment, the blog feature will display the most recent blog article followed by older blog articles in descending order of creation. Blog sidebars allow site visitors to apply filters to limit the selection of blog articles shown.

The blog article is followed by a link to post or view comments.

When a blog article is selected, the blog article and comments are displayed (if enabled).

Other abilities depend on whether the site visitor is a moderator, administrator, community member, privileged member or anonymous.

#### Working with Articles {#working-with-articles}

When creating a new blog article, there is the choice to

1. Publish Immediately
1. Publish a Draft
1. Publish at a Scheduled date and time

The blog articles will appear under the appropriate tab (Published, Drafts or Scheduled) to members able to author on publish.

#### Moderators and Administrators {#moderators-and-administrators}

When the signed in user has moderator or administrator privileges, they are able to perform [moderation tasks](../../communities/using/moderate-ugc.md) (as permitted by the configuration of the component) on all blog articles and comments posted to a blog.

![](assets/chlimage_1-152.png)

#### Members {#members}

When the signed in user is a community member or [privileged member](../../communities/using/users.md#privilegedmembersgroup) (depending on configuration), they are able to select `New Article` to create and post a new blog article.

Specifically, they may

* create a new blog article
* post a new blog article on behalf of another member
* post a comment to a blog article
* edit their own blog article or comment
* delete their own blog article or comment
* flag others' blog articles or comments

![](assets/chlimage_1-153.png) ![](assets/chlimage_1-154.png)

#### Anonymous {#anonymous}

Site visitors who are not signed in may only read posted blog articles and comments, translate them if supported, but may not add a blog article or comment nor flag others' articles or comments.

![](assets/chlimage_1-155.png)

## Additional Information {#additional-information}

More information may be found on the [Blog Essentials](../../communities/using/blog-developer-basics.md) page for developers.

For moderation of blog entries and comments, see [Moderating User Generated Content](../../communities/using/moderate-ugc.md).

For tagging blog entries and comments, see [Tagging User Generated Content](../../communities/using/tag-ugc.md).

For translation of blog entries and comments, see [Translating User Generated Content](../../communities/using/translate-ugc.md).