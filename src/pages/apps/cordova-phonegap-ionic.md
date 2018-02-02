## Integrate Branch

!!! warning "Inconsistent Universal links behavior on iOS 11.2"
    After updating a device to iOS 11.2, we found that the app's AASA file is no longer downloaded reliably onto your user’s device after an app install. As a result, clicking on Universal links will no longer open the app consistenly. You can set [forced uri redirect mode](/pages/links/integrate/#forced-redirections) on your Branch links to open the app with URI schemes. View details of the issue on the [Apple Bug report](http://www.openradar.me/radar?id=4999496467480576).

- ### Configure Branch

    - Complete your [Branch Dashboard](https://dashboard.branch.io/settings/link)

        ![image](/img/pages/apps/cordova-configure.png)
        ![image](/img/pages/apps/cordova-link-domain.png)

- ### Configure app

     - *Cordova and Ionic*

        ```xml
        <!-- sample config.xml -->
        <widget id="com.eneff.branch.cordovatestbed" version="0.0.1" xmlns="http://www.w3.org/ns/widgets" xmlns:cdv="http://cordova.apache.org/ns/1.0">
          <!-- Branch -->
          <plugin name="branch-cordova-sdk" spec="^2.5.0" />
          <branch-config>
            <branch-key value="key_live_ndqptlgXNE4LHqIahH1WIpbiyFlb62J3" />
            <uri-scheme value="branchcordova" />
            <link-domain value="cordova.app.link" />
            <ios-team-release value="PW4Q8885U7" />
          </branch-config>
        ```

    - *PhoneGap*

        ```xml
        <!-- sample config.xml -->
        <widget id="com.eneff.branch.cordovatestbed" version="1.0.0" xmlns="http://www.w3.org/ns/widgets" xmlns:gap="http://phonegap.com/ns/1.0">
          <!-- Branch -->
          <plugin name="branch-cordova-sdk" spec="^2.5.0" />
          <branch-config>
            <branch-key value="key_live_ndqptlgXNE4LHqIahH1WIpbiyFlb62J3" />
            <uri-scheme value="branchcordova" />
            <link-domain value="cordova.app.link" />
            <ios-team-release value="PW4Q8885U7" />
          </branch-config>
        ```

    - Change the following values to match your [Branch Dashboard](https://dashboard.branch.io/settings/link)

        - `com.eneff.branch.cordovatestbed`
        - `key_live_ndqptlgXNE4LHqIahH1WIpbiyFlb62J3`
        - `branchcordova`
        - `cordova.app.link`
        - `PW4Q8885U7`

- ### Initialize Branch

    - *Cordova and PhoneGap*

        ```js hl_lines="11 14 16 17 18 19 20 21 22"
        // sample index.js
        var app = {
          initialize: function() {
            this.bindEvents();
          },
          bindEvents: function() {
            document.addEventListener('deviceready', this.onDeviceReady, false);
            document.addEventListener('resume', this.onDeviceResume, false);
          },
          onDeviceReady: function() {
            app.handleBranch();
          },
          onDeviceResume: function() {
            app.handleBranch();
          },
          handleBranch: function() {
            // Branch initialization
            Branch.initSession(function(data) {
              if (data['+clicked_branch_link']) {
                // read deep link data on click
                alert('Deep Link Data: ' + JSON.stringify(data));
              }
            });
          }
        };

        app.initialize();
        ```

    - *Ionic 1*

        ```js hl_lines="16 20 23 24 25 26 27 28 29"
        // sample app.js
        angular.module('starter', ['ionic', 'starter.controllers', 'starter.services'])

        .run(function($ionicPlatform) {
          $ionicPlatform.ready(function() {
            if (window.cordova && window.cordova.plugins && window.cordova.plugins.Keyboard) {
              cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
              cordova.plugins.Keyboard.disableScroll(true);
            }
            if (window.StatusBar) {
              StatusBar.styleDefault();
            }

            // Branch
            $ionicPlatform.on('deviceready', function() {
              handleBranch();
            });

            $ionicPlatform.on('resume', function() {
              handleBranch();
            });

            function handleBranch() {
              // Branch initialization
              Branch.initSession(function(data) {
                if (data['+clicked_branch_link']) {
                  // read deep link data on click
                  alert('Deep Link Data: ' + JSON.stringify(data));
                }
              });
            }
          });
        })
        // ...
        ```

    - *Ionic 2 and 3*

          ```java hl_lines="18 22 25 26 27 28 29 30 31 32 33 34 35 36"
          // sample app.component.js
          import { Component } from '@angular/core';
          import { Platform } from 'ionic-angular';
          import { StatusBar, Splashscreen } from 'ionic-native';

          import { TabsPage } from '../pages/tabs/tabs

          @Component({
            template: `<ion-nav [root]="rootPage"></ion-nav>`
          })
          export class MyApp {
            rootPage = TabsPage;

            constructor(platform: Platform) {
              platform.ready().then(() => {
                StatusBar.styleDefault();
                Splashscreen.hide();
                handleBranch();
              });

              platform.resume.subscribe(() => {
                handleBranch();
              });

              // Branch initialization
              const handleBranch = () => {
                // only on devices
                if (!platform.is('cordova')) { return }
                const Branch = window['Branch'];
                Branch.initSession(data => {
                  if (data['+clicked_branch_link']) {
                    // read deep link data on click
                    alert('Deep Link Data: ' + JSON.stringify(data));
                  }
                });
              }
            }
          }
          ```

- ### Test deep link iOS

    - Create a deep link from the [Branch Dashboard](https://dashboard.branch.io/marketing)

    - Delete your app from the device

    - Compile your app *(`cordova run ios` `phonegap run ios` `ionic run ios`)*

    - Paste deep link in `Apple Notes`

    - Long press on the deep link *(not 3D Touch)*

    - Click `Open in "APP_NAME"` to open your app *([example](/img/pages/apps/ios-notes.png))*

- ### Test deep link Android

    - Create a deep link from the [Branch Dashboard](https://dashboard.branch.io/marketing)

    - Delete your app from the device

    - Compile your app *(`cordova run android` `phonegap run android` `ionic run android`)*

    - Paste deep link in `Google Hangouts`

    - Click on the deep link to open your app

## Implement features

- ### Initialize Branch features

    - Loads Branch into your app

    - Must be called on `deviceready` and `resume`

        ```js
        // for development and debugging only
        Branch.setDebug(true)

        // Branch initialization
        Branch.initSession(function(data) {
          if (data['+clicked_branch_link']) {
            // read deep link data on click
            alert('Deep Link Data: ' + JSON.stringify(data))
          }
        })
        ```

- ### Create content reference

    - The `Branch Universal Object` encapsulates the thing you want to share (content or user)

    - Uses the [Universal Object Properties](#/pages/links/integrate/#universal-object)

        ```js
        // only canonicalIdentifier is required
        var properties = {
          canonicalIdentifier: 'content/123',
          canonicalUrl: 'https://example.com/content/123',
          title: 'Content 123 Title',
          contentDescription: 'Content 123 Description ' + Date.now(),
          contentImageUrl: 'http://lorempixel.com/400/400/',
          price: 12.12,
          currency: 'GBD',
          contentIndexingMode: 'private',
          contentMetadata: {
            custom: 'data',
            testing: 123,
            this_is: true
          }
        }

        // create a branchUniversalObj variable to reference with other Branch methods
        var branchUniversalObj = null
        Branch.createBranchUniversalObject(properties).then(function (res) {
          branchUniversalObj = res
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

- ### Create deep link

    - Creates a deep link URL with encapsulated data

    - Needs a [Branch Universal Object](#create-content-reference)

    - Uses [Deep Link Properties](/pages/links/integrate/)

    - Validate with the [Branch Dashboard](https://dashboard.branch.io/liveview/links)

        ```js
        // optional fields
        var analytics = {
          channel: 'facebook',
          feature: 'onboarding',
          campaign: 'content 123 launch',
          stage: 'new user',
          tags: ['one', 'two', 'three']
        }

        // optional fields
        var properties = {
          $desktop_url: 'http://www.example.com/desktop',
          $android_url: 'http://www.example.com/android',
          $ios_url: 'http://www.example.com/ios',
          $ipad_url: 'http://www.example.com/ipad',
          $match_duration: 2000,
          custom_string: 'data',
          custom_integer: Date.now(),
          custom_boolean: true
        }

        branchUniversalObj.generateShortUrl(analytics, properties).then(function (res) {
          alert('Response: ' + JSON.stringify(res.url))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

- ### Share deep link

    -  Will generate a Branch deep link and tag it with the channel the user selects

    - Needs a [Branch Universal Object](#create-content-reference)

    - Uses [Deep Link Properties](/pages/links/integrate/)

        ```js
        // optional fields
        var analytics = {
          channel: 'facebook',
          feature: 'onboarding',
          campaign: 'content 123 launch',
          stage: 'new user',
          tags: ['one', 'two', 'three']
        }

        // optional fields
        var properties = {
          $desktop_url: 'http://www.example.com/desktop',
          custom_string: 'data',
          custom_integer: Date.now(),
          custom_boolean: true
        }

        var message = 'Check out this link'

        // optional listeners (must be called before showShareSheet)
        branchUniversalObj.onShareSheetLaunched(function (res) {
          // android only
          console.log(res)
        })
        branchUniversalObj.onShareSheetDismissed(function (res) {
          console.log(res)
        })
        branchUniversalObj.onLinkShareResponse(function (res) {
          console.log(res)
        })
        branchUniversalObj.onChannelSelected(function (res) {
          // android only
          console.log(res)
        })

        // share sheet
        branchUniversalObj.showShareSheet(analytics, properties, message)
        ```

- ### Read deep link

    - Retrieve Branch data from a deep link

    - Best practice to receive data from the `listener` (to prevent a race condition)

    - Returns [deep link properties](/pages/links/integrate/#read-deep-links)

    - Listener

        ```js
        // Branch initialization within your deviceready and resume
        Branch.initSession(function(deepLinkData) {
          if (data['+clicked_branch_link']) {
            // handler for deep link data on click
            alert('Response: ' + JSON.stringify(deepLinkData))
          }
        })
        ```

    - Latest data

        ```js
        Branch.getLatestReferringParams().then(function(res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function(err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

    - First data

        ```js
        Branch.getFirstReferringParams().then(function(res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function(err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

- ### Navigate to content

    - Handled within `Branch.initSession()`

    - Branch allows you to pass any custom key-value from URLs to your app. Use this data to navigate to content, display a personalized welcome screen, login a user, offer a promotion, etc.

        ```js
        Branch.initSession(function(data) {
          if (data['+clicked_branch_link']) {
            // option 1: save to model to be used later
            window.localStorage['branchData'] = data;

            // option 2: navigate to page
            window.location.href = '#/content/123'

            // option 3: display data
            alert(JSON.stringify(data));
          }
        });
        ```

- ### Display content

    - List content on `iOS Spotlight`

    - Needs a [Branch Universal Object](#create-content-reference)

        ```js
        branchUniversalObj.listOnSpotlight().then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

- ### Track content

    - Track how many times a piece of content is viewed

    - Needs a [Branch Universal Object](#create-content-reference)

    - Validate with the [Branch Dashboard](https://dashboard.branch.io/liveview/content)

        ```js
        branchUniversalObj.registerView().then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

- ### Track users

    - Sets the identity of a user (email, ID, UUID, etc) for events, deep links, and referrals

    - Validate with the [Branch Dashboard](https://dashboard.branch.io/liveview/identities)

        ```js
        var userId = '123456'
        Branch.setIdentity(userId).then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err.message))
        })
        ```

    - Removes the identity of a user

        ```js
        Branch.logout().then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err.message))
        })
        ```

- ### Track events

    - Registers a custom event

    - Events named `open`, `close`, `install`, and `referred session` are Branch restricted

    - Best to [Track users](#track-users) before [Track events](#track-events) to associate a custom event to a user

    - Validate with the [Branch Dashboard](https://dashboard.branch.io/liveview/events)

        ```js
        var eventName = 'clicked_on_this'
        var metadata = { 'custom_dictionary': 123, 'anything': 'everything' }
        Branch.userCompletedAction(eventName, metaData).then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err.message))
        })
        ```

        ```js
        var eventName = 'clicked_on_this'
        Branch.userCompletedAction(eventName).then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err.message))
        })
        ```

- ### Track commerce

    - Registers a custom commerce event

    - Uses [Track commerce properties](#commerce-properties) for `Currency` and `Category`

    - Validate with the [Branch Dashboard](https://dashboard.branch.io/liveview/commerce)

        ```js
        // only revenue is required
        var event = {
          'revenue': 50.29,
          'currency': 148, // USD
          'transactionID': 'transaction id',
          'coupon': 'coupon',
          'shipping': 2.22,
          'tax': 5.11,
          'affiliation': 'affiliation',
          'products': [
            {
              'sku': 'u123',
              'name': 'cactus',
              'price': 4.99,
              'quantity': 2,
              'brand': 'brand',
              'category': 17, // Software
              'variant': 'variant'
            },
            {
              'sku': 'u456',
              'name': 'grass',
              'price': 0.00,
              'quantity': 1
            }
          ]
        }

        // optional fields
        var metadata = {
          'custom_dictionary': 123,
          'anything': 'everything'
        }

        Branch.sendCommerceEvent(event, metadata).then(function (res) {
          console.log(res)
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          console.error(err)
          alert('Error: ' + JSON.stringify(err.message))
        })
        ```

- ### Handle referrals

    - Referral points are obtained from referral rules on the [Branch Dashboard](https://dashboard.branch.io/referrals/rules)

    - Validate on the [Branch Dashboard](https://dashboard.branch.io/referrals/analytics)

    - Reward credits

        -  [Referral guide](/pages/dashboard/analytics/#referrals)

    - Redeem credits

        ```js
        var amount = 10
        var bucket = 'this_bucket'
        Branch.redeemRewards(amount, bucket).then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

        ```js
        var amount = 10
        Branch.redeemRewards(amount).then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

    - Load credits

        ```js
        var bucket = 'this_bucket'
        Branch.loadRewards(bucket).then(function(res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function(err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

        ```js
        Branch.loadRewards().then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

    - Load history

        ```js
        Branch.creditHistory().then(function (res) {
          alert('Response: ' + JSON.stringify(res))
        }).catch(function (err) {
          alert('Error: ' + JSON.stringify(err))
        })
        ```

## Troubleshoot issues

- ### Recommendations

    - Need to select `"app uses IDFA or GAID"` when publishing your app

    - Best to enable [Deepviews](https://dashboard.branch.io/settings/deepviews) ([Supported platforms](/pages/links/behavior/#expected-redirect-behavior))

- ### Optional app config

    - Additional configuration for custom link domains, simulating installs, unique bundle identifiers, etc

        ```xml
        <!-- sample config.xml -->
        <widget id="com.eneff.branch.cordovatestbed" version="1.0.0" xmlns="http://www.w3.org/ns/widgets" xmlns:cdv="http://cordova.apache.org/ns/1.0">
          <!-- Branch -->
          <plugin name="branch-cordova-sdk" spec="~2.4.2" /> <!-- optional spec -->
          <branch-config>
            <branch-key value="key_live_ndqptlgXNE4LHqIahH1WIpbiyFlb62J3" />
            <uri-scheme value="branchcordova" />
            <link-domain value="yourcustomdomain.com" />
            <link-domain value="cordova.app.link" />  <!-- optional previous link domain -->
            <link-domain value="bnc.lt" />  <!-- optional previous link domain -->
            <ios-team-release value="PW4Q8885U7" /> <!-- required if iOS app -->
            <ios-team-debug value="FG35JLLMXX" /> <!-- optional -->
            <android-prefix value="/WSuf" /> <!-- optional (for bnc.lt and custom domains) -->
            <android-testmode value="true" /> <!-- optional (simulate installs) -->
          </branch-config>
        ```

        ```xml
        <widget ios-CFBundleIdentifier="com.eneff.branch.cordovatestbedios" android-packageName="com.eneff.branch.cordovatestbedandroid" version="1.0.0" xmlns="http://www.w3.org/ns/widgets" xmlns:cdv="http://cordova.apache.org/ns/1.0">
        ```

- ### Sync with Mixpanel

    - Sync Branch withh Mixpanel if the plugin is installed 
    
    - Add before `Branch.initSession();` ([Initialize Branch Features](#initialize-branch-features))
 
        ```js
        Branch.setRequestMetadata("$mixpanel_distinct_id", "123")
        ```

- ### Simulate an install

    - Delete your app

    - *[iOS]* iPhone Device -> Settings -> Privacy -> Advertising -> Reset Advertising Identifier -> Reset Identifier

    - *[Android]* Add `<android-testmode value="true" />` to your `Config.xml` ([Testing: Optional App Config](#testing-optional-app-config))

    - Add `Branch.setDebug(true);` before `Branch.initSession();` ([Initialize Branch Features](#initialize-branch-features))

    - Click on a deep link to navigate to your `$fallback_url` because your app is not installed

    - Install your app

    - Open your app

    - Read from `Branch.initSession(data)` for `+is_first_session = true`

- ### Sample app

    - [Branch testbed app](https://github.com/BranchMetrics/cordova-ionic-phonegap-branch-deep-linking/tree/master/testbed)

- ### Universal Object properties

    - For [Create content reference](#create-content-reference)

        | Key | Default | Usage | Link Property
        | --- | :-: | --- | :-:
        | canonicalIdentifier | | **(Required)** This is the unique identifier for content that will help Branch dedupe across many instances of the same thing. Suitable options: a website with pathing, or a database with identifiers for entities | `$canonical_identifier`
        | canonicalUrl | | The canonical URL, used for SEO purposes | `$canonical_url`
        | title | | The name for the piece of content | `$og_title`
        | contentDescription | | A description for the content | `$og_description`
        | contentImageUrl | | The image URL for the content. Must be an absolute path | `$og_image_url `
        | price | | The price of the item | `$amount`
        | currency | | The currency representing the price in ISO 4217 currency code | `$currency`
        | contentIndexingMode | `"public"` | Can be set to either `"public"` or `"private"`. Public indicates that you’d like this content to be discovered by other apps. | `$publicly_indexable`
        | contentMetadata | | Any custom key-value data e.g. `{ "custom": "data" }`

- ### Commerce properties

    - For [Track commerce](#track-commerce)

    - Categories

        | Value | Category | Value | Category |
        | --- | --- | --- | --- |
        | 0 | Animals & Pet Supplies | 11 | Home & Garden |
        | 1 | Apparel & Accessories | 12 | Luggage & Bags |
        | 2 | Arts & Entertainment | 13 | Mature |
        | 3 | Baby & Toddler | 14 | Media |
        | 4 | Business & Industrial | 15 | Office Supplies |
        | 5 | Camera & Optics | 16 | Religious & Ceremonial |
        | 6 | Electronics | 17 | Software |
        | 7 | Food, Beverage & Tobacco | 18 | Sporting Goods |
        | 8 | Furniture | 19 | Toys & Games |
        | 9 | Hardware | 20 | Vehicles & Parts |
        | 10 | Health & Beauty | | |

    - Currencies

        | Value | Currency | Value | Currency | Value | Currency |
        | --- | --- | --- | --- | --- | --- |
        | 0  | AED | 60 | HKD | 120 | RSD |
        | 1  | AFN | 61 | HNL | 121 | RUB |
        | 2  | ALL | 62 | HRK | 122 | RWF |
        | 3  | AMD | 63 | HTG | 123 | SAR |
        | 4  | ANG | 64 | HUF | 124 | SBD |
        | 5  | AOA | 65 | IDR | 125 | SCR |
        | 6  | ARS | 66 | ILS | 126 | SDG |
        | 7  | AUD | 67 | INR | 127 | SEK |
        | 8  | AWG | 68 | IQD | 128 | SGD |
        | 9  | AZN | 69 | IRR | 129 | SHP |
        | 10 | BAM | 70 | ISK | 130 | SLL |
        | 11 | BBD | 71 | JMD | 131 | SOS |
        | 12 | BDT | 72 | JOD | 132 | SRD |
        | 13 | BGN | 73 | JPY | 133 | SSP |
        | 14 | BHD | 74 | KES | 134 | STD |
        | 15 | BIF | 75 | KGS | 135 | SYP |
        | 16 | BMD | 76 | KHR | 136 | SZL |
        | 17 | BND | 77 | KMF | 137 | THB |
        | 18 | BOB | 78 | KPW | 138 | TJS |
        | 19 | BOV | 79 | KRW | 139 | TMT |
        | 20 | BRL | 80 | KWD | 140 | TND |
        | 21 | BSD | 81 | KYD | 141 | TOP |
        | 22 | BTN | 82 | KZT | 142 | TRY |
        | 23 | BWP | 83 | LAK | 143 | TTD |
        | 24 | BYN | 84 | LBP | 144 | TWD |
        | 25 | BYR | 85 | LKR | 145 | TZS |
        | 26 | BZD | 86 | LRD | 146 | UAH |
        | 27 | CAD | 87 | LSL | 147 | UGX |
        | 28 | CDF | 88 | LYD | 148 | USD |
        | 29 | CHE | 89 | MAD | 149 | USN |
        | 30 | CHF | 90 | MDL | 150 | UYI |
        | 31 | CHW | 91 | MGA | 151 | UYU |
        | 32 | CLF | 92 | MKD | 152 | UZS |
        | 33 | CLP | 93 | MMK | 153 | VEF |
        | 34 | CNY | 94 | MNT | 154 | VND |
        | 35 | COP | 95 | MOP | 155 | VUV |
        | 36 | COU | 96 | MRO | 156 | WST |
        | 37 | CRC | 97 | MUR | 157 | XAF |
        | 38 | CUC | 98 | MVR | 158 | XAG |
        | 39 | CUP | 99 | MWK | 159 | XAU |
        | 40 | CVE | 100 | MXN | 160 | XBA |
        | 41 | CZK | 101 | MXV | 161 | XBB |
        | 42 | DJF | 102 | MYR | 162 | XBC |
        | 43 | DKK | 103 | MZN | 163 | XBD |
        | 44 | DOP | 104 | NAD | 164 | XCD |
        | 45 | DZD | 105 | NGN | 165 | XDR |
        | 46 | EGP | 106 | NIO | 166 | XFU |
        | 47 | ERN | 107 | NOK | 167 | XOF |
        | 48 | ETB | 108 | NPR | 168 | XPD |
        | 49 | EUR | 109 | NZD | 169 | XPF |
        | 50 | FJD | 110 | OMR | 170 | XPT |
        | 51 | FKP | 111 | PAB | 171 | XSU |
        | 52 | GBP | 112 | PEN | 172 | XTS |
        | 53 | GEL | 113 | PGK | 173 | XUA |
        | 54 | GHS | 114 | PHP | 174 | XXX |
        | 55 | GIP | 115 | PKR | 175 | YER |
        | 56 | GMD | 116 | PLN | 176 | ZAR |
        | 57 | GNF | 117 | PYG | 177 | ZMW |
        | 58 | GTQ | 118 | QAR |     |     |
        | 59 | GYD | 119 | RON |     |     |

- ### Cordova dependencies

    - Node

        ```sh
        /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)";
        brew update;
        brew install node;
        ```

    - Xcode

        - Install [Xcode](https://developer.apple.com/download/)

        - Open Xcode -> agree to SDK license agreement

        - Open Xcode -> Create new Xcode project -> Run simulator -> Agree to developer mode on mac

    - Android Studio

        - Read [instructions](https://developer.android.com/studio/install.html)

        - Install [JVM](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

        - Install [Android Studio](https://developer.android.com/studio/index.html)

        - Open Android Studio -> configure -> appearance/system settings/android sdk -> android 6.0 -> Okay

        - Open Android Studio -> New project -> ... -> Run -> Create new emulator -> Nexus 6p 23 -> Finish

            ```sh
            # add to ~/.bash_profile
            export ANDROID_HOME=$HOME/Library/Android/sdk
            export PATH=$ANDROID_HOME/tools:$PATH
            export PATH=$ANDROID_HOME/platform-tools:$PATH
            ```

            ```sh
            source ~/.bash_profile;
            ```

            ```sh
            android update sdk;
            ```

        - Install Android SDK build-tools 24.0.1

        - Generate Android Keystore

            ```sh
            keytool -genkeypair -dname "cn=Full Name, ou=Business Unit, o=Company, c=US" -alias release -keypass aaa111 -keystore release.keystore -storepass aaa111 -validity 10000
            keytool -list -v -keystore release.keystore
            ```

    - Genymotion *[optional]*

        - Install [Virtual Box](https://www.virtualbox.org/wiki/Downloads)

        - Install [Genymotion](https://www.genymotion.com/download/)

        - Genymotion -> Add virtual device -> Google Nexus 6P - 6.0.0 - API 23 -> Next

- ### Display console logs

    - iOS Simulator

        - `cordova run ios;`

        - Safari -> Preferences -> Advance -> Show Develop menu in menu bar

        - Safari -> Develop -> Simulator -> index.html -> Console

        - *May need to unplug and replug device*

        - *May need to open Xcode and update provisioning profile*

    - iOS Xcode

        - `cordova plugin add cordova-plugin-console;`

        - `cordova build ios;`

        - Xcode -> APP_LOCATION/platforms/ios/APP_NAME.Xcodeproj

        - Xcode -> App -> General -> Signing -> Team

        - Xcode -> Product -> Run

        - Xcode -> View -> Debug Area -> Activate Console

    - Android Device

        - Plug device in

        - `cordova run android;`

        - Chrome -> [chrome://inspect/#devices](chrome://inspect/#devices) -> Console

    - Android Genymotion

        - Genymotion -> Start

        - `cordova run android;`

        - Chrome -> [chrome://inspect/#devices](chrome://inspect/#devices) -> Console

- ### Update the Branch SDK

    - To get the latest improvements and capabilities

        ```bash
        # terminal
        cordova plugin remove io.branch.sdk
        cordova plugin remove branch-cordova-sdk
        ```

        ```xml
        <!-- config.xml -->
        <plugin name="branch-cordova-sdk" spec="^2.5.0" />
        ```
        
    - [Test Deep Link iOS](#test-deep-link-ios)
    - [Test Deep Link Android](#test-deep-link-android)

- ### Incompatibilities

    - The following plugins will not work with the Branch SDK

        - [PhoneGap NFC Plugin](https://github.com/chariotsolutions/phonegap-nfc)

        - [Custom URL scheme](https://github.com/EddyVerbruggen/Custom-URL-scheme)

        - [Cordova Universal Links Plugin](https://github.com/nordnet/cordova-universal-links-plugin)

        - [Ionic Deeplinks Plugin](https://github.com/driftyco/ionic-plugin-deeplinks)

    - PhoneGap Build is also not supported by the Branch SDK because we need plugin hooks to enable Entitlements, Universal Links, App Links, and URI Scheme redirects but PhoneGap Build does not allow plugin hooks

- ### Cordova errors

    - ##### Device only

        - Error

            ```sh
            ORIGINAL EXCEPTION: Branch is not defined
            ```

            ```sh
            ReferenceError: Branch is not defined
            ```

        - Solution

            - Branch opens and installs your app, so you cannot simulate Branch in the desktop browser or simulator

            ```js
            // Ionic 2/3 - running on browser instead of device
            if (!platform.is('cordova')) { return }
            Branch.userCompletedAction('did_this')
            ```

            ```js
            // Ionic 2/3 - missing Branch import
            const Branch = window['Branch'];
            ```

    - ##### Provisioning Profile missing

        - Error

            ```sh
            ** ARCHIVE FAILED **

            The following build commands failed:
              Check dependencies
            (1 failure)
            Error: Error code 65 for command: xcodebuild with args: -xcconfig,cordova/build-debug.xcconfig,-workspace,Branch Testing.xcworkspace,-scheme,Branch Testing,-configuration,Debug,-destination,generic/platform=iOS,-archivePath,Branch Testing.xcarchive,archive,CONFIGURATION_BUILD_DIR=build/device,SHARED_PRECOMPS_DIR=build/sharedpch
            ```

            ```sh
            No profiles for 'com.eneff.branch.cordova_testbed' were found
            ```

        - Solution

            - Fix by opening your app in `Xcode` and launch from there (to select a `Provisioning Profile`)

    - ##### Invalid bundle id

        - Error

            ```sh
            An invalid value 'XC com eneff branch cordova_testbed' was provided for the parameter 'appIdName'.
            ```

            ```sh
            Error: Error code 1 for command: /gradlew with args: cdvBuildDebug,-b,/build.gradle,-Dorg.gradle.daemon=true,-Pandroid.useDeprecatedNdk=true
            ```

        - Solution

            - Don't use `cordova`, `hyphens` (Android), or `underscores` (iOS) in your bundle id (widget id)
