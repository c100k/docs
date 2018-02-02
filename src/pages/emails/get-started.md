## Overview

Deep Linked Email allows you to automatically convert your email links into multi-platform deep links that take users directly to content in the app on mobile devices, while still maintaining the same web experience for desktop and mobile users without the app.

![image](/img/pages/email/responsys/deep-linked-email.png)

With a script provided by Branch, you can dynamically create Branch links in email. In any place the script is called, the web URL is converted into its corresponding Branch link. The email is then sent.

When a link is clicked by a user without the app, it will route that user to the original web URL (including on desktop). When a link is clicked by a user with your app, it will direct that user into the relevant in-app content regardless of platform or email client.

## Guide

### Prerequisites

- This guide requires you to have already [integrated the Branch SDK](/pages/apps/ios/) into your app.

Contact your Branch Account Manager or [accounts@branch.io](mailto:accounts@branch.io) at any time for assistance with the setup steps.

### Choose your email service provider

Navigate to the [Deep Linked Email](https://dashboard.branch.io/email){:target="\_blank"} section of the Branch dashboard. Select your email service provider and click **Get Started**.

![image](/img/pages/email/choose-esp.png)

### Set up deep linking for email

#### Email link behavior

Branch turns the web URLs you put into your emails into Branch deep links, opening the app for users with your app installed to that same content in the app.

![image](/img/pages/email/users-with-app.png)

To do this, it must be possible to map your web URL content (e.g. a page with brown loafers at `https://shop.com/shoes/brown-loafers`) into a working deep link that takes users to brown loafers in the app. The Deep Linked Email [setup flow](https://dashboard.branch.io/email){:target="\_blank"} will attempt to automatically detect this mapping for you.

If you do not want to set this up yet, you can select **No, just open to app homepage for now**.

![image](/img/pages/email/app-homepage.png)

By default, email deep links will redirect users _without your app_ to the same content on the web instead.

![image](/img/pages/email/users-without-app.png)

If you would like to send users to the App Store or another default you have specified in Link Settings, you can select **Open to default redirects**.

![image](/img/pages/email/default-redirects.png)

#### Checking your deep linking setup

If you chose **not** to set up deep linking to specific content within your app, then you can [skip this step](#next-steps).

![image](/img/pages/email/responsys/enter-web-url.png)

In this step, you will want to enter a web URL that corresponds to a specific screen within your app. In other words, the webpage should have content that also exists in your app. If you do not know whether your web content also exists in-app, try any URL other than your website homepage. Some examples:

- A product page, like a page with brown loafers
- An article
- A content page, like a video or image

Once you choose one and click **Submit**, [meta tags that can be used for deep linking](/pages/web/hosted-data/) will be retrieved from your webpage. You will see a result indicating the mapping between your web content and your app content. For a full explanation of the tests run on this page and the possible results, check out the [support section](/pages/emails/support/#deep-linking-setup-messages).

#### We couldn't determine your deep linking setup

If an app deep linking scheme that maps to your web content cannot be successfully detected, you can [configure your settings manually](#deep-linking-settings-for-email), or you can reach out to your Branch account manager or support for assistance.

![image](/img/pages/email/failure-result.png)

We will help you set up one of the following methods:

If you use unique key/value data as deep link values:

1. _Recommended:_ **Hosted deep link data:** You can host your deep link data on your website with a metatag that looks like this `<meta name="branch:deeplink:my_key" content="my_value" />` where `my_key` and `my_value` will become a key value pair in deep link data. For each web URL, Branch will look for those tags and embed the deep link data (if found) into the deep link. Note that Branch also accepts App Links tags for deep linking. For more details, please read [Hosted Deep Link Data](/getting-started/hosted-deep-link-data/guide/).
1. **As query parameters:** Simply append query parameters on to your web url and Branch will take those parameters and put them in deep link data.

If you use your web URL as a deep link value:

1. **URL path:** If you use the path of your web URL as your  `$deeplink_path` value, or any other deep link value, then the configuration will automatically take the path of the URL and put it in deep link data.
1. **Full URL:** If you use the full web URL as your `$deeplink_path` value, or any other deep link value, then the configuration will take the entire URL and put it in deep link data.

!!! protip "Host deep link data for more than just emails"
    The Branch [Quick Link creator](/getting-started/creating-links/dashboard/) also scrapes your web URL for deep link data to make link creation even easier. [Hosting Deep Link Data](/getting-started/hosted-deep-link-data/guide/) on your website will make using Branch products easier in future.


In the meantime, you can proceed to the next step: **[Configure ESP](#configure-your-esp)**.

#### Deep linking settings for email

The following are all the possible settings you can configure for deep linking with email.

##### Link Behavior

| Setting | Example | Link Data Result
| --- | --- | ---
| **Open the app homepage** | No settings configured to generate deep link data for email; email links will route to the app homepage. | 
| **Open to specific app content** | Deep link to specific app content based on one or more of the following settings. | 
| Translate query parameters on URLs into Branch link data | **URL:** `https://shop.com/shoes/brown-loafers&product_id=123456` | `product_id: 123456`
| Translate web URL into Branch link data: <br> Full URL for key ______ | **URL:** `https://shop.com/shoes/brown-loafers` <br> **Key:** `$canonical_url` | `$canonical_url: https://shop.com/shoes/brown-loafers`
| Translate web URL into Branch link data: <br> URL path for key ______ | **URL:** `https://shop.com/shoes/brown-loafers` <br> **Key:** `$deeplink_path` | `$deeplink_path: shoes/brown-loafers`
| Retrieve hosted deep link data from website and translate into Branch link data | **URL:** `https://shop.com/shoes/brown-loafers` <br> **Meta Tags:** `<meta name="branch:deeplink:product_id" content="123456" />` | `product_id: 123456`
| Strip protocol (http:// or https://): <br> from $deeplink_path <br> from $ios_deeplink_path <br> from $android_deeplink_path <br> *Note: Typically used with one of the other settings.* | **URL:** `https://shop.com/shoes/brown-loafers` <br> **Other Settings:** Translate web URL into Branch link data: Full URL for key `$deeplink_path` | `$deeplink_path: shop.com/shoes/brown-loafers`
| Translate query parameters on URLs into Branch link data from parameter ______ to key ______ <br> *Note: Not configurable in the UI. [Contact support](https://support.branch.io/support/tickets/new){:target="_blank"} to use this setting.* | **URL:** `https://shop.com/shoes/brown-loafers&product_id=123456&utm_content=shoes` <br> **Parameter:** `utm_content` <br> **Key:** `category` | `category: shoes`

##### Link behavior for users without your app

| Setting | Description
| --- | ---
| Open to specific web content | Route to the original URL specified in the email.
| Open to default redirects | Route to defaults specified in [Link Settings](https://dashboard.branch.io/link-settings){:target="_blank"}.

### Visualizing Email Performance

In order to view the performance of your Deep Linked Emails, navigate to the Analytics tab of the Email section of the dashboard. 

The Analytics tab provides two charts: a time series and a table. Both charts are populated exclusively with data from your Branch Deep Linked Email campaigns, and you can filter further using the options at the top of each chart. The charts display counts and rates of events attributed to Deep Linked Email link clicks.

Both the time series and the table automatically compare by platform, so you can see the results of your campaigns across all platforms (iOS app & web, Android app & web, desktop web).

!!! protip "Email Analytics filtering"
    These charts are pre-filtered, so it will not be possible to view data associated with non-email Branch links in the Email Analytics section of the dashboard. This filtering does not make use of standard Branch analytics labels (e.g. channel, campaign, feature), so it may be difficult to recreate these reports on other dashboard analytics pages.

## Next steps

Completing your integration requires some setup that is specific to your email service provider. Please follow the documentation for your provider:

- **[Amazon SES](/pages/emails/amazon-ses/)**
- **[Betaout](/pages/emails/betaout/)**
- **[Blueshift](pages/emails/blueshift/)**
- **[Braze (formerly Appboy)](/pages/emails/braze/)**
- **[Bronto](/pages/emails/bronto/)**
- **[Campaign Monitor](/pages/emails/campaign-monitor/)**
- **[Epsilon](/pages/emails/epsilon/)**
- **[Kahuna](/pages/emails/kahuna/)**
- **[Leanplum](/pages/emails/leanplum/)**
- **[Mailgun](/pages/emails/mailgun/)**
- **[Mailjet](/pages/emails/mailjet/)**
- **[Mandrill](/pages/emails/mandrill/)**
- **[PostUp](/pages/emails/postup/)**
- **[Responsys](/pages/emails/responsys/)**
- **[Sailthru](/pages/emails/sailthru/)**
- **[Salesforce](/pages/emails/salesforce/)**
- **[SendGrid](/pages/emails/sendgrid/)**
- **[SparkPost](/pages/emails/sparkpost/)**
- **[Vero](/pages/emails/vero/)**
- **[Zeta](/pages/emails/zeta/)**
