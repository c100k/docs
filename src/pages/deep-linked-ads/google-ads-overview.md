---
title: Google Adwords Overview
description: An overview page of using Branch in your Google Adwords campaigns.
path: tree/master/src/pages/deep-linked-ads
source: google-ads-overview.md
---
# Google Adwords Overview

## Overview

With Branch, you can put deep links in every type of Google AdWords campaign, improving conversion rates and letting you measure the impact of your campaigns on mobile.  

The Google AdWords interface can be confusing so we've created a guide to help you find the right documentation. The new AdWords interface still follows roughly the same campaign creation flow. We'll update this page as needed if the campaign creation flow is updated, or new ad types are supported.

Once you have completed set up below, follow each guide to track campaigns.

## Setup

Setting up Adwords conversion events with Branch allows Branch to get direct confirmation from Google for which conversion events were driven by an Adwords advertisement and allows Adwords to collect accurate conversion data for your app.

### Latest Method (OAuth method)

If you are on the new AdWords integration through Branch, read these instructions. Do **not** follow the steps of the legacy instructions. If you would like to try the latest method, send an email to sahil@branch.io. Benefits of the new integration include tracking all Universal App Campaigns automatically (no links required), and sending all conversion events back to AdWords for optimized spend.

#### AdWords Setup

1. Log in to your [AdWords dashboard](https://adwords.google.com/aw/overview){:target="\_blank"} account that has the highest level of access. You will have the ability to connect all your AdWords accounts with Branch.
1. Go to `Settings > Linked Accounts`.
<img src="/img/pages/deep-linked-ads/google/linked-accounts.png" alt="Linked Accounts" class="three-quarters center">
1. Create a new link ID: Go to `Third Party App Analytics > +`.
1. Add a new provider: Select "other" in the drop down. Input the following ID: `3404357870`.
1. Select iOS or Android.
1. Create Link IDs for all platforms you run campaigns on.
<img src="/img/pages/deep-linked-ads/google/link-id.png" alt="Link IDs" class="three-quarters center">

**Note** you must be an admin in your AdWords account in order to generate Link IDs!

#### Branch Setup

Navigate to the [partner management](https://dashboard.branch.io/ads/partner-management){:target="\_blank"} tab and click `Connect with Google`. Choose the email address that is tied to the AdWords accounts you want to connect.

![Connect Google](/img/pages/deep-linked-ads/google/connect-with-google.png)

Select all the necessary accounts, and continue.

![Connect Accounts](/img/pages/deep-linked-ads/google/connect-accounts.png)

Finally, paste the Link IDs from earlier.

![Create Link IDs](/img/pages/deep-linked-ads/google/link-ids.png)

#### Import Events

All that remains is importing Branch events into AdWords. After you have set this up, wait ~ 20 minutes, and go back to the AdWords dashboard.

1. Go to `Conversions`.
<img src="/img/pages/deep-linked-ads/google/conversions.png" alt="Linked Accounts" class="three-quarters center">
1. Add a new conversion: `+ > App > Third Party App Analytics`.
<img src="/img/pages/deep-linked-ads/google/create-conversion.png" alt="Linked Accounts" class="three-quarters center">
1. Import your Branch specific events. Click `Import and Continue`.
1. Mark `Include in Conversions` to `YES`.

That's it! All of your campaigns with mobile conversions will be tracked in Branch's dashboard.

### Legacy (no OAuth option)

If you do not have the option in your Branch dashboard to connect via oauth, you are on the old style of AdWords. If you'd like to use the new option, please see above.

#### Branch Setup
If you do not have the ability to connect to AdWords through OAuth, please follow the instructions below.

Start by going to the [Partner Management tab](https://dashboard.branch.io/ads/partner-management){:target="\_blank"}, and search for Google AdWords. Hit enable.

#### AdWords Setup
Start by going to your AdWords dashboard, and clicking `Tools` > `Conversions`, which is usually found in the top right nav of AdWords.


<img src="/img/pages/deep-linked-ads/google-conversions/adwords-tools-conversion.png" alt="Conversion Menu" class="center half">

Click `+ Add a Conversion` button, and select `App`. Select `First opens and in-app actions`. Select the appropriate platform (iOS or Android). Select `App installs (first-open)` or `In-app actions`. Please note that iOS actions are only available for ads that show on the Display Network.

<img src="/img/pages/deep-linked-ads/google-conversions/adwords-app-conversion-card.png" alt="Conversion IDs" class="center three-quarters">

You will then fill out information about the conversion.

* Give it a name like `Branch Android/iOS Conversion`
* Under `Value` assign a value (or select “Don’t assign a value to this install”)
* Under `Mobile app` input your application details
* Select `Include in "Conversions"` to have the conversion events appear in your Adwords columns

From here, you will select the option to have a server report conversions: `Set up a server-to-server conversion feed...`.

Make reference of the `Conversion ID` & `Conversion label` as shown in the screenshot below.

![Conversion IDs](/img/pages/deep-linked-ads/google-conversions/adwords-conversions.png)

Now navigate back to the Branch dashboard, specifically the [AdWords Settings](https://dashboard.branch.io/ads/partner-management/a_google_adwords?tab=settings){:target="\_blank"}.

From here, you'll paste in the `Conversion ID` and `Conversion label` from your Adwords dashboard into the appropriate fields for either iOS or Android. If you plan on using custom events, please add the conversion ID for your account into the `Purchase or Custom Event Conversion ID` field. Make sure to hit save.

##### Purchase and custom event

1. To add Purchase and other custom event conversions in Google, first [enable the conversions in AdWords](/pages/deep-linked-ads/google-conversions/).

1. Under the Account Settings tab, ensure you have pasted in the *Conversion ID* you generated earlier for the purchase or other event, into the `Purchase or Custom Event Conversion ID` field.

1. Then, navigate to the Postback Config tab and add your *Conversion Label* into the Goal ID field.

![AdWords Goal IDs](/img/pages/deep-linked-ads/google-conversions/aw-custom-goal-ids.png).

You're all setup to confirm app install conversions between Branch and Adwords!

!!! note "Conversion Windows"
	Adwords has a default 30 day conversion window for app install actions which can't be changed. To minimize discrepancies between Branch and Adwords conversion values, we recommend setting your Branch attribution window to the same value.
	Navigate to `Link Settings` > `Attribution Windows` and set the **Click to conversion event** to 30 days.

	**By default, the window is set to 30 days in the Branch dashboard.**

![Branch Conversion Window](/img/pages/deep-linked-ads/google-conversions/attribution-window.png)

## Campaign Type

Now that you've completed set up, you can create links for these type of campaigns. **NOTE** if you've completed the new style of set up, you do **not** create links for Universal App Campaigns below.

Google Campaign | Campaign Type/Objective | Branch Documentation Link | Branch Ad Format
--- | --- | --- | ---
Search Network | Mobile app engagement | **[link](/pages/deep-linked-ads/google-search-engagement-ads/#overview)** | App Only: Engagement
Search Network | Standard  | **[link](/pages/deep-linked-ads/google-xplatform-search-ads/#standard-search-ads)** | Cross-platform Search
Search Network | Dynamic Search Ads  | **[link](/pages/deep-linked-ads/google-xplatform-search-ads/#dynamic-search-ads)** | Cross-platform Search
Display Network | Engage with your mobile app | **[link](/pages/deep-linked-ads/google-display-engagement-ads/#overview)** | App Only: Engagement
Display Network | Others (Visit your website, Influence, etc.)  | **[link](/pages/deep-linked-ads/google-xplatform-display-ads/#overview)** | Cross-platform Display
Video | Mobile App Installs | **[link](/pages/deep-linked-ads/google-video-ads/#video-app-install-ads)** | App Only: Install
Video | Standard | **[link](/pages/deep-linked-ads/google-video-ads/#video-standard-ads)** | Cross-platform Display
Shopping | Web and App Purchases | **[link](/pages/deep-linked-ads/google-shopping-ads/#overview)** | Cross-platform Display
Universal App Campaigns | Universal App Campaigns | **[link](/pages/deep-linked-ads/google-uac/#overview)** | App Only: Install

## Troubleshooting

### FAQ

**Q: I'm getting discrepancy between conversion counts in Branch and Google Adwords**

**A:** While we should always expect around a 5% discrepancy due to time zone differences and the like, if you are seeing significant discrepancies, it could be an indication of a broader problem.

The first thing to do is to make sure your attribution window in Branch lines up with Google. Go to [Link Settings](https://dashboard.branch.io/link-settings), and navigate down to the Attribution Windows section. Here, you should set the attribution window for `click to install`, `click to session start`, and `click to conversion event` to be 30, 90, and 90 days respectively. This aligns with Google's default attribution windows, but if you'd like to make them shorter, feel free.

Another source of discrepancies is the fact that attribution is based upon *click* time in AdWords, whereas it is based upon *install* time in the Branch dashboard. This isn't a discrepancy per se, but will sometimes show different numbers in the two dashboards.

Finally, AdWords can delay reporting up to 24 hours. It's best to measure campaigns in a trailing manner.

**Q: My campaign is reporting a number of conversions much higher than the number of conversions shown in the conversion table in Adwords**

**A:** When viewing a campaign, it shows the sum of all conversion events that apply to it. To view by conversion, navigate to `Segment` > `Conversions` > `Conversion name`, in order to clearly see the breakdown of your campaign's conversions.

<img src="/img/pages/deep-linked-ads/google-conversions/conversion-segment.png" alt="Adwords Conversion Segment" class="center">
