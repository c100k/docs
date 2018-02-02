---
---


### Prerequisites

- To use the Branch Link Conversion SDK, you'll need an EMD (Email Message Designer) enabled account. If you're using the Classic dashboard, or if you’re not sure, please talk to your Responsys Account Manager.

{! ingredients/email/email-configure-esp.md !}

You can retrieve your click tracking domain from your Responsys settings. Enter it in item 1 of this step. On **Done** click, an AASA file - required for Universal Links - specific to that domain will be generated.

{! ingredients/email/email-send-aasa.md !}

{! ingredients/email/email-technical-setup.md !}

{! ingredients/email/email-associated-domains.md !}

{! ingredients/email/email-validate-test.md !}

{! ingredients/email/email-usage.md !}

### Options for generating Branch links for email

There are a few different ways you can create Branch links that are compatible with Deep Linked Email + Responsys. You will need to replace the web URLs in your templates with these. To create Branch links, you can either:

1. [Use the Branch Responsys SDK](#use-the-branch-responsys-sdk)
1. [Making regular Branch links compatible with email](#making-regular-branch-links-compatible-with-email)
1. [Create email links via API without changing your email templates](#create-email-links-via-api-without-changing-your-email-templates)
1. [Convert all web links in your email to deep links](#convert-all-web-links-in-your-email-to-deep-links)

**Responsys uses the shortcode `e_rs` for links in emails** - please use this in place of `e_xx` in the guide below.

#### Use the Branch Responsys SDK

In this step, we'll upload an SDK that makes it very easy to create deep links in your emails.  Please remember that this will require an EMD (Email Message Designer) enabled account.

!!! protip "Watch how to do this instead"
    There is also a [tutorial video](https://www.youtube.com/watch?v=u8h8KlqFvo4){:target="\_blank"} that walks through these steps.

1. Work with your Branch account manager to modify the following code snippet, replacing `DOMAIN-HERE` with your Branch base domain:

    ```
    <#macro deeplink link_to_be_wrapped><#assign branch_base_url="https://DOMAIN-HERE/3p?%243p=e_rs"><#assign final_link=branch_base_url + "&%24original_url=" + link_to_be_wrapped?url("ISO-8859-1")><a href="${final_link}"><#nested></a></#macro> <#macro tracked_deeplink link_to_be_wrapped><#assign branch_base_url="https://DOMAIN-HERE/3p?%243p=e_rs"><#assign deeplink=branch_base_url + "&%24original_url=" + link_to_be_wrapped?url("ISO-8859-1")></#macro>
    ```

1. Log in to your Responsys account.
1. In the Responsys Dashboard, open your Content Library. You can also access it via the Shortcuts screen on the main page: 

    ![image](/img/pages/email/responsys/responsys-shortcuts.png)

1. Once you are in the Content Manager, you’ll see a list of folders where content is stored. Under **All Content**, create a new folder named `Branch_SDK`:

    ![image](/img/pages/email/responsys/responsys-new-folder.png)

1. Select the **Branch_SDK** folder and then click **Create Document**:

    ![image](/img/pages/email/responsys/responsys-create-document.png)

1. In the Create Document window:
    * Enter `branch-sdk` in the “Document Name” field.
    * In the **Content Box**, delete all the text.
    * Paste the snippet you copied in **1**.
    * Click Save. 

      ![image](/img/pages/email/responsys/responsys-snippet.png)

You have now successfully created the deep linking script. Your file structure should look as follows:

![image](/img/pages/email/responsys/deep-linked-email-manage-content.png)

##### Configure your Responsys email templates

This code is referred to as the "Branch script" - this script will convert your web URLs to deep links.

The Responsys integration requires you to add email template code in two places.

1. At the top of an email template
2. Immediately before a hyperlink

Copy the following snippet, and using the “Source” view, paste the snippet directly under the `<html>` tag for every template you plan to add deep linking to.

```
<#include "cms://contentlibrary/Branch_SDK/branch-sdk.htm">
```

##### Create deep links

Wherever you are using `<a>` tags in your email templates, replace those with `<@deeplink>` tags, or add `<@tracked_deeplink />` for web URLs that you would like to deep link.

Here's how it looks with Link Tracking disabled.

*Before:*
`<a href="https://branch.io">Example link</a>`

*After:*
`<@deeplink "https://branch.io">Example link</@deeplink>`
{% endexample %}

With link tracking enabled, you can still use Branch links in emails.

*Before:*
`<a href="https://branch.io/product/1234">Example link</a>`

*After:*
`<@tracked_deeplink "https://branch.io/product/1234" />
<a href="${clickthrough('TEST_TRACKED_DEEPLINK' , 'deeplink=' + deeplink)}">Example link</a>`

This latter example pulls from a Link Table.

![image](/img/pages/email/responsys/deep-linked-email-template.png)

{! ingredients/email/email-link-options.md !}

### Handle links for web-only content

In some cases you may have content on web that isn’t in the app - for example, a temporary Mother’s Day promotion or an unsubscribe button. You can designate links to only open on web if you use the Responsys Link Table feature. There are three URL fields in the link table when creating a new link: `LINK_URL`, `IOS_LINK_URL`, and `ANDROID_LINK_URL`. If you only enter the link in the `LINK_URL` field, the path of the final click-wrapped url will begin with `/pub/cc`.  However, if you also input the same link in `IOS_LINK_URL`, then the path of the final click-wrapped url will begin with `pub/acc`. You should set up your AASA file to whitelist only the path `/pub/acc*` in order to not launch the app from web-only links.

![image](/img/pages/email/responsys/branch_responsys_webonly.png)

![image](/img/pages/email/responsys/branch_responsys_deeplink.png)

## Support

### Styling
If you include style tags within your `<a>` tags, you’ll need to separate those out into a separate div inside the `<@deeplink>` tag. If you use tracked links with `<a>` tags, those will work fine.

Style Tags within your anchor tags like so:

**Before:**

```
<a href="https://branch.io/" style="color:#000001; text-decoration:none;">Branch Website</a>
```

**After:**

```
<@deeplink "https://branch.io/"><div style="color:#000001; text-decoration:none;">Branch Website</div></@deeplink>
```

### Launch failed error
You’ll see this error if you haven’t included the `<#import >` snippet in your template.

```obj-c
Launch Failed: Launch failed: Template /contentlibrary/branch test campaign/My Default Template.htm caused an execution error: on line 183, column 92 in cms://contentlibrary/branch test campaign/Content.htm: deeplink is not a user-defined directive. It is a freemarker.template.SimpleScalar
```

### Using dynamic data from profile extension tables

The `<@deeplink >` and `<@tracked_deeplink />` tags even work with dynamic links injected via RPL.
```
<@deeplink "${latestProduct.url}">${latestProduct.name}</@deeplink>
```

{! ingredients/email/email-support.md !}
