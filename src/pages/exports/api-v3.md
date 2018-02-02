## Overview

Branch’s new Data Export API for [People-Based Attribution](/pages/dashboard/people-based-attribution/) can be used to pull granular Branch event data directly. A few important notes for this API:

- The data will only be retrievable for 7 days
- The data will only appear after end of day since it's processed every 24 hours
- The data will not be visible prior to your whitelisting date
- The data will only be visible for your live key (not your test key)

!!! note "Data Feeds is a premium solution"
    The Data Export API is included in Branch’s [Data Feeds](/pages/exports/data-feeds/) offering, which can be purchased according to Branch’s [pricing schedule](https://branch.io/pricing/){:target="\_blank"}, and is available at no additional charge to customers who are on MAU plans for [Journeys](https://branch.io/journeys/){:target="\_blank"}, [Deep Linked Email](https://branch.io/email/){:target="\_blank"}, or [Universal Ads](https://branch.io/attribution/){:target="\_blank"}. Without Data Feeds, you can still export Branch data in CSV form directly from the Branch dashboard via [Sources](https://dashboard.branch.io/sources){:target="\_blank"} or [CSV Exports](https://dashboard.branch.io/data-import-export/csv-exports){:target="\_blank"}.

    **If you are looking for the legacy Data Export API**, please see [these docs](/pages/exports/api/).

## Setup

In order to use this API you must have an App ID and a Branch Public Key. You must also be whitelisted to use the API. If you'd like to be whitelisted for the Export API, please contact [integrations@branch.io](mailto:integrations@branch.io).

You can find your keys in your [Account Settings](https://dashboard.branch.io/account-settings/app){:target="\_blank"}.

### Base URL:
https://api.branch.io/v3/export/

### Endpoint
* POST https://api.branch.io/v3/export
* Content-Type: application/json

### Query Parameters

**branch_key** _required_
: The Branch key of the originating app

**branch_secret** _required_
: The Branch secret key of the originating app

**export_date** _required_
: The UTC date of the requested data export

**Example request:**

```
curl -X POST api.branch.io/v3/export -H 'content-type:application/json' -d '{"branch_key":"key_live_gcASshuadd7l39m36NhdsDPWRjmkdcgh12jsg1", "branch_secret": "secret_live_ztPsdKIjUtcjkUYF732nsl81HJ75BJqiv24J86", "export_date": "2017-10-01"}'
```

### Response

The response payload will be in JSON format and for each export it will have an array of paths to files on s3 to download. Note that there may be multiple files (depending on the size of the day's export) and that each csv file will be gzipped.


```
{
  "eo_branch_cta_view": ["https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_branch_cta_view-v2-e458d6183650e6401ca017e673d23885abdf9250378ffce377f318a38e4f0017-u6mGkx.csv.gz"],
  "eo_click":[ "https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_click-v2-bbe7351be61c8797c1d80437dffcaa493a495e24a04c25b9131c9c042450319a-7bWLZr.csv.gz"],
  "eo_commerce_event": ["https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_commerce_event-v2-05f56947e843a51d785b8a03a615dfe0dccfda1abd3efad1010d5fae8b2fc19c-oiZ38C.csv.gz"],
  "eo_custom_event": ["https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_custom_event-v2-09fd3f0ff086364a2dc0155118a1b120a2837fd3c56ab250062bcb8f46ab731c-FHgAt3.csv.gz"],
  "eo_install": ["https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_install-v2-9db98c07dccd3d8c8a26a7fec1f1596bb5cdb526eb7dc633f6470a5e75566b33-zdHNVt.csv.gz"],
  "eo_open": ["https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_open-v2-9fafa230fa9f3b0fcef5d00f6bcfae990131d1fcbf497625ee504847c30530b8-3hvp1O.csv.gz"],
  "eo_pageview": ["https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_pageview-v2-9166aaea1e6c46fcf0a48d5a32c02c6b8272d3edb10c332db2ebc0cbb34d58c0-neWTEO.csv.gz"],
  "eo_reinstall": ["https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_reinstall-v2-b4188abf0635915c56be670ad988eab889f084b7d2109e3225a6974a80e2781f-htV3uL.csv.gz"],
  "eo_sms_sent": ["https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_sms_sent-v2-32ebf2131e1538fc180e27df8e5e5fbe8ea89ff5e971811056c449095f35e651-5xay42.csv.gz"],
  "eo_web_session_start": ["https://branch-exports.s3.amazonaws.com/271025641725777235-2017-10-01-eo_web_session_start-v2-056d25078e665dab3d78d1eb00336ad27087da5593ba1003f8ad8923dfce31a0-06wAW2.csv.gz"
}
```

All exports via Data Feeds are powered by Branch's [People-Based Attribution](/pages/dashboard/people-based-attribution/). For an exhaustive list of events included in these exports and more detailed definitions of each event, please see the [Event Ontology Data Schema](/pages/exports/event_ontology_data_schema/).

!!! tip
    A full day's files will be available on our S3 bucket at that location to download around 8:00am UTC. It will return a blank array from s3 for any empty files until the UTC day is over and the data has been transfered to s3, therefore it is recommended you schedule any ETLs to fetch the data for the previous day around 8:00am UTC.

## Support

### Time Limits

Data will be available through the API for 7 days after the date it's posted. It will be also available for 90 days after that upon request. After 90 days, we may delete the data to reduce unnecessary storage. If you need a record of your data for longer than 90 days, please set up a recurring export and store data in your systems.
