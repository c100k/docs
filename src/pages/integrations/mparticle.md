## Overview

With a push of a button you can send your Branch data to your mParticle dashboard, helping you to understand the power of Branch as an acquisition pathway, export your data to your entire suite of analytics tools and build custom audiences upon Branch data.

This documentation explains how to send **Branch install events to mParticle**. mParticle calls this an Inbound Feed Integration. If you'd like to send mParticle events to your Branch dashboard, please review the Branch/mParticle SDK Kit integration documentation for [iOS](/pages/apps/mparticle-ios/) and [Android](/pages/apps/mparticle-android/). 

### How does it work?

Once the Branch SDK is integrated into an app, Branch can detect which links are leading to installs, re-opens, and users' actions. Enabling this integration and providing your mParticle token will result in Branch automatically forwarding attributed installs to mParticle, in the exact format mParticle expects.

### What events does Branch send?

Branch will send **attributed installs**. Branch also sends all the data that is attached to the link that drove the attributed installs. This will allow you to analyze which campaigns, channels, etc. are helping you acquire and engage users. You can see the list of fields that we send to mParticle [here](#what-branch-sends-to-mparticle).

### What does it look like?

Branch events will appear as an attribution event in mParticle. You can then export Branch data to your other data sources.

Branch attribution events are mapped as follows:

- Event Type = Custom Event
- Custom Event Type = attribution
- Event Name = attribution

![image](/img/pages/integrations/mparticle/mparticle-showing-branch-data.png)

For information on how to test your integration and see more detailed data, please review our [testing instructions below](#testing-your-integration).

## Setup

### Prerequisites
- This guide requires you to have already integrated the Branch and mParticle mobile SDK(s).
- This guide requires you to have the [AdSupportFramework](https://docs.branch.io/pages/apps/ios-launch/#submitting-to-the-app-store) on iOS and [Google Play Services library] (https://docs.branch.io/pages/apps/android-launch/#submitting-to-the-play-store) on Android.

### Retrieve mParticle Token

Find your mParticle Token and enter it into the Branch Dashboard.

1. Navigate to [https://app.mparticle.com](https://app.mparticle.com) and log into the Dashboard.
1. In the dashboard, navigate to [Setup > Inputs](https://app.mparticle.com/setup/inputs). 
1. Click on **iOS** or **Android**.

    ![image](/img/pages/integrations/mparticle/mparticle-settings-inputs.png)

1. Copy your key and secret - you'll enter it into the Branch dashboard in a minute.

    ![image](/img/pages/integrations/mparticle/mparticle-keys.png)


### Configure Branch Dashboard

{! ingredients/data-integrations/enable-data-integrations.md !}

1. On the Branch Dashboard (dashboard.branch.io), navigate to the [Integrations page](https://dashboard.branch.io/integrations).
1. Search for mParticle and click on the tile.
1. Enter your mParticle Token and hit **Enable**.

    ![image](/img/pages/integrations/mparticle/mparticle-branch-settings.png)

    !!! warning "Please test integration!"
        Branch is not responsible for inaccurate API keys.

**Additional mParticle Resources:**

You can find additional information about the Branch and mParticle integration in the mParticle documentation.

## Advanced

### What Branch Sends to mParticle

| Property Name | Value | Sourced from | Example
| --- | --- | --- | --- | --- | ---
| event_name | "attribution" | _hardcoded_ | "attribution"
| custom_event_type | "attribution" | _hardcoded_ | "attribution"
| event_id | Unique ID for the event | Branch install event ID | 469939270182891107
| custom_attributes | Branch Link Data Dictionary | Last Attributed Touch Data for the link to which the install was attributed | "campaign": "mParticle test", "channel": "Slack"
| timestamp_unixtime_ms | timestamp of the event in ms | event | 1513280479654
| device_info | Device Data, like OS Version and country | Branch User Data for the device | "os_version": "10.0","device_country": "US"
| user_identities | mParticle customer ID (mostly empty) | mParticle customer ID for the user | myuser@user.com
| application_info | App information like package name | Branch SDK/App Details | "application_name": "Branch-3rdParty-SDK-Testbed","application_version": "1.2.0"
| IP | IP of the event | Device | 192.82.115.928

## Testing your integration

To see the data being passed to mParticle in more detail, you can set up a webhook to Requestbin. This will allow you to send only a subset of your Branch events to Requestbin and verify the data is coming through as expected. 

1. Enable the Branch Data Integration above. Install your app from a Branch link to generate some data in mParticle. It can take up to an hour for the custom attributes to show in mParticle. 
1. Create a Branch link from the Quick Links section of the dashboard, with a campaign of *branch_test*.

    ![image](/img/pages/integrations/mparticle/mparticle-test-link.png)

1. In mParticle, navigate to the Directory in mParticle and add **Webhooks** as an option. Add a Requestbin URL generated from [https://requestb.in](https://requestb.in).

    ![image](/img/pages/integrations/mparticle/mparticle-add-webhooks.png)

1. To refine the data being sent to the webhook, navigate to **Connections > Connect**. 
1. Select your OS as the input, and **Webhooks** as the output.
1. Set a filter to filter on *campaign* and put the campaign name of *branch_test* that you added in your Branch link. 

    ![image](/img/pages/integrations/mparticle/mparticle-webhook-forwarding-rule.png)

1. Finally, view the Requestbin. You can do this by appending **?inspect** to your requestbin URL. You should be able to copy paste the POST body from the request bin into a JSON formatter like [https://jsonlint.com/](https://jsonlint.com/) to view the event details. 

    ![image](/img/pages/integrations/mparticle/mparticle-requestbin.png)

