## Overview

The Branch partnership with [Braze](https://www.braze.com) provides a push-button way to deliver Branch installs and attributions to your Braze dashboard. This allows you to analyze your users coming in from Branch deep linked campaigns.

**At this time, our integration only applies to the iOS platform.**

### How it works

We have built a custom integration to automatically send all Branch install data to Braze without any extra work on your side (besides integrating both SDKs). Simply click a button, and you'll be good to go!

!!! protip "How do we differentiate Braze and Branch installs?"
    We rely on a Branch link being clicked, which leads to an install. This sets an internal boolean that an install came from Branch.

## Setup

### Prerequisites

- This guide requires you to have already integrated the Branch mobile SDKs into your app.
- You also need to [sign up for an Braze account](https://dashboard.braze.com/developers/sign_up) and [install the Braze SDK](https://documentation.braze.com/).
- Ensure Braze's SDK is [collecting the IDFA](https://documentation.braze.com/iOS/#optional-idfa-collection).
- Make sure to follow the steps in Braze's [documentation here](https://www.braze.com/documentation/Partner_Integrations/#branch).

### Get the Braze API key

1. On the Braze dashboard, navigate to the **App Settings** section, and click **3rd Party Integrations**.
1. From there, grab your API key (this will be the same for all attribution partners listed on the page).


### Configure the Branch Dashboard

1. On the Branch Dashboard (dashboard.branch.io), navigate to the [Integrations page](https://dashboard.branch.io/integrations).
1. Locate Braze and choose **Enable**.
  * If you have not yet entered billing information, please do so now.
1. Enter your Braze iOS API Key and hit **Save**.

![image](/img/pages/integrations/braze/braze.jpg)

## Advanced

### What Branch sends to Braze

Branch Analytics Tag | Braze Data Placeholder Tag
--- | ---
Campaign | campaign
Channel | adgroup
